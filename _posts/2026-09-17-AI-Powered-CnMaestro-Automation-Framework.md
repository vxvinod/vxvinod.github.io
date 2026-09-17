---
layout: post
title:  "AI Powered CnMaestro Automation Framework"
date:   2026-09-17
desc: "Building an NLP-driven, LangGraph based Test Automation Framework for CnMaestro using LLMs, MCP servers and Vector DB."
---

# AI Powered CnMaestro Automation Framework

## 1. Project Vision and Objectives

The project aims to develop an **AI-powered Test Automation Framework** that automates and validates end-to-end scenarios in CnMaestro with WiFi/NSE/CnMatrix devices using **natural language prompts**. The system is designed to ease test creation (fetched from TestRail), and the LLM intelligently drives the automation using MCP tools for CnMaestro device configuration, verifies the device config through CLI, and integrates seamlessly with test infrastructure (Test Management System, Jenkins, TestRail, Bitbucket), backend APIs (MCP servers), and device interfaces.

We designed an **NLP-driven AI Automation framework** combining:

- **Natural Language Understanding (NLP) via LLMs** - Test Engineers can give test scenarios in natural language text format, which is understood by the LLM that drives the automation using the MCP servers.
- **Test Orchestrator** - The CN Test Management system pulls the test prompt from TestRail and creates a container that has a LangGraph Test Runner to execute the test and get back the report, saving it to the application.
- **LangGraph Test Runner** - The NLP instructions are fed into the LLM, which decides what tools to use to configure via MCP servers, followed by nodes of operations such as post onboarding, verification of config, and report collection.
- **Vector DB** - Used for providing context to the LLM, such as rules for onboarding devices and rules for picking a device from the device pool.

This system empowers test engineers to describe a scenario in natural language and let the framework generate, execute, and validate test cases autonomously, while easing and accelerating test creation, execution, and reporting through the CN Test Management System, with high accuracy and traceability.

---

## 2. CN Test Management Orchestration

The CN Test Management System helps manage test execution. Each test case can be executed individually, or a full test suite can be triggered to run all associated test cases sequentially or in parallel. This behavior - whether to run in series or parallel - is configurable directly within the CN Test Management UI.

### Single Test Case Flow

- A user triggers test execution through the **CN Test Management System**, which interfaces with **Jenkins** through a background job.
- Jenkins spawns a container that:
  - Pulls the test scenario from **TestRail**
  - Fetches the LangGraph TestRunner framework from **Bitbucket**
  - Triggers test execution
- The **LangGraph TestRunner** executes the test steps.
- The test report is pushed to the **Report Server**.
- The **LangGraph TestRunner** updates the test result in CN Test Management via API.
- Users can view logs from Jenkins, HTML reports, and execution history in the CN Test Management UI.

### Test Suite Flow (Parallel Execution)

A user triggers test execution through the CN Test Management System, which interfaces with Jenkins. Jenkins spawns multiple containers, each responsible for executing a test scenario pulled from TestRail, a test framework from Bitbucket, and picking the necessary device required for test execution from the **Device Pool**. Results are pushed back to the Report Server, and users can view past and current execution reports and logs in the CN Test Management UI.

This scalable setup allows test runs to be parallelized across N containers. These containers remain in sleep mode when idle and are activated only when required, significantly optimizing host resource usage.

```
                     +-----------------------+
                     |   CN Test Management  |
   Actor  ---------> |   - Test Execution     |
                     |   - Report Management |
                     +-----------------------+
                                |
                         Background Job
                                v
                          +----------+        +-------------+
                          | Jenkins  |------->| Container 1 |---+
                          +----------+        +-------------+   |
                               |               | Container 2 |  |
                               |               +-------------+  |
                               |               | Container 3 |  |
                               |               +-------------+  |
                               |               |     ...     |  |
                               |               +-------------+  |
                               |               | Container N |  |
                               |               +-------------+  |
                               |                      |         |
                               |                      v         |
                               |             +------------------+
                               |             |  Test Execution  |<--- TestRail
                               |             |                  |<--- Bitbucket
                               |             +------------------+
                               |                |            |
                               |                v            v
                               |        +--------------+  +------------+
                               |        | Report Server|  | Device Pool|
                               |        +--------------+  +------------+
                               |                |
                               +----------------+
```

---

## 3. NLP-Driven LangGraph Framework Architecture

The NLP-driven test automation framework is built around a **LangGraph** execution engine that interprets natural language prompts via an LLM, dynamically selects appropriate MCP tools, and orchestrates validation, reporting, and flow management using modular nodes.

### LLM + ToolNode: ReAct Loop in LangGraph

