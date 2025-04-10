---
layout: post
title:  "2025-04-05-Welcome-to-World-of-AI-RAG-AGENTIC-AI-MCP"
date:   2025-04-05 
desc: "AI - Its not optional - Its a must to use and make magic in our work and life."
---

# Welcome to the World of AI
In this Era, We are been differentiated as 2 type of techies, One who learns, explores AI and other who don't. So better lets be in good, highly energetic and growing mind side to learn, explore and make use of AI to do our task.
Here are the some of the basic terms and high level uses of *AI, ML, RAG, Agents, Automation, and LangChain in Action*

---
##  What is AI & ML?

-  **ML (Machine Learning)** is a subset of AI where machines learn from data which helps to predict the outptut with the trained data.
-  **AI (Artificial Intelligence)** enables machines to mimic human intelligence.



-  **Types of ML**:

- Supervised Learning - When target label is present in the data.

- Unsupervised Learning - target label is not present in the data.

- Reinforcement Learning - used in AI games where there is a state, Environment and reward.

---

  

## Basic ML Models

  

-  **Linear Regression**:

Input data need to be mapped to a real-valued output - Predict continuous values (e.g., house prices).

  

-  **Clustering (e.g., K-Means)**:

Group similar data points without labels.

  

-  **Classification**:

Categorize data into classes (e.g., spam detection, sentiment analysis).

-  **Recommendation**:

To recommend items - Netflix - more likeable to the datapoint.

-  **Forecasting**:

Data pattern understanding - predic future prices.
  

---

  

## What is RAG (Retrieval-Augmented Generation)?

  Retrieval - Augumented - Generation is a NLP to improve quality of LLM response.

 **Retrieval** - Retrieves data from local data store like DB, vector DB, Excel files.
 **Augumented** - Using the data forms appropriate query.
 **Generation** - Using the query generate response from LLM. 



- Ideal for **question answering** from documents, websites, or databases.

 ### How RAG works
  ```
           [ User Query ]
               |
         [ Embed Query ]
               |
         [ Retrieve Documents ]
               |
     [ Inject Docs + Query into LLM ]
               |
         [ Generate Response ]

  ```
### Use Cases

-   Customer support bots
    
-   Internal knowledge assistants
    
-   Legal, medical, technical Q&A
    
-   AI agents with access to dynamic or large corpora
---

  

##  What are AI Agents?

  

-  **Agents** are LLM-powered systems that perform tasks step-by-step. that can **perceive**, **reason**, and **act** **autonomously or semi-autonomously** to achieve specific goals.


**It Can** 
-   Make decisions
    
-   Use tools or APIs
    
-   Execute multi-step plans
    
-   React to feedback or environmental changes.
  
  ### How AI AGENTS WORK
  
```sh

[User Task/Goal]
      ↓
[LLM interprets task]
      ↓
[Agent plans steps]
      ↓
[Executes action or queries tools]
      ↓
[Updates memory / refines answer]
      ↓
[Repeats or ends task]

```

### Real-World Use Cases

-   Automating web tasks (e.g., data entry, scraping)
    
-   Customer support automation
    
-   Writing code based on bugs or issues
    
-   Managing calendar and emails
    
-   Research assistants that search, read, and summarize

  

## Agentic AI & RAG+Agents

  

-  **Agentic AI**: LLM + Tools + Memory + Multi-step Reasoning

-  **RAG+Agents:**

- Retrieve relevant knowledge

- Reason and act using that context

- Use tools like browsers, code runners, APIs

- Enables **deep automation and adaptive intelligence**.

  This is the **combo** of **Retrieval-Augmented Generation (RAG)** and **Agentic AI** — one of the most powerful ways to build intelligent assistants.

###  What It Means:

-   **RAG handles the “what to say”**: Retrieves relevant knowledge to ground the agent in factual, domain-specific data.
    
-   **The Agent handles the “what to do”**: Uses the retrieved info to reason, plan, and act — like using tools, updating databases, or automating workflows.
    

###  Example Use Case: Web App Test Automation Agent

> “Check all user flows in our web app, report broken paths, and open tickets.”

### With RAG+Agents:

-   RAG fetches test plans, user stories, bug history
    
-   Agent runs tests, detects issues
    
-   Uses GitHub API to open issues
    
-   Writes a daily QA summary
    

----------

## Why RAG + Agents Work So Well Together

### RAG

- Provides **knowledge**
- Context-aware responses
- Great for question answering
- Pulls live or private data

### Agent

- Provides **action**
 - Goal-driven behavior
- Great for task execution
 - Executes logic, API calls, code

---

  

## Web Automation & MCP Servers

  

### How AI + RAG + Agents Help with MCP

#### Scenario: Web Automation Agent Integrated with MCP

1.  An AI agent gets a **task from the MCP server** (e.g., “push wlan psk for xxx cambium id in cnmaestro cloud”)
    
2.  It **uses RAG** to pull knowledge (like config docs or previous test logs)
    
3.  The agent **launches a web automation routine** using Playwright
    
4.  It **parses the results**, compares with thresholds or rules
    
5.  **Reports back to the MCP** or updates dashboards/alerts

  
```
[MCP Server] — task → “Check app login status on 3 environments”
        ↓
[Agent] — retrieves login test from vector DB (RAG)
        ↓
[Agent] — uses Playwright to simulate login
        ↓
[Agent] — logs results, opens a JIRA issue if failure
        ↓
[MCP] — gets status report, updates dashboard

```
---
