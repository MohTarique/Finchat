# FinChat — Financial Document Q&A

A Streamlit-based financial document assistant designed to help users explore financial PDFs, retrieve relevant passages, and ask questions in natural language.

> **Architecture note:** This README describes the planned architecture using **Streamlit, LlamaIndex, PostgreSQL/pgvector, and an LLM**. The current application may still use LangChain and ChromaDB. The pgvector/LlamaIndex pipeline should be integrated and tested before describing it as production-ready.

## Screenshot

![FinChat application screenshot](screenshots/finchat-screenshot.png)

## Features

- Upload financial PDFs such as annual reports and earnings-call transcripts.
- Ask natural-language questions about the uploaded documents.
- Retrieve relevant document passages to support answers.
- Build toward source-aware answers with document and page references.
- Streamlit web interface with a simple upload-and-query workflow.

*Actual features and citation behavior depend on the implementation in `app.py`.*

## Architecture

```text
                  User Question
                       |
                       v
                Streamlit UI
                       |
                       v
               LlamaIndex RAG
             / document retrieval \
                       |
                       v
          PostgreSQL + pgvector
          (embeddings and search)
                       |
                       v
              Retrieved passages
                       |
                       v
              LLM answer generation
                       |
                       v
             Answer + source metadata
```

### Components

| Component | Responsibility |
|---|---|
| Streamlit | User interface, PDF upload, and chat interaction |
| LlamaIndex | Document ingestion, indexing, and retrieval orchestration |
| PostgreSQL | Persistent relational database |
| pgvector | Stores embeddings and performs vector similarity search |
| Embedding model | Converts document chunks and queries into vectors |
| LLM | Generates answers using retrieved document context |

### LangChain and AutoGen

- **LangChain:** The existing repository may use LangChain in its current RAG workflow. During migration, keep the current path working until the LlamaIndex implementation has been tested.
- **AutoGen:** An optional future extension for multi-agent workflows. It is **not** claimed as an integrated dependency in this architecture.

## How retrieval works

1. A user uploads a financial PDF through Streamlit.
2. The ingestion pipeline extracts text and splits it into chunks, retaining useful metadata such as file name and page number where available.
3. An embedding model converts chunks into vectors.
4. LlamaIndex stores and retrieves vectors through PostgreSQL and pgvector.
5. Relevant passages are added to the question context.
6. The configured LLM generates a response grounded in the retrieved content.
7. The application displays the answer and any source metadata available from retrieval.

## Technology stack

- Python
- Streamlit
- LlamaIndex
- PostgreSQL
- pgvector
- An embedding model and an LLM (provider configured separately)
- Optional existing components during migration: LangChain and ChromaDB

## Prerequisites

- Windows, macOS, or Linux
- A supported Python version for the packages pinned by the project
- PostgreSQL with the `vector` extension enabled, if using the pgvector backend
- Credentials for the configured model provider, unless using local models

## Setup

### 1. Clone the repository

```bash
git clone https://github.com/MohTarique/Finchat.git
cd Finchat
```

### 2. Create and activate a virtual environment

**Windows PowerShell:**

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
```

If PowerShell blocks activation, you can run commands through the environment's Python directly, for example:

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

### 3. Install dependencies

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

If LlamaIndex and pgvector are not yet included in `requirements.txt`, install the required integrations after confirming compatibility with your existing dependencies:

```powershell
.\.venv\Scripts\python.exe -m pip install llama-index llama-index-vector-stores-postgres "psycopg[binary]" pgvector
```

### 4. Configure PostgreSQL

Create a database for FinChat and enable pgvector in that database:

```sql
CREATE EXTENSION IF NOT EXISTS vector;
```

Set a database connection string in your environment as `FINCHAT_DB_URL`. Do not commit credentials to Git.

Example PowerShell session variable (replace the placeholder with your actual connection string):

```powershell
$env:FINCHAT_DB_URL = "postgresql://USER:PASSWORD@HOST:5432/DATABASE"
```

This variable applies only to the current PowerShell session. Use an appropriate secret manager or local environment configuration for persistent setup. Do not paste real credentials into README files or public issues.

### 5. Configure the model provider

Configure the API key or local model settings required by your implementation. Keep secrets in environment variables or an ignored local `.env` file. Never commit API keys.

### 6. Run the app

```powershell
.\.venv\Scripts\python.exe -m streamlit run app.py
```

Then open the local URL printed by Streamlit, usually `http://localhost:8501`.

## Configuration checklist

Before running the pgvector pipeline, confirm:

- [ ] PostgreSQL is reachable from the application.
- [ ] The `vector` extension is enabled in the target database.
- [ ] `FINCHAT_DB_URL` is configured securely.
- [ ] The embedding model's output dimension matches the pgvector column/index configuration.
- [ ] The LlamaIndex PostgreSQL vector-store integration is installed.
- [ ] PDF ingestion preserves the metadata needed for source references.
- [ ] The current ChromaDB path is not removed until the new pipeline has been tested.

## Security

- Do not commit `.env`, API keys, database passwords, `.venv/`, or local database files.
- Use least-privilege credentials for PostgreSQL.
- Avoid uploading confidential financial documents to public repositories or unapproved model providers.
- Review the repository's license and third-party dependency terms before redistribution.

## Troubleshooting

**PostgreSQL connection fails**
- Check that the server is running and the connection string, port, database, and credentials are correct.
- Confirm network access and firewall settings if using hosted PostgreSQL.

**`vector` extension is missing**
- Confirm the PostgreSQL installation/provider supports pgvector, then install/enable the extension according to its documentation.

**Embedding dimension mismatch**
- Configure the vector store to match the embedding model's actual output dimension.

**OpenAI insufficient quota**
- Database and framework changes do not restore API credits. Check the provider's billing/quota settings or configure a supported alternative model.

**Streamlit does not open automatically**
- Copy the local URL printed in PowerShell into your browser.

## Roadmap

- [ ] Complete and test the LlamaIndex + PostgreSQL/pgvector retrieval path.
- [ ] Add reliable source/page citations for answers.
- [ ] Add configuration validation and clearer error messages.
- [ ] Compare retrieval quality against the existing LangChain/ChromaDB implementation.
- [ ] Evaluate AutoGen only if multi-agent workflows provide a concrete benefit.

## Author

**Moh Tarique Siddiqui**

---

*This README documents the intended architecture and setup. Update it as implementation details, dependency versions, and supported features are verified.*
