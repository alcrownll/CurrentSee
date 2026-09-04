# CurrentSee

**Turning messy, multi-market client data into clean, understandable insights.**

---

## Overview

Companies operating across multiple countries often receive data back in inconsistent formats — different currencies, date formats, languages, and column naming conventions. Comparing performance across markets usually means manually cleaning and reconciling spreadsheets by hand.

**CurrentSee** automates that process: it ingests raw market data, normalizes it into a consistent format, translates non-English labels, and generates plain-English summaries and visualizations — so a business user can understand what's happening across markets without touching a spreadsheet.

## Who It's For

A business analyst (or similar role) at a company with operations in multiple countries who needs a quick, reliable view of "how is each market performing" without manually wrangling regional data exports.

## Features

- **Data upload** — accept raw CSV/spreadsheet exports from a given market
- **Normalization** — standardize currencies, date formats, and units across markets
- **Translation** — convert non-English column names/labels to English via an LLM
- **AI-generated summaries** — plain-English narrative of key trends per market
- **Visualization** — charts showing trends within and across markets
- **Multi-market comparison** — view multiple markets side by side

## Architecture

CurrentSee is split into two backend services plus a database, following a layered (Controller → Service → Repository) architecture rather than traditional MVC, since this is a pure API system with no server-rendered views.

```
┌─────────────┐      external API       ┌──────────────────┐
│   Client /  │ ───────────────────────▶│  Java Backend     │
│  Dashboard  │◀─────────────────────── │  (Spring Boot)    │
└─────────────┘                          └────────┬──────────┘
                                                    │ internal API
                                                    ▼
                                          ┌───────────────────┐
                                          │  Python AI Service │
                                          │     (FastAPI)      │
                                          └────────┬────────────┘
                                                    │
                                          ┌─────────▼─────────┐
                                          │   PostgreSQL DB    │
                                          └────────────────────┘
```

- **Java Backend (Spring Boot)** — the external-facing REST API. Handles authentication, data upload, dataset management, and persistence. This is the only service exposed to clients.
- **Python AI Service (FastAPI)** — an internal-only service the Java backend calls for data cleaning, translation, and LLM-generated summaries. Never called directly by clients.
- **PostgreSQL** — stores raw and normalized datasets, market metadata, and summary results.

## Tech Stack

| Layer | Technology |
|---|---|
| Backend API | Java, Spring Boot |
| AI / Data Processing | Python, FastAPI, pandas |
| Database | PostgreSQL |
| AI / LLM | Claude API (translation + summarization) |
| Testing | JUnit 5 + Mockito (Java), pytest (Python) |
| CI/CD | GitHub Actions |
| Containerization | Docker, Docker Compose |
| Version Control | Git / GitHub (feature branches + PR workflow) |

## Repository Structure

```
CurrentSee/
├── backend-java/         # Spring Boot service — external REST API
├── ai-service-python/    # FastAPI service — normalization, translation, AI summaries
├── docs/                 # Architecture notes and diagrams
└── .github/workflows/    # CI pipelines
```

## Development Practices

This project is built following real team workflows:

- No direct commits to `main` — all work happens on feature branches (`feature/xyz`, `fix/xyz`)
- Every change goes through a pull request, even as a solo developer
- CI runs tests automatically on every PR
- Unit tests for business logic (Java + Python), integration tests for API + DB behavior

## Roadmap

- [ ] Scaffold Spring Boot service with health check endpoint
- [ ] Set up PostgreSQL + first migration (datasets table)
- [ ] Scaffold FastAPI service with normalization endpoint
- [ ] Connect Java → Python internal API call
- [ ] Add LLM-based translation + summarization
- [ ] Add basic dashboard for visualization
- [ ] Dockerize both services + DB with Docker Compose
- [ ] Set up GitHub Actions CI pipeline

## Getting Started

> Setup instructions will be added here once the initial services are scaffolded.

## License

MIT