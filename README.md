# Budgeted Document-Answering Agent (Antigravity Build)

A precise, zero-RAG, hand-written document-answering agent that operates under a **hard budget of 6 tool calls** per question. Built without agent frameworks (no LangChain, no LlamaIndex, no CrewAI), with post-generation grounding verification and untrusted-data injection defenses.

---

## 🚀 Quickstart

### 1. Prerequisites & Virtual Environment
Ensure Python 3.10+ is installed.

```bash
# Create and activate virtual environment
python -m venv .venv

# Windows PowerShell:
.\.venv\Scripts\Activate.ps1

# Linux / macOS:
source .venv/bin/activate
```

### 2. Install Dependencies
```bash
pip install -r requirements.txt
```

### 3. Configure API Key
Create a `.env` file from `.env.example`:
```bash
cp .env.example .env
```
Fill in your `GEMINI_API_KEY` (or `ANTHROPIC_API_KEY`):
```env
GEMINI_API_KEY=your_gemini_api_key_here
GEMINI_MODEL=gemini-flash-lite-latest
```

---

## 💻 Running the Components

### Run Streamlit Web Application (Port 8502)
```bash
streamlit run app.py --server.port 8502
```
Access the application at: `http://localhost:8502`

### Run Command Line Interface (CLI)
Debug questions quickly without launching the browser UI:
```bash
python cli.py samples/trap_test.pdf "What is the authorized budget allocation for the quantum computing initiative?"
```

### Run Evaluation Suite
Generate the test trap PDF (8 pages, split facts, amendments, prompt injections) and evaluate accuracy:
```bash
# Generate test artifacts
python make_test_pdf.py

# Run evaluation on test questions
python eval.py samples/trap_test.pdf samples/trap_questions.json
```

### Run Unit Tests (PyTest)
Verify budget enforcement, refusal at 7th call, fresh state isolation, and grounding checks:
```bash
pytest tests/test_core.py -v
```

---

## 🛠️ Project Structure
- `document_store.py`: In-memory PDF parser and backing store (PyMuPDF).
- `tools.py`: Exactly 4 functions (`list_documents`, `list_headings`, `get_page`, `search_keyword`).
- `budget.py`: `BudgetedToolExecutor` enforcing the 6-call max budget and untrusted data wrapping.
- `logger.py`: Appends JSONL audit traces to `./logs/tool_calls.jsonl`.
- `prompts.py`: System prompt covering navigation strategy, amendments, and injection defenses.
- `agent.py`: Handwritten while-loop agent with native tool calling and code-level grounding verification.
- `app.py`: Streamlit chat UI with status badges, cited pages, and expandable call traces.
- `cli.py`: Standalone CLI runner.
- `eval.py`: Automated benchmark runner asserting budget limits and checking answers.
- `make_test_pdf.py`: Creates the 8-page trap document and question set.
- `tests/test_core.py`: Pytest suite for non-LLM core mechanics.
- `MEMO.md`: Architectural documentation, design rationale, and failure mode mitigations.
