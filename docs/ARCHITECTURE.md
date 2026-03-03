# Architecture Overview

## Purpose
Music Library Lead Finder is a local-first lead discovery and qualification pipeline for music licensing targets.

## Main Flow
1. Seed/discovery URLs are loaded (`seeds.txt`, optional discovery providers).
2. Crawler fetches pages and extracts contact signals.
3. Heuristic scoring classifies candidate lead quality.
4. Leads/pages are persisted to DynamoDB.
5. Dashboard supports human review and status updates.

## Core Components
- `run.py`: crawl/discovery/score/persist entrypoint
- `dashboard_app.py`: review UI and lead workflow actions
- `validate_seeds.py`: seed quality checks
- `dedupe_cleanup.py` / `delete_bad_emails.py`: data maintenance utilities

## Data Stores
- `LEADS_TABLE` (DynamoDB)
- `PAGES_TABLE` (DynamoDB)

## Optional Scale Path
- SQS queue mode (`QUEUE_ENABLED=1`) for producer/worker split.
