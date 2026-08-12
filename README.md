# 🤖 Team Research Bot

A compact **multi-agent research pipeline** demonstrating structured, serialized agent handoffs and robust JSON validation.

This educational project shows how to design an AI agent team with clearly defined roles:

**Researcher → Writer → Fact-Checker → Editor**

The pipeline uses **Pydantic-backed contracts** to validate communication between agents, supports both a deterministic mock LLM and real OpenAI models, and saves a complete JSON trace of every run for debugging and inspection.

---

## 🌟 Why This Project Matters

This project demonstrates practical concepts that are highly relevant to **ML/AI Engineering, Generative AI, NLP, and Prompt Engineering** roles.

### Key highlights

* 🧩 Designed a role-based **multi-agent architecture**
* 🔄 Implemented structured **agent-to-agent handoffs**
* 📦 Used **Pydantic schemas** for structured JSON communication
* 🛡️ Added JSON validation and automatic error-repair attempts
* 🧪 Built a deterministic **mock LLM mode** for reproducible demonstrations
* 🤖 Added support for **real OpenAI models**
* 📝 Implemented complete **inter-agent trace logging**
* 🧱 Designed the system with modular and independently testable components
* 📚 Included a guided Jupyter notebook and trainer guide for educational use

---

## 🏗️ Architecture

The system follows a sequential multi-agent pipeline:

```text
                    ┌─────────────────────┐
                    │    Source Pack      │
                    │  Local Documents    │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │     Researcher      │
                    │                     │
                    │ Extracts evidence   │
                    │ and key findings    │
                    └──────────┬──────────┘
                               │
                         JSON Message
                               │
                               ▼
                    ┌─────────────────────┐
                    │       Writer        │
                    │                     │
                    │ Creates research    │
                    │      brief          │
                    └──────────┬──────────┘
                               │
                         JSON Message
                               │
                               ▼
                    ┌─────────────────────┐
                    │   Fact-Checker      │
                    │                     │
                    │ Validates claims    │
                    │ against evidence    │
                    └──────────┬──────────┘
                               │
                         JSON Message
                               │
                               ▼
                    ┌─────────────────────┐
                    │       Editor        │
                    │                     │
                    │ Produces the final  │
                    │ edited research     │
                    │      brief          │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │    Final Output     │
                    │  + JSON Run Trace   │
                    └─────────────────────┘
```

### Agent Responsibilities

| Agent              | Responsibility                                                |
| ------------------ | ------------------------------------------------------------- |
| 🔎 **Researcher**  | Extracts evidence-backed findings from the local source pack  |
| ✍️ **Writer**      | Converts research findings into a structured research brief   |
| ✅ **Fact-Checker** | Validates claims against the original evidence                |
| 📝 **Editor**      | Integrates fact-checking results and produces the final brief |

---

## 🔄 Workflow

The orchestrator executes the agents sequentially:

```text
Source Documents
       │
       ▼
   Researcher
       │
       ▼
  Structured JSON
       │
       ▼
     Writer
       │
       ▼
  Structured JSON
       │
       ▼
  Fact-Checker
       │
       ▼
  Structured JSON
       │
       ▼
     Editor
       │
       ▼
 Final Research Brief
       │
       ▼
 JSON Trace
```

Every agent communicates through a **serialized JSON message**.

Before a message is passed to the next agent, it is validated using **Pydantic models**.

This creates an explicit contract between agents and makes the system easier to debug and maintain.

---

## 🛡️ Structured JSON & Validation

One of the main goals of the project is to demonstrate **schema-driven agent communication**.

Instead of allowing agents to freely exchange unstructured text, each agent produces a structured message that follows a predefined Pydantic schema.

```text
Agent A
   │
   │ JSON
   ▼
Pydantic Validation
   │
   ├── Valid ──────────► Agent B
   │
   └── Invalid
          │
          ▼
    Repair Attempt
          │
          ▼
    Pydantic Validation
          │
          ▼
       Agent B
```

This approach provides:

* Explicit communication contracts
* Type validation
* Predictable message structures
* Easier debugging
* Safer agent handoffs
* Automatic recovery from malformed LLM output

When using a real LLM, the wrapper attempts an **automatic repair call** if the generated JSON does not satisfy the required schema.

---

## 🧪 Deterministic Mock Mode

The project includes a deterministic mock LLM mode.

This allows the entire pipeline to run without:

* An OpenAI API key
* Internet access
* GPU resources
* External search APIs

Mock mode is particularly useful for:

* Classroom demonstrations
* Interviews
* Unit testing
* Debugging
* Reproducible experiments

Because the mock responses are deterministic, repeated runs produce predictable results.

---

## 🤖 Real OpenAI Mode

The project can also use real OpenAI models for experimentation.

The LLM wrapper abstracts the model interaction so that the agents do not need to know whether they are running against:

```text
Mock LLM
   OR
Real OpenAI Model
```

This separation keeps the agent implementation modular and easier to test.

