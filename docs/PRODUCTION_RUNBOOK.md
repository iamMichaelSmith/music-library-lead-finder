# Production Runbook

## Prerequisites
- Python 3.10+
- AWS credentials with least-privilege access to configured DynamoDB tables
- `.env` configured from `.env.example`

## Install & Verify
```powershell
python -m venv .venv
.\.venv\Scripts\pip install -r requirements.txt
.\.venv\Scripts\pip install -r requirements-dev.txt
.\.venv\Scripts\python -m pytest -q
```

## Required Environment Keys
- `AWS_REGION`
- `LEADS_TABLE`
- `PAGES_TABLE`
- `DASHBOARD_USERS`
- `DASHBOARD_SESSION_SECRET`

## Run Crawler
```powershell
.\.venv\Scripts\python run.py
```

## Run Dashboard
```powershell
.\.venv\Scripts\python -m uvicorn dashboard_app:app --host 127.0.0.1 --port 8001
```

## Pre-merge Gate
```powershell
.\.venv\Scripts\python -m ruff check .
.\.venv\Scripts\python -m black --check .
.\.venv\Scripts\python -m pytest -q
```

## Rollback
- Keep changes branch-based and merge via PR.
- If regression appears, revert the merge commit.
- Keep secrets out of source control.