LangGraph supports the **ReAct (Reasoning and Acting)** loop, which allows the LLM to reason about a scenario, decide on an action (such as invoking a tool), observe the result, and repeat this process iteratively. This enables adaptive and intelligent test flows.

**How ReAct works in testing:**

- **Reason**: LLM evaluates the test objective and current context (e.g., create AP group with `ntp_agroup.json` payload).
- **Act**: Selects and invokes an MCP tool (e.g., `read_payload()`, `post_wifi_enterprise_ap_groups()`).
- **Observe**: Analyzes the tool response.
- **Repeat**: If needed, loops back to reason and act again until completion.

Before executing the test, the necessary context must be provided to the LLM, such as:

- Device onboarding rules (WiFi, NSE, CnMatrix)
- Device pool selection rules

### WiFi Device Onboarding Rules (example)

```json
{
  "runbook_name": "Onboard WIFI Device",
  "description": "Onboard an Enterprise wifi device with configuration.",
  "order_execution_steps": [
    "A WLAN must be created before onboarding or adding Access point or AP device and before AP Group creation.",
    "Create an APGroup after creating the WLAN and link wlan to AP Group.",
    "An APGroup must be linked to a WLAN before assigning it to a device.",
    "Devices must be added only after the APGroup is created or configured.",
    "If the WLAN or APGroup is missing, the device onboarding must fail."
  ]
}
```

### Device Onboarding Rules

Before executing any test:

1. Select a device by model from the device pool where `active=true` and `runmode=true`.
2. Lock the device by setting `runmode=false`.
3. Store the device IP, serial, user, and password in state.
4. Execute WLAN, APGroup creation and assignment.
5. After test completion, release the device by setting `runmode=true`.

Strictly follow this workflow. Do not proceed if any step fails.

---

## 4. MCP (Model Context Protocol) Integration

The NLP-driven framework leverages multiple specialized **MCP Servers** to modularize and standardize interactions with APIs, device interfaces, and data sources. These MCP clients are dynamically discovered and invoked as tools by the LangGraph Test Runner during runtime. All MCP servers are accessed through a central **MCP Client**, and these MCP tools are bound to the LLM. This allows the LLM, through LangGraph, to dynamically invoke any available tool based on the scenario, enabling flexible, intelligent automation driven by natural language prompts.

### NBI/SBI MCP Server
- **Purpose**: Acts as an API client for CnMaestro's backend, handling both SBI and NBI APIs. This server is auto-integrated from the Swagger-exported `openapi.json` file, allowing internal MCP tool methods to be created for each endpoint.
- **Capabilities**:
  - Create/update/delete Sites, APGroups, WLANs, Devices
  - Retrieve onboarding status, client status, and device status

### Device Pool MCP Server
- **Purpose**: Maintains all devices across various models. The LLM intelligently selects an appropriate device based on the required model, blocks it during test execution, and releases it once the test completes.
- **Capabilities**:
  - Allocate idle devices to tests
  - Track busy/available status for upcoming tests
  - Perform bulk device upgrades

### Device MCP Server
- **Purpose**: Provides CLI-level access to managed devices (WiFi, NSE, CnMatrix). Used to retrieve live configurations and execute operational commands.
- **Capabilities**:
  - The verification node uses this MCP tool to execute CLI commands like `show clock`, `service show config`, etc.
  - Retrieve interface status, firmware versions, and system uptime

### DMS Query Engine MCP Server
- **Purpose**: Retrieve DMS data
- **Capabilities**: YTD

---

## 5. Vector DB

**VectorDB** is a specialized database used to store and retrieve high-dimensional vector embeddings - numeric representations of meaningful text, such as rules. In this NLP-driven automation framework, VectorDB plays a critical role in giving the LLM **"memory"** and **"context."**

VectorDB stores domain knowledge such as:

- Rules for WiFi onboarding
- Rules for NSE onboarding
- Rules for CnMatrix onboarding
- Rules for device picking from device pool

These rules are dynamically retrieved using semantic search and fed to the LLM based on the test being executed. This ensures accurate decision-making and relevant contextual grounding during test planning and execution.

```
+-----------+     +---------------------+       +------------+
|  Device   |<--->| Device MCP Server   |\      |            |
+-----------+     | (CLI commands)      | \     |            |
                   +---------------------+  \    |            |
+-----------+      +---------------------+   \   |            |
| CnMaestro |<-NBI>| NBI MCP Server      |----->| MCP Client |
+-----------+      +---------------------+   /   |            |<--->  LLM
                   +---------------------+  /    |            |        ^
                   | Device Pool MCP     |-/     |            |        |
                   +---------------------+       +------------+        |
                   +---------------------+             |         Prompt Directory
                   | DMS Query Engine    |------/       v            (TestRail)
                   +---------------------+       +------------+        ^
                                                  |  Vector DB |        |
                                                  |  - Flow    |  Test Runner Agent
                                                  |    Rules   |  (LangGraph)  <---- Test Orchestrator
                                                  |  - Onboard |
                                                  |    Rules   |
                                                  +------------+
```

