<div align="center">

#  sidekick

### *A Recursive Loop Agent System for Reliable Task Completion with LangGraph.*

<br />

<!-- Tech Stack Badges -->

---
</div>

## 🚀 Features & Capabilities

**Sidekick** is engineered as a recursive, goal-oriented agent system. It goes beyond simple chat interfaces—it actively executes tasks, self-verifies against your exact criteria, and iteratively learns from feedback to ensure production-grade outcomes.

> 🔄 **The Core Engine:** Built-in **Worker** and **Evaluator** loops ensure autonomous course correction. If the primary output fails to meet your success metrics, the Evaluator provides detailed feedback, and the system automatically pivots and retries.

### 🧠 Core Capabilities

*   **Autonomous Task Execution:** Define a high-level task and a success criterion. Sidekick orchestrates the tools needed, evaluates its own progress, and refines its approach until the goal is achieved.
*   **Recursive Self-Evaluation:** Never worry about hallucinated or incomplete results. The system acts as its own QA layer by cross-referencing final outputs against your constraints.
*   **Actionable Toolset:**
    *   🌐 **Web Browsing & Automation:** Full **Playwright** integration allows the agent to natively navigate, click, interact with, and extract data from live web pages.
    *   🔌 **Extensible Architecture:** Easily register custom tools (`sidekick_tools.py`) to give your agent superpowers specific to your workflow.
*   **Contextual Intelligence:** Powered by **LangGraph** to maintain complex state and memory, ensuring the agent retains context across multi-step execution paths and user feedback sessions.

### 🛠️ What can you do with Sidekick?

| Use Case | How Sidekick Handles It |
| :--- | :--- |
| **Automate Web Research** | Visits target sites, interacts with elements, extracts live data, and synthesizes clean summaries. |
| **Iterative Content Generation** | Drafts content (like code or documentation) and continuously refines it based on your ongoing feedback. |
| **Complex Goal Solving** | Breaks down ambiguous prompts into a logical sequence of actionable sub-tasks and executes them. |
| **Custom Agent Workflows** | Leverages the underlying Graph architecture to plug in specialized tools and prompts for bespoke pipelines. |



## 🏗️ Architecture & Tech Stack

Sidekick is engineered around a stateful, graph-based agentic framework. By leveraging explicit state transitions, the system ensures that long-running, multi-step tasks are executed reliably and evaluated deterministically.

### 🧩 The Agentic Workflow (StateGraph)

The core engine utilizes a `StateGraph` that manages the lifecycle of a task through three primary operational states. Below is the cyclic data flow:

*   **Worker Node:** Takes the current state and task description, coordinates available tools, and drafts a comprehensive response.
*   **Tools Node:** Dynamically executes side effects (e.g., live web browsing via Playwright) and returns data directly back to the graph state.
*   **Evaluator Node:** Acts as the critical QA layer. It reviews the Worker's output against your exact `success_criteria`. If it passes, the workflow terminates; if it fails, it appends corrective feedback to the state and throws the execution back to the Worker for a recursive retry.

---

### 💻 Core Tech Stack

| Component | Technology | Purpose |
| :--- | :--- | :--- |
| **Agent Orchestration** | `LangGraph` | Manages state machine logic, cyclic graph loops, and multi-actor coordination. |
| **LLM Integration** | `LangChain` & `langchain-google-genai` | Abstracts model interactions and structures tool-calling schemas. |
| **Core LLM** | `Google Gemini (gemini-3.1-flash-lite)` | Provides ultra-fast, cost-effective inference ideal for high-frequency recursive loops. |
| **Automation Engine** | `Playwright` | Empowers the agent with dynamic browser interaction, page navigation, and DOM scraping. |
| **State & Memory** | `MemorySaver` | Handles state checkpointing across task "supersteps" to preserve context. |
| **Package Management** | `uv` | Next-generation Python dependency management for rapid, reproducible environments. |

### 🗂️ State & Session Management

Sidekick operates as an importable, class-based Python module (`Sidekick` class). 

*   **Session Isolation:** Every instance is assigned a unique `sidekick_id` (UUID4) upon initialization.
*   **Context Retention:** Using LangGraph's checkpointer mechanism, the agent retains full memory of conversation history, runtime execution paths, and previous feedback intervals within its current session—preventing context drift during recursive tasks.

---

## ⚙️ Installation & Setup

Sidekick leverages `uv` for lightning-fast, reproducible dependency management and `Playwright` for robust web automation. Follow these steps to get your environment up and running.

### 📋 Prerequisites

Before starting, ensure you have the following installed on your machine:
* Python 3.10 or higher
* `uv` (Recommended fast Python package installer) — *If you don't have it, install it via:* `curl -LsSf https://astral.sh/uv/install.sh | sh`

### 1. Clone the Repository

```bash
git clone https://github.com/Himanshu-sharma05/sidekick.git
cd sidekick
```
### 2. Set Up the Virtual Environment & Dependencies

```bash
# Create and activate a virtual environment
uv venv
source .venv/bin/activate  # On Windows use: .venv\Scripts\activate

# Install dependencies from the pyproject.toml 
uv sync
```

### 3. Install Playwright Browsers
Sidekick utilizes Playwright to interact with live web pages. You must install the headless browser binaries required for execution:

```bash 
uv run playwright install chromium
```
**Note** - Installing just chromium keeps your environment lightweight, though you can run uv run playwright install to install all browsers.

### 4. Configure Environment Variables
Create a .env file in the root directory of your project and add your API keys. Sidekick is configured to use Google Gemini by default

```python
# Google Gemini API Key (Required)
GEMINI_API_KEY="your_gemini_api_key_here"

# Serper Api key (required) for google search tool
SERPER_API_KEY="put your serper api key here"

# Optional: LangSmith Configuration for tracing your LangGraph agent execution
LANGCHAIN_TRACING_V2="true"
LANGCHAIN_API_KEY="your_langsmith_api_key_here"
LANGCHAIN_PROJECT="sidekick-agent"
```




## 🛠️ Usage Quickstart

Run this command in your terminal to finally run the agent:
```bash
uv run app.py
```

> [!CAUTION]
> **Security Warning:** This project integrates a Python REPL (Read-Eval-Print Loop) tool, which allows the LLM agent to execute arbitrary Python code locally. Only run tasks you trust, and never deploy this system in an environment where it has access to sensitive host data without proper sandboxing.