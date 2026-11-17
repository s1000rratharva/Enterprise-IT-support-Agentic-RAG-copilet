# Enterprise IT Support Agentic RAG Copilot

A FastAPI-based enterprise IT support assistant that answers internal helpdesk questions using a private knowledge base and falls back to web search when the local knowledge is insufficient. The app includes a chat interface, LangGraph orchestration, document ingestion, and a lightweight audit trail.

## Overview

This project is designed for internal IT support scenarios such as:

- VPN and device troubleshooting
- Password reset and MFA guidance
- Company policy lookups
- Software access and provisioning questions
- External vendor guidance when the internal knowledge base is incomplete

The workflow combines:

- Private company KB retrieval using Pinecone + embeddings
- LangGraph routing and evidence grading
- Groq-powered answer generation
- Tavily web fallback for weak or missing internal evidence
- A web UI for chat and document upload

## Project Structure

- `app/` — FastAPI app, API routes, configuration, RAG workflow and vectorstore
- `data/sample_kb/` — sample company knowledge base content used for testing
- `templates/` and `static/` — frontend UI assets
- `uploads/` — uploaded supporting documents
- `ingest_sample_kb.py` — indexes the sample KB into Pinecone
- `run.py` — starts the local FastAPI server

## Prerequisites

- Conda or Miniconda installed
- Python 3.11
- API keys for the following services:
  - Groq (`GROQ_API_KEY`)
  - Tavily (`TAVILY_API_KEY`)
  - Pinecone (`PINECONE_API_KEY`)

## Setup

Create and activate a project environment:

```bash
conda create -n enterprise-it-support-rag python=3.11 -y
conda activate enterprise-it-support-rag
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Create a `.env` file in the project root with the required configuration:

```env
GROQ_API_KEY=your_groq_key
TAVILY_API_KEY=your_tavily_key
PINECONE_API_KEY=your_pinecone_key
PINECONE_INDEX_NAME=fde-it-support-rag
PINECONE_NAMESPACE=company-it-kb
ADMIN_API_KEY=change-me
```

If you are using the sample knowledge base, index it before running the app:

```bash
python ingest_sample_kb.py
```

## Run the app

Start the local development server:

```bash
python run.py
```

Then open:

```text
http://127.0.0.1:8000
```

## Features

- Chat with an enterprise IT support agent
- Upload and index new documents into Pinecone
- Route user questions to private KB or direct response
- Grade evidence before answering
- Retry with query reformulation when retrieval quality is weak
- Show LangGraph trace details in the UI

## API Notes

The app exposes:

- `GET /api/health` — health check
- `POST /api/chat` — ask a support question
- `POST /api/ingest` — upload a document and index it

Use the `X-Admin-Key` header when uploading documents; it must match `ADMIN_API_KEY` from `.env`.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
