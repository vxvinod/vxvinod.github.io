## Where To Use

- Manual test cases in TestRail can be easily converted into user prompts, which will drive the automation.
- System testing with integration layers.

---

## 8. Explaining This Architecture (Interview-Style Walkthrough)

When explaining this system, it helps to walk through it as **problem → architecture → tradeoffs**, not just a feature list.

**Problem:** Testing network devices meant engineers writing a new script for every scenario - create WLAN, create AP group, add device, verify config. Slow and repetitive. The goal was to let engineers describe the test in plain English and have the system execute and verify it.

**Architecture, left to right:**

```
Test Prompt (NL text, pulled from TestRail)
        |
        v
   LangGraph StateGraph  ---------------+
        |                               |
   +----v-----+   1. RAG retrieval      |  Rules/context
   |  Agent   |<------------------------>|  Vector DB
   |  (LLM)   |   (onboarding rules,       (Chroma/FAISS)
   +----+-----+    device pool rules)
        | 2. tool_calls decided by LLM
        v
   +----------+   3. tools bound via MCP Client
   | ToolNode |------------------------------+
   +----+-----+                              |
        | loop (ReAct: reason->act->observe) v
        |                            +---------------+
        |                            |  MCP Server(s) |
        |                            |  NBI/SBI, Device|
        |                            |  Pool, Device   |
        |                            +-------+--------+
        |                                    | calls real
        |                            CnMaestro API / Device CLI
        v
  Post-Onboarding -> Verify -> Report -> END
```

**Why each piece exists** - this is what interviewers actually probe:

