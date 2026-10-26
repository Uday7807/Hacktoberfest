# RecallAI — AI-Powered Personal Search Across Mixed Content

RecallAI searches a personal knowledge library across PDF, TXT, Markdown, code, DOCX, FAQs, and public webpage text. It combines ChromaDB semantic retrieval with SQLite FTS5 keyword retrieval, then optionally generates a grounded answer using Google Gemini.

## Project structure

- `frontend/` — Stitch-exported HTML screens and API integration
- `backend/` — FastAPI search and ingestion API
- `.env.example` — safe environment-variable template

## Requirements

Python 3.11 or 3.12 is recommended. From the repository root:

```bash
python -m venv .venv
# Windows: .venv\\Scripts\\activate
# macOS/Linux: source .venv/bin/activate
pip install -r backend/requirements.txt
cp .env.example .env
```

Add your Gemini key to `.env` as `GEMINI_API_KEY=...` if you want generated answers. Never commit `.env` or API keys.

## Run backend

```bash
uvicorn backend.main:app --reload --host 127.0.0.1 --port 8000
```

API docs: http://127.0.0.1:8000/docs
Health check: http://127.0.0.1:8000/api/health

## Run frontend

In another terminal:

```bash
python -m http.server 5173 --directory frontend
```

Open http://127.0.0.1:5173. Start the backend first. The Search and Add Content screens are the primary API-connected screens; other Stitch exports may remain prototypes.

## API endpoints

- `GET /api/health` — health check
- `GET /api/library` — list indexed sources
- `POST /api/ingest/file` — upload and index a supported file
- `POST /api/ingest/url` — index a public webpage
- `POST /api/ingest/faq` — add a question/answer pair
- `POST /api/search` — hybrid search with optional Gemini answer
- `DELETE /api/library/{doc_id}` — delete a source

The default `X-User-Id` is `demo-user`; this is not authentication. Do not expose the backend publicly without implementing authentication, authorization, rate limiting, and a security review of upload/URL ingestion.
