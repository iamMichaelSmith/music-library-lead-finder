# Music Library Lead Finder

Local-first Python crawler + review dashboard for discovering music licensing library leads, extracting contact signals, and organizing results in DynamoDB for human review.

> **Important:** This project is a research and qualification tool. It does **not** automatically send outreach.

## Why this project exists

Finding high-quality music library leads manually is slow and inconsistent.  
This project speeds up lead discovery by:

- Crawling targeted sites
- Extracting contact signals (email / contact forms)
- Scoring relevance
- Storing structured lead records for review in a lightweight dashboard

---

## What this demonstrates (for employers)

- Python data-pipeline engineering
- Web crawling + HTML parsing
- Heuristic scoring and deduplication
- AWS service integration (DynamoDB, optional SQS)
- FastAPI dashboarding for internal ops tools
- Config-driven architecture and testable utility functions

---

## Architecture (high level)

**Seeds / Discovery** -> **Crawler** -> **Signal Extraction + Scoring** -> **DynamoDB** -> **Review Dashboard**

Optional scaling path: **Producer/Worker mode via SQS**

---

## Features

- Seed-based crawl with optional search-provider discovery
- Contact signal extraction (email/form/link)
- Basic lead relevance scoring
- Domain and page-level dedupe behavior
- DynamoDB persistence for leads/pages
- Review dashboard with authentication
- Optional queue-based scaling with SQS
- Environment-driven configuration (`.env`)

---

## Tech Stack

- **Language:** Python 3.10+
- **Core libs:** requests, BeautifulSoup, boto3, python-dotenv
- **Dashboard:** FastAPI + Uvicorn + Jinja templates
- **Storage:** AWS DynamoDB (or DynamoDB Local)
- **Queue (optional):** AWS SQS
- **Tests:** pytest

---

## Repository Layout

- `run.py` - main crawler pipeline entrypoint
- `dashboard_app.py` - internal review dashboard
- `tests/test_core.py` - baseline utility tests
- `seeds.txt` - crawl seed URLs
- `queries.txt` - optional discovery queries
- `requirements.txt` - runtime dependencies

---

## Quick Start (Windows PowerShell)

```powershell
python -m venv .venv
.\.venv\Scripts\pip install -r requirements.txt
copy .env.example .env
.\.venv\Scripts\python run.py
```

## Quick Start (Linux / Raspberry Pi)

```bash
python3 -m venv .venv
./.venv/bin/pip install -r requirements.txt
cp .env.example .env
./.venv/bin/python run.py
```

---

## Environment Configuration

Start with `.env.example` and set at least:

```env
AWS_REGION=us-east-1
LEADS_TABLE=MusicLibraryLeads
PAGES_TABLE=MusicLibraryPages
DASHBOARD_USERS=admin:change_me
DASHBOARD_SESSION_SECRET=change_this_secret
```

Optional (discovery providers):

```env
DISCOVERY_ENABLED=1
DISCOVERY_PROVIDERS=brave,serper
BRAVE_API_KEY=your_key
SERPER_API_KEY=your_key
```

Optional (DynamoDB local):

```env
DYNAMODB_ENDPOINT_URL=http://localhost:8000
```

Optional (queue mode / SQS):

```env
QUEUE_ENABLED=1
QUEUE_MODE=producer   # producer | worker | local
SQS_QUEUE_URL=https://sqs.us-east-1.amazonaws.com/ACCOUNT/QUEUE
```

---

## Verification (clone-and-test checklist)

Use these steps to verify from a fresh pull:

1. **Install dependencies**
   ```powershell
   python -m venv .venv
   .\.venv\Scripts\pip install -r requirements.txt
   ```

2. **Configure environment**
   ```powershell
   copy .env.example .env
   ```
   Then set required values.

3. **Run unit tests**
   ```powershell
   .\.venv\Scripts\python -m pytest -q
   ```

4. **Smoke run crawler**
   ```powershell
   .\.venv\Scripts\python run.py
   ```
   Expected: process starts, crawls configured seeds, writes leads/pages records.

5. **Smoke run dashboard**
   ```powershell
   .\.venv\Scripts\python -m uvicorn dashboard_app:app --host 127.0.0.1 --port 8001
   ```
   Open `http://127.0.0.1:8001` and confirm login and lead listing.

---

## Security & Responsible Use

- Use this tool only on data you are authorized to process.
- Respect robots directives, platform terms, and applicable laws.
- Do not use this project for spam, harassment, or unauthorized scraping.
- Keep credentials out of source control (`.env` should remain uncommitted).

---

## Known Limitations

- Current test coverage is utility-heavy; end-to-end coverage can be expanded.
- Scoring is heuristic and may require tuning per niche.
- Crawl quality depends on seed quality and provider signal quality.
- Dynamic/JS-heavy sites may need stronger rendering support if expanded.

---

## Suggested Next Improvements

- Split `run.py` into modular components (`crawler`, `scoring`, `storage`, `discovery`)
- Add structured logging + trace IDs
- Add integration tests for persistence and crawl flows
- Add metrics/reporting (success rate, dedupe ratio, lead quality over time)

---

## License

Add a license file (recommended: MIT) for clearer hiring-manager confidence and open-source clarity.

## Quality & CI
This repo is set up for CI-driven reliability checks on every PR.

Run this local preflight before push:
```powershell
.\.venv\Scripts\python -m ruff check .
.\.venv\Scripts\python -m black --check .
.\.venv\Scripts\python -m pytest -q
```

## Developer Tooling
```powershell
.\.venv\Scripts\pip install -r requirements-dev.txt
.\.venv\Scripts\pre-commit install
.\.venv\Scripts\pre-commit run --all-files
```

## Operational Docs
- `docs/ARCHITECTURE.md`
- `docs/PRODUCTION_RUNBOOK.md`
