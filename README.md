# SPLASHBot Backend v2

FastAPI backend for SPLASHBot, an AI assistant for macroeconomics and market questions. Version 2 reorganises the original service into routers and a service layer, improves the PDF retrieval pipeline with Gemini, and returns structured chart data that the web client renders.

## Features

- Firebase-backed sign-up, login and bearer-token verification
- Chat sessions, history and message endpoints per session
- File upload for PDFs and images, used as context for answers
- Retrieval over uploaded PDFs with `sentence-transformers` and FAISS
- Analysis modes selectable per chat: General Macroeconomics, 2 Wheels, 4 Wheels, and Retail (General, Beauty, FnB, Drugstore)
- Gemini-generated analysis code run against BigQuery-backed dataframes, with results returned as tables or JSON chart specifications
- Follow-up and starter question suggestions, optional web snippet search

## Tech stack

Python 3.12, FastAPI, Uvicorn, Pydantic, Firebase Admin SDK, `google-generativeai`, Google BigQuery, sentence-transformers, FAISS, pdfplumber, pandas, Plotly. A `Dockerfile` is included.

## Structure

```
main.py        app and router registration
api/           auth, chat, chat history, chat messages, file upload, init questions
services/      business logic behind each router
core/          Gemini, Firebase, BigQuery and logging setup
models/        chat orchestration and Pydantic schemas
utils/         market analysis models, semantic search, web search, question generation
```

## Running locally

```bash
pip install -r requirements.txt
uvicorn main:app --port 8000
```

Set `GEMINI_API_KEY`, `GCS_API_KEY` and `GCS_CX` in a `.env` file. Firebase service-account credentials, the BigQuery project settings and the prompt modules under `helper/` are expected locally and are not included in this repository. Docker: `docker build -t splashbot-api .`.

The web client is the `splashbot.app` repository.