---

## 📊 Trace Logging

Every pipeline execution generates a run-level JSON trace.

Example:

```text
traces/
└── run_XXXXXXXXXX.json
```

The trace contains the serialized messages exchanged between agents and the intermediate outputs generated during the pipeline.

This makes it possible to inspect:

* What the Researcher produced
* What the Writer received
* What claims were generated
* How the Fact-Checker evaluated them
* What the Editor changed
* Where a validation error occurred

This is especially useful for **debugging and evaluating multi-agent systems**.

---

## 🛠️ Tech Stack

| Technology                  | Purpose                        |
| --------------------------- | ------------------------------ |
| **Python 3.10 / 3.11**      | Core application               |
| **Pydantic**                | Message schemas and validation |
| **OpenAI API**              | Real LLM execution             |
| **Jupyter Notebook**        | Guided walkthrough             |
| **JSON**                    | Agent-to-agent communication   |
| **Python Standard Library** | Orchestration and file I/O     |

### Design Principles

* Modular architecture
* Clear separation of responsibilities
* Schema-driven communication
* Deterministic testing
* Reproducible execution
* Minimal dependencies
* Traceable agent execution

---

## 📁 Project Structure

```text
team_research_bot/
│
├── notebook.ipynb
│
├── main.py
├── schemas.py
├── llm.py
├── prompts.py
├── source_loader.py
├── orchestrator.py
│
├── agents/
│   ├── researcher.py
│   ├── writer.py
│   ├── fact_checker.py
│   └── editor.py
│
├── data/
│   └── source_pack/
│       └── ...
│
├── traces/
│   └── run_XXXXXXXXXX.json
│
├── tests_smoke.py
├── trainer_guide.md
├── requirements.txt
└── README.md
```

---

## 📌 Component Responsibilities

### `main.py`

CLI entry point for running the complete pipeline.

Supports both:

* Mock execution
* Real OpenAI execution

---

### `schemas.py`

Contains the Pydantic models used to define the contracts between agents.

Responsible for:

* Message validation
* Structured artifacts
* Type checking
* Consistent communication formats

---

### `llm.py`

Provides the LLM abstraction layer.

Responsible for:

* Mock LLM execution
* Real OpenAI calls
* JSON handling
* Validation recovery
* Repair attempts

---

### `prompts.py`

Contains the role-specific prompts used by the agents.

Each agent receives instructions appropriate to its responsibility.

---

### `source_loader.py`

Loads the bundled source documents from:

```text
data/source_pack/
```

These documents provide the evidence used by the Researcher and Fact-Checker.

---

### `orchestrator.py`

Coordinates the complete workflow.

Responsible for:

* Agent execution order
* Passing messages between agents
* Validation
* Trace collection
* Final pipeline status
* Saving the run trace

---

### `agents/`

Contains the individual agent implementations:

```text
agents/
├── researcher.py
├── writer.py
├── fact_checker.py
└── editor.py
```

Each agent has a clearly defined responsibility and can be tested or replaced independently.

---

## 🚀 Setup

### Prerequisites

* Python **3.10 or 3.11**
* Git
* Optional: VS Code
* Optional: Jupyter Notebook
* Optional: OpenAI API key for real LLM mode

---

## 📦 Installation

### Option A — Python Virtual Environment

Navigate to the project directory:

```bash
cd team_research_bot
```

Create a virtual environment:

```bash
python -m venv .venv
```

### Windows

```powershell
.venv\Scripts\activate
```

### macOS / Linux

```bash
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

---

## 🐍 Option B — Conda

Create the environment:

```bash
conda create -n team-research-bot python=3.11 -y
```

Activate it:

```bash
conda activate team-research-bot
```

Install dependencies:

```bash
pip install -r requirements.txt
```

---

## ▶️ Running the Project

### Mock Mode

Mock mode does not require an API key.

From the project root:

```bash
python main.py --mock
```

Example:

```text
Run ID: run_XXXXXXXXXX
Pipeline Status: SUCCESS
Messages: 4
Trace: traces/run_XXXXXXXXXX.json

Final Research Brief:
...

Key Takeaways:
...