---

## 6. LangGraph Workflow Breakdown

- **LangGraph START** - The entry point of the graph where test execution begins.
- **LLM** - Gemini LLM interprets the NLP instruction and plans the execution by choosing tools from the tool node.
- **Tools** - Includes all MCP tools for SBI/NBI APIs, Device Pool, and Device Interface.
- **Post Onboarding** - Once configuration is applied, ensures the device is onboarded and marks it busy in the Device Pool.
- **Verify** - Uses Device MCP tools to fetch device configuration and validates it against user expectations.
- **Log Collector (Report)** - Creates and stores a test execution report.

```
[ Langgraph START ]
        |
        v
     [ LLM ]
        |
        v
  < Tool condition >----> [ Tools ]
        |
        v
 [ Post Onboarding ]
        |
        v
     [ Verify ]
        |
        v
[ Log Collector (Report) ]
        |
        v
      [ END ]
```

---

## 7. Use Case: WLAN + APGROUP - NTP Time Validation

**Prompt Example:**

> "create 3 wlans, one wlan with psk named 'wlan1-demo1' with ssid name 'ssid-demo1' and password 'Xirrus!23' and band '5ghz' and other second wlan with psk named 'wlan2-demo2' with ssid name 'ssid-demo2' and password 'Xirrus!234' and band '5ghz' and third wlan with psk name 'wlan3-demo3' with password 'Xirrus!23' and band 24ghz ssid name 'ssid-demo3'. create AP Group with ntp_apgroup.json payload. add the device with model XV2-2 to the cnmaestro account and associate the apgroup to it."

```json
verify = [
  {
    "command": "service show config",
    "expected_op": {
      "wlan1_ssid": "ssid-demo1", "wlan1_security": "wpa2-psk", "wlan1_band": "2.4GHz",
      "wlan2_ssid": "ssid-demo2", "wlan2_security": "wpa2-psk", "wlan2_band": "5GHz",
      "wlan3_ssid": "ssid-demo3", "wlan3_security": "wpa2-psk", "wlan3_band": "2.4GHz",
      "system_location": "madurai", "tz_name": "America/Anchorage"
    },
    "type_of_match": "contains"
  },
  {
    "command": "show clock",
    "expected_op": {
      "ntp_server": "10.110.136.1", "timezone": "America/Anchorage"
    },
    "type_of_match": "time_equality"
  }
]
```

### Test Flow (Sequence)

```
Gemini-LLM      Tool Node                 Post Onboarding        Verify                  Log Collector
    |----------> Create WLAN1 ---success-->|                        |                          |
    |----------> Create WLAN2 ---success-->|                        |                          |
    |----------> Create WLAN3 ---success-->|                        |                          |
    |----------> read file    ---success-->|                        |                          |
    |----------> post_wifi_enterprise_ap_groups --success-->        |                          |
    |----------> filter_device_by_model  ---success-->               |                          |
    |----------> add_device             ---success-->                |                          |
    |----------> update_device_field(false/block) --success-->        |                          |
    |----------> wait_until_device_online --success-->                 |                          |
    |------------------------------------------------> 1. wait_until_device_online               |
    |                                                  2. update_device_field(true/release)      |
    |                                                        |------> Verify wlan config          |
    |                                                        |         (service show config)  ----> Generate report
    |                                                        |------> Verify NTP config            |
    |                                                        |         (show clock)           ----> Generate report
```

**Langsmith Run:** [https://smith.langchain.com/public/8cea5e90-60c6-42e4-833e-469cf7949c15/r](https://smith.langchain.com/public/8cea5e90-60c6-42e4-833e-469cf7949c15/r)

**Report:** `http://10.110.136.4/CNSupportPortal/1.0.0/Development/One_Network_API/TS_NTP/TC_ApGroup_NTP_Valid/06-23-2025/TC_ApGroup_NTP_Valid.html`

---

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

This framework shows how combining **NLP + LLMs + LangGraph + MCP + VectorDB** can transform manual, script-heavy network device testing into a natural-language-driven, self-orchestrating pipeline - cutting down test creation time while improving traceability and accuracy across WiFi, NSE, and CnMatrix device testing on CnMaestro.