| Component | Why not the "obvious" simpler alternative |
|---|---|
| **LangGraph** (not a plain LLM call, not a linear chain) | Testing is inherently a loop: decide, act, observe, decide again. Also need conditional branches (if WLAN creation fails, don't proceed to AP group) and durable state across many tool calls - a plain chain can't cycle or hold state cleanly. |
| **MCP (client + server)** instead of hardcoding API calls in the agent | Decouples "what tools exist" from "how the LLM uses them." The NBI/SBI server is auto-generated from `openapi.json` - add an endpoint to the API, it becomes a tool automatically, no agent code change. Also lets you swap or add device-pool / CLI servers independently. |
| **Vector DB / RAG** for rules, not prompt-stuffing everything | Onboarding rules differ by device type (WiFi vs NSE vs CnMatrix) and grow over time. Stuffing every rule into every prompt wastes tokens and confuses the model. Semantic retrieval pulls only the relevant rule set for the scenario at hand. |
| **Device Pool as a separate MCP server** with lock/release | Prevents two parallel test containers from grabbing the same physical device - a concurrency problem as much as an AI problem. |
| **Separate Verify node from the Agent** | Never let the LLM self-report success. Verification runs a real CLI command (`show clock`, `service show config`) and does a deterministic match - keeps the system honest and auditable. |

**Tradeoffs worth naming out loud:**

- LLM tool-selection is non-deterministic - mitigated with strict rule injection (RAG) and a hard-fail verify step, not by trusting the LLM's judgment alone.
- Latency: each ReAct iteration is a round trip to the LLM. For N tool calls that's N+1 LLM calls - acceptable for test automation, not for a low-latency user-facing product.
- Every run is traced (LangSmith) so failures are debuggable - important for production maturity, not just a demo.

---

## 9. Key Code Snippets

### Vector DB (RAG store) - indexing and retrieving onboarding rules

```python
# vector_store.py
import chromadb
from chromadb.utils import embedding_functions

client = chromadb.PersistentClient(path="./onboarding_rules_db")
embedder = embedding_functions.GoogleGenerativeAiEmbeddingFunction(api_key=API_KEY)

collection = client.get_or_create_collection(
    name="device_onboarding_rules",
    embedding_function=embedder,
)

def index_rule(doc_id: str, text: str, metadata: dict):
    collection.upsert(ids=[doc_id], documents=[text], metadatas=[metadata])

# one-time load
index_rule(
    "wifi_onboarding_v1",
    text="""A WLAN must be created before adding an Access Point or AP Group.
             An APGroup must be linked to a WLAN before assigning it to a device.
             Devices must be added only after the APGroup is created.""",
    metadata={"device_type": "wifi"},
)

def retrieve_rules(query: str, device_type: str, k: int = 3) -> list[str]:
    results = collection.query(
        query_texts=[query],
        n_results=k,
        where={"device_type": device_type},
    )
    return results["documents"][0]
```

### RAG injection into the LLM context

```python
# rag_context.py
def build_context_prompt(user_prompt: str, device_type: str) -> str:
    rules = retrieve_rules(user_prompt, device_type)
    return f"""Relevant onboarding rules (must be strictly followed):
{chr(10).join(f'- {r}' for r in rules)}

User test scenario:
{user_prompt}
"""
```

### MCP Server - exposing device/API operations as tools

```python
# mcp_nbi_server.py
from mcp.server.fastmcp import FastMCP
import requests

mcp = FastMCP("cnmaestro-nbi-server")

@mcp.tool()
def post_wifi_enterprise_ap_groups(payload_path: str) -> dict:
    """Create a WiFi AP Group from a JSON payload file."""
    payload = json.load(open(payload_path))
    resp = requests.post(f"{CNMAESTRO_URL}/api/v2/aps_groups", json=payload)
    resp.raise_for_status()
    return resp.json()

@mcp.tool()
def add_device(model: str, ip: str, serial: str) -> dict:
    """Register a device to the CnMaestro account."""
    resp = requests.post(f"{CNMAESTRO_URL}/api/v2/devices",
                          json={"model": model, "ip": ip, "serial": serial})
    return resp.json()

if __name__ == "__main__":
    mcp.run(transport="stdio")   # or "sse" for network transport
```

```python
# mcp_device_pool_server.py
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("device-pool-server")

@mcp.tool()
def filter_device_by_model(model: str) -> dict:
    """Find an idle device of the given model where active=true and runmode=true."""
    device = db.find_one({"model": model, "active": True, "runmode": True})
    if not device:
        raise ValueError(f"No idle device available for model {model}")
    return device

@mcp.tool()
def update_device_field(serial: str, field: str, value: bool) -> dict:
    """Lock (runmode=false) or release (runmode=true) a device."""
    return db.update({"serial": serial}, {field: value})

if __name__ == "__main__":
    mcp.run(transport="stdio")
```

### MCP Client - discovering tools and binding them to the LLM

```python
# mcp_client.py
from langchain_mcp_adapters.client import MultiServerMCPClient

mcp_client = MultiServerMCPClient({
    "nbi": {
        "command": "python",
        "args": ["mcp_nbi_server.py"],
        "transport": "stdio",
    },
    "device_pool": {
        "command": "python",
        "args": ["mcp_device_pool_server.py"],
        "transport": "stdio",
    },
    "device_cli": {
        "command": "python",
        "args": ["mcp_device_cli_server.py"],
        "transport": "stdio",
    },
})

async def get_tools():
    # dynamically discovers every @mcp.tool() across all connected servers
    return await mcp_client.get_tools()
```

### LLM node + ReAct loop in LangGraph

```python
# agent_graph.py
from langgraph.graph import StateGraph, END, MessagesState
from langgraph.prebuilt import ToolNode
from langchain_google_genai import ChatGoogleGenerativeAI

llm = ChatGoogleGenerativeAI(model="gemini-2.5-pro")

async def build_graph():
    tools = await get_tools()                 # from MCP client
    llm_with_tools = llm.bind_tools(tools)

    def agent_node(state: MessagesState):
        context = build_context_prompt(state["messages"][-1].content, "wifi")  # RAG
        response = llm_with_tools.invoke(context)
        return {"messages": [response]}

    def should_continue(state: MessagesState):
        last = state["messages"][-1]
        return "tools" if last.tool_calls else "post_onboarding"

    graph = StateGraph(MessagesState)
    graph.add_node("agent", agent_node)
    graph.add_node("tools", ToolNode(tools))
    graph.add_node("post_onboarding", post_onboarding_node)
    graph.add_node("verify", verify_node)
    graph.add_node("report", report_node)

    graph.set_entry_point("agent")
    graph.add_conditional_edges("agent", should_continue,
                                 {"tools": "tools", "post_onboarding": "post_onboarding"})
    graph.add_edge("tools", "agent")            # ReAct loop: back to reasoning
    graph.add_edge("post_onboarding", "verify")
    graph.add_edge("verify", "report")
    graph.add_edge("report", END)

    return graph.compile()
```

### Verify node - deterministic check, not LLM-trusted

```python
# verify_node.py
def verify_node(state):
    device = state["device"]
    for check in state["verify_spec"]:
        output = run_cli_command(device, check["command"])   # via device_cli MCP tool
        if check["type_of_match"] == "contains":
            assert all(v in output for v in check["expected_op"].values()), \
                f"Verification failed for {check['command']}"
        elif check["type_of_match"] == "time_equality":
            assert_time_matches(output, check["expected_op"])
    return {"verify_status": "PASSED"}
```

The two one-liners worth having ready in an interview: **"why LangGraph over a simple agent loop"** (cycles + conditional state, not just tool calling) and **"why MCP over a custom tool-calling layer"** (standard protocol, auto-discovery, swap backends without touching agent code).

---

## Closing Thoughts