References:
...
```

---

## 🔑 Running with OpenAI

To use a real OpenAI model, configure your API key according to the project's environment configuration.

For example:

```text
OPENAI_API_KEY=your_api_key_here
```

Then run the application using its real-model mode.

> **Security:** Never commit your API key, `.env` file, or other secrets to GitHub.

Add sensitive files to `.gitignore`:

```gitignore
.env
.venv/
__pycache__/
*.pyc
```

---

## 📓 Jupyter Notebook

The project includes:

```text
notebook.ipynb
```

The notebook provides a guided walkthrough of:

1. Loading the source documents
2. Creating the agent pipeline
3. Running the Researcher
4. Passing structured output to the Writer
5. Fact-checking generated claims
6. Editing the final response
7. Inspecting the JSON trace

It is designed for classroom demonstrations and learning how multi-agent systems work.

---

## 🧪 Testing

Run the smoke tests with:

```bash
python tests_smoke.py
```

The smoke tests provide a quick way to verify that the core pipeline is functioning correctly.

---

## 📤 Expected Output

A successful execution reports:

* **Run ID**
* **Pipeline status**
* **Number of messages passed between agents**
* **Trace file path**
* **Final edited research brief**
* **Key takeaways**
* **References**

Example trace:

```text
traces/
└── run_XXXXXXXXXX.json
```

Opening the trace allows you to inspect every serialized agent message and intermediate result.

---

## 💡 Interview Talking Points

This project can be discussed in interviews as an example of designing a **structured multi-agent AI system**.

### 1. Why multiple agents?

Instead of asking one LLM to perform the entire task, responsibilities are divided into specialized agents.

```text
Research → Writing → Verification → Editing
```

This improves separation of concerns and makes individual stages easier to test and debug.

---

### 2. Why use Pydantic?

Pydantic provides a strict schema for communication between agents.

Without validation:

```text
Agent A → arbitrary text → Agent B
```

With validation:

```text
Agent A
   ↓
Structured JSON
   ↓
Pydantic Validation
   ↓
Agent B
```

This makes agent communication more predictable and reliable.

---

### 3. Why have a Fact-Checker?

LLMs can generate unsupported or inaccurate claims.

The Fact-Checker compares the Writer's claims against the original source evidence before the Editor produces the final answer.

---

### 4. Why have a mock LLM?

A deterministic mock allows the project to be demonstrated without:

* API costs
* API keys
* Internet connectivity
* Non-deterministic model responses

It also makes testing easier.

---

### 5. Why trace the pipeline?

Multi-agent systems can be difficult to debug because multiple model calls are involved.

The trace provides visibility into every handoff:

```text
Researcher
    ↓
Writer
    ↓
Fact-Checker
    ↓
Editor
```

This makes it easier to identify where an incorrect output originated.

---

## 🎯 Skills Demonstrated

* Multi-Agent Systems
* Agent Orchestration
* Generative AI
* Prompt Engineering
* LLM Integration
* Pydantic
* JSON Schema Validation
* Error Recovery
* Structured Agent Communication
* Modular Python Architecture
* Testable AI Pipelines
* LLM Observability / Tracing
* Technical Documentation

---

## 📝 Resume-Friendly Description

> **Team Research Bot — Multi-Agent Research Pipeline**
>
> Designed and implemented a role-based multi-agent research pipeline using Python, Pydantic, and OpenAI, with specialized Researcher, Writer, Fact-Checker, and Editor agents. Implemented schema-driven JSON communication, automatic validation and repair, deterministic mock LLM execution, and end-to-end trace logging for reproducible debugging and evaluation.

---

## 🔮 Future Improvements

The architecture can be extended in several directions.

### Parallel / Asynchronous Execution

Independent agents could execute concurrently to reduce overall latency.

```text
             ┌── Agent A ──┐
Input ───────┼── Agent B ──┼──► Aggregator
             └── Agent C ──┘
```

---

### External Retrieval

The local source pack could be replaced or supplemented with:

* Web search
* Vector databases
* Document retrieval
* Knowledge bases
* External APIs

This would allow the system to work with dynamic information.

---

### Evaluation Metrics

The pipeline could be extended with automated evaluation metrics such as:

* Fact-checking accuracy
* Citation correctness
* Claim coverage
* Response quality
* Agent-level success rates
* End-to-end evaluation scores

---

## 🔧 Troubleshooting

### `OPENAI_API_KEY` missing

If you do not have an API key, run the project in mock mode:

```bash
python main.py --mock
```

---

### Source directory not found

Make sure you are running the command from the project root:

```bash
cd team_research_bot
python main.py --mock
```

---

### JSON validation failure

The LLM wrapper attempts one automatic repair when generated output does not match the expected schema.

If validation continues to fail:

1. Reduce LLM temperature.
2. Simplify the schema.
3. Strengthen the agent prompt.
4. Make the expected JSON structure more explicit.
5. Test the agent independently.

---

## 📚 Educational Resources

The project includes:

* `notebook.ipynb` — Guided walkthrough
* `trainer_guide.md` — Teaching flow and troubleshooting notes
* `tests_smoke.py` — Basic pipeline verification

These resources make the project suitable for both **self-learning and classroom demonstrations**.

---

## ⭐ Key Takeaway

**Team Research Bot demonstrates how multiple specialized AI agents can collaborate through structured, validated communication rather than relying on unstructured LLM-to-LLM handoffs.**

The combination of:

```text
Specialized Agents
        +
Pydantic Contracts
        +
Deterministic Mock Execution
        +
LLM Integration
        +
Trace Logging
        =
Reliable & Debuggable Multi-Agent Pipeline
```

makes the project a practical demonstration of **production-oriented multi-agent AI design principles**.
