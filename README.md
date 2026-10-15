# FinChat — Financial Document Assistant

**FinChat** is an AI-powered financial document assistant built with Streamlit. Upload financial PDFs, process their contents, and ask questions in natural language to find information in reports more easily.

> Built by **Moh Tarique Siddiqui**

---

## Overview

Financial reports can be long and difficult to search manually. FinChat provides a chat-style interface for exploring uploaded financial documents. It uses a retrieval-augmented generation (RAG) workflow to find relevant document content and help generate answers grounded in the uploaded material.

## Features

- **PDF upload** — upload financial documents through the sidebar.
- **Natural-language Q&A** — ask questions about the uploaded documents.
- **Document retrieval** — find relevant passages for a question.
- **Source-aware answers** — the project is designed to provide references to document pages where supported by the source and processing pipeline.
- **Conversation flow** — ask follow-up questions while exploring a report.
- **Local web interface** — run the Streamlit app on your own computer.

## Technology Stack

- **Python** — application runtime
- **Streamlit** — web interface
- **OpenAI API** — language-model functionality
- **LangChain / LangChain Community** — document and retrieval workflow
- **ChromaDB** — vector storage and similarity search

Exact package versions are defined in `requirements.txt`.

## Project Structure

Your repository may contain additional files; a typical layout looks like this:

```text
finchat/
├── app.py                 # Streamlit application entry point
├── requirements.txt       # Python dependencies
├── .streamlit/
│   └── config.toml        # Optional Streamlit theme configuration
├── .venv/                 # Local virtual environment (do not commit)
├── .env                   # Optional local secrets file (do not commit)
└── README.md              # Project documentation
```

## Requirements

- Windows 10/11, macOS, or Linux
- Python version compatible with the packages in `requirements.txt` (Python 3.11 is a reasonable starting point for this project)
- An OpenAI API key with an active API billing balance
- Internet access for API requests and package installation

> **API billing:** ChatGPT subscriptions and OpenAI API billing are separate. API requests may incur charges. Check your API usage and billing limits before processing documents.

## Setup on Windows

These instructions assume your project folder is named `finchat` and is located at `C:\Users\<your-user>\finchat`.

### 1. Open PowerShell in the project folder

```powershell
cd "$HOME\finchat"
```

### 2. Create a virtual environment

If Python 3.11 is installed:

```powershell
py -3.11 -m venv .venv
```

If you already have a working `.venv`, you can keep using it rather than creating it again.

### 3. Install dependencies

```powershell
.\.venv\Scripts\python.exe -m pip install --upgrade pip
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

If dependency installation fails, check the Python version and the specific error before changing package versions.

### 4. Start FinChat

```powershell
.\.venv\Scripts\python.exe -m streamlit run app.py
```

Streamlit prints a **Local URL**, usually `http://localhost:8501`. Open that URL in your browser. If the port is already in use, Streamlit may select another port, such as `8502`.

Keep the terminal open while using the app. Stop the server with `Ctrl+C`.

## Optional: Start with One Double-Click

To make launching easier on Windows, create a file named `start.bat` in the project folder with the following contents:

```bat
@echo off
cd /d "%USERPROFILE%\finchat"
echo Starting FinChat...
.\.venv\Scripts\python.exe -m streamlit run app.py
pause
```

Save it as **`start.bat`**, choosing **All files** in Notepad's “Save as type” field so it does not become `start.bat.txt`. Double-click the file to launch FinChat.

## Configure Your API Key

1. Open FinChat in your browser.
2. Enter your OpenAI API key in the **OpenAI API Key** field in the sidebar, if prompted.
3. Follow the app's instructions to apply the key.

Create and manage API keys through the official [OpenAI API keys page](https://platform.openai.com/api-keys).

### Keep secrets safe

- Never commit API keys to GitHub.
- Do not paste API keys into screenshots, issue reports, or public messages.
- If you store secrets in a `.env` file, ensure `.env` is listed in `.gitignore`.
- If a key is exposed, revoke it and create a replacement.

## How to Use

1. Start FinChat and open the Local URL shown by Streamlit.
2. Add your API key in the sidebar if required.
3. Use **Upload Documents** to select one or more supported PDF files.
4. Click **Process Documents** and wait for processing to finish.
5. Ask questions about the uploaded reports.

Example questions:

- What was the company's total revenue?
- Summarize the key financial risks discussed in the report.
- What does the report say about operating expenses?
- Compare the performance of the business segments.
- Which pages discuss liabilities or cash flow?

Answers depend on the content and quality of the uploaded documents. Verify important figures against the original report before relying on them.

## Troubleshooting

### `git` or `python` is not recognized

Install Git or Python and reopen PowerShell. On Windows, `py --list` shows installed Python versions.

### Dependency installation fails

Confirm that the virtual environment uses a Python version compatible with the requirements. Review the full error output before upgrading or downgrading packages.

### The browser does not open automatically

Use the **Local URL** printed in the terminal and open it manually in your browser. Keep the terminal running.

### `Address already in use` or a different port appears

Open the exact Local URL printed by Streamlit. It may be `http://localhost:8502` or another port instead of `8501`.

### OpenAI API error `429`, `insufficient_quota`, or `credit_balance_exhausted`

This usually indicates that the API account or project has no usable quota or credits, or has reached a billing limit. Check the [OpenAI API billing overview](https://platform.openai.com/settings/organization/billing/overview) and usage limits. ChatGPT subscription access does not automatically provide API credits.

### API key authentication error

Check that the key is valid and belongs to the intended API project or organization. Do not share the key when asking for help.

## Privacy and Responsible Use

- Upload only documents you are authorized to use.
- Review the privacy and data-handling terms of any external AI service before sending documents.
- Do not treat model output as financial, investment, legal, or tax advice.
- Independently verify important conclusions and numerical values against the original source documents.

## Roadmap Ideas

Possible future improvements include:

- More robust source citations and page-level references
- Support for additional document formats
- Document-library management and removal
- Exportable answers and summaries
- More informative processing status and error messages
- Configurable themes and interface branding
- Optional local-model support

These are ideas for future development, not claims that the features are currently implemented.

## Contributing

Suggestions and improvements are welcome. For a change:

1. Create a fork or feature branch.
2. Make a focused change.
3. Test the application locally.
4. Submit a pull request describing the change.

## License

No license is specified here. Add a `LICENSE` file before redistributing the project, and ensure the license is appropriate for the original code and its dependencies.

---

**FinChat** — explore financial documents through natural-language questions.

Built by **Moh Tarique Siddiqui**.
