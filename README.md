# SafGPT

An agentic AI chatbot built with **LangGraph** and **FastAPI**, featuring tool-calling (web search, calculator, live weather), document-based RAG (Retrieval-Augmented Generation), and persistent multi-thread conversation history via SQLite.

## Features

- 🤖 **Agentic conversation engine** powered by [LangGraph](https://github.com/langchain-ai/langgraph), routing between direct LLM responses and tool calls automatically.
- 🔍 **Web search** via Tavily for up-to-date information.
- 🧮 **Calculator tool** for math expressions.
- 🌦️ **Live weather lookup** via OpenWeatherMap (geocoding + current conditions).
- 📄 **Document RAG** — upload PDFs, DOCX, TXT, MD, PY, or CSV files and query their contents; documents are chunked, embedded, and stored per conversation thread in a [Chroma](https://www.trychroma.com/) vector store.
- 💾 **Persistent conversation threads** — chat history is checkpointed to SQLite via `langgraph-checkpoint-sqlite`, so conversations can be resumed later.
- ⚡ **FastAPI backend** serving the chat and upload endpoints.

## Tech Stack

| Component | Technology |
|---|---|
| Agent orchestration | LangGraph |
| LLM | Groq (`langchain-groq`) |
| Embeddings | HuggingFace / Google Generative AI (`langchain-huggingface`, `langchain-google-genai`) |
| Vector store | Chroma |
| Web framework | FastAPI + Jinja2 templates |
| Search tool | Tavily |
| Persistence | SQLite (checkpointer) |
| File parsing | `pypdf`, `docx2txt` |

## Project Structure

```
SafGPT/
├── app.py              # FastAPI app / entry point
├── agent.py             # LangGraph agent definition
├── tools.py              # Tool definitions (search, calculator, weather, RAG retrieval)
├── rag.py                # Document ingestion & retrieval (Chroma + embeddings)
├── database.py            # DB / checkpointer helpers
├── main.py                # Placeholder entry script
├── templates/              # Jinja2 HTML templates
├── uploads/                 # Uploaded documents
├── chroma_db/                # Persisted vector store
├── data/                      # App data
├── pyproject.toml
├── requirements.txt
└── uv.lock
```

## Setup

### 1. Clone the repository
```bash
git clone https://github.com/Ahamed-Safnas/SafGPT.git
cd SafGPT
```

### 2. Create a virtual environment and install dependencies

Using **uv** (recommended):
```bash
uv venv
uv sync
```

Or using **pip**:
```bash
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

### 3. Configure environment variables

Create a `.env` file in the project root:
```env
GROQ_API_KEY=your_groq_api_key
TAVILY_API_KEY=your_tavily_api_key
GOOGLE_API_KEY=your_google_api_key
OPENWEATHER_API_KEY=your_openweather_api_key
```

### 4. Run the app
```bash
uv run app.py
```
or
```bash
python app.py
```

The FastAPI server will start locally — visit the printed local URL (typically `http://127.0.0.1:8000`) in your browser.

## Usage

- Start a new conversation or continue an existing thread.
- Ask general questions, request calculations, or ask about current weather in any city.
- Upload a document to enable RAG — the agent will retrieve relevant chunks from your uploaded files to answer document-specific questions.

## Requirements

- Python ≥ 3.11
- API keys for Groq, Tavily, Google Generative AI, and OpenWeatherMap (see `.env` setup above)

## Roadmap / Future Updates

- [ ] Multi-file batch upload for RAG
- [ ] Support for additional LLM providers (OpenAI, Anthropic, local models via Ollama)
- [ ] User authentication and per-user conversation isolation
- [ ] Conversation export (PDF / Markdown)
- [ ] Improved source citations in RAG answers (page numbers, highlighted excerpts)
- [ ] Dockerfile for containerized deployment
- [ ] Unit and integration test coverage
- [ ] Admin dashboard for managing uploaded documents and threads
- [ ] Rate limiting and usage analytics

Have an idea or feature request? Feel free to open an issue or submit a pull request.

## License

Apache-2.0
