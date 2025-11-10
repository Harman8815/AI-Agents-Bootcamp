##  Working of an AI Agent — How to Make One

### 1. Setup Environment

* Create a `.env` file to store API keys and other environment variables safely.
* Example `.env` content:

  ```
  GOOGLE_API_KEY=your_google_api_key_here
  ```
* Install the required package:

  ```bash
  pip install google-adk
  ```

  *(Make sure you have Python 3.9+ installed.)*

---

### 2. Import and Initialize SDK

The **Google ADK** (Agent Development Kit) allows interaction with Gemini models and tool integration.
It provides modules for defining and running AI agents with tool access like Google Search.

```python
from google import adk
from google.adk import Agent, InMemoryRunner
from google.adk.tools import google_search
from dotenv import load_dotenv
import os
```

```python
# Load environment variables
load_dotenv()
api_key = os.getenv("GOOGLE_API_KEY")

# Initialize the ADK
adk.init(api_key=api_key)
print("✅ Google ADK initialized.")
```

---

### 3. Define an Agent

We define an agent with a configuration dictionary that includes its model, purpose, and tools.

```python
analysis_agent_config = {
    "name": "insight_analyzer",
    "model": "gemini-2.5-pro",
    "description": "An intelligent agent designed to analyze data, identify patterns, and generate insights.",
    "instruction": "You are an analytical assistant. Use Google Search to validate your findings or gather missing data.",
    "tools": [google_search],
}

analysis_agent = Agent(**analysis_agent_config)
print("📊 Analysis Agent initialized.")
```

---
Here’s a **condensed and cleaner version** of your Step 4 section — same meaning, half the length, with only the two original code blocks kept and all explanations summarized for quick note-style reading:

---

### 4. Run the Agent

To execute or test your AI agent, you use a **Runner** — the component that manages communication, context, and execution flow between the user and the model.

```python
runner = InMemoryRunner(agent=analysis_agent)
```

This creates a temporary, in-memory environment to run the agent.
`InMemoryRunner` exists mainly for **development, debugging, or small-scale local runs**, since it stores all session data in RAM (not a database). It’s fast, lightweight, and perfect for prototyping.

You can then send queries to the agent asynchronously:

```python
response = await runner.run_debug("What is Artificial Intelligence?")
print(response.text)
```


####  What the Runner Does

* Acts as the **execution engine** that connects user prompts to the agent’s logic, model, and tools.
* Handles **session context**, memory (if any), and tool invocations automatically.
* Uses event-based processing — receiving a prompt, letting the agent think, call tools if needed, and returning a structured response.
* In AI/ML workflows, it’s essential because it enables multi-turn interactions, memory management, and orchestration — turning a static model call into an interactive, stateful agent.

`InMemoryRunner` is ideal when:

* You’re developing or testing agents locally.
* You don’t need persistence between runs.
* You want simplicity before scaling to production runners with database or distributed session storage.

---
