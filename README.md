# SETU — सेतु · Intelligent Data Capture & Schedule-Linking Layer

**The bridge between what was planned and what actually happened.**

SETU turns the messy field updates a site already sends — voice notes, daily reports,
spreadsheets, photographed diary pages — into an up-to-date Primavera schedule in
minutes instead of weeks, with a confidence score and an audit trail behind every date.

## 🔗 Quick Links

| | |
|---|---|
| 🚀 **Live app** | **https://setu-live.vercel.app** |
| 📊 **Presentation (PPT)** | [Google Drive](https://drive.google.com/drive/folders/1U0C9GeCJx9CTRL0o38cEHl6f9uTMM1YM) |
| 🎥 **Demo video** | *coming soon — see [submission/DEMO.md](submission/DEMO.md)* |
| 💻 **Source code** | [github.com/KeeratMalhotra/SETU](https://github.com/KeeratMalhotra/SETU) |

> This repository is the SIH 2026 submission (overview, architecture, presentation,
> demo and screenshots). The full working product lives in the source-code repo above.

## 1. Project Information

- **Project Title:** SETU (सेतु) — Intelligent Data Capture & Schedule-Linking Layer for Infrastructure Project Management
- **PS ID:** 26122
- **PS Title:** Intelligent Data Capture & Schedule-Linking Layer for Infrastructure Project Management — Real-Time Actual Progress Tracking (Planning-to-Execution Bridge)
- **Organization:** Oil India Limited
- **Department:** Oil India Limited
- **Category:** Software
- **Theme:** Smart Automation
- **Team:** Team Epsilon

### Team Members

| Member | Roll No. | Role | GitHub |
|---|---|---|---|
| **Keerat Malhotra** | 2024UIN3311 | Team Lead · Backend & Deployment | [@KeeratMalhotra](https://github.com/KeeratMalhotra) |
| Bhumika Aswal | 2024UIN2353 | Frontend & UX | [@bhumikaaaswal91](https://github.com/bhumikaaaswal91) |
| Jatin Kumar | 2024UIN3332 | AI / Linker (5-signal matcher) | [@Jatin21006](https://github.com/Jatin21006) |
| Lian Suan Mang | 2024UIN3340 | Telegram Bot & Ingestion | [@josephliann](https://github.com/josephliann) |
| Bhavya Maheshwari | 2024UIN2373 | Data & Institutional Memory | [@m-bhavyaa](https://github.com/m-bhavyaa) |
| Abhinav Kumar | 2024UIN3302 | Schedule Linking & Audit | [@not-simple-abhi](https://github.com/not-simple-abhi) |

## 2. Problem Statement

Infrastructure project schedules cascade from macro milestones (L1) down to micro,
executable activities (L5/L6) across multiple disciplines — civil, piping,
static/rotating equipment, electrical, instrumentation, HSE — all executing and
reporting in parallel. The baseline plan is well-structured (Primavera/MS Project),
but **actual execution data flows back through daily progress reports, site diaries,
discipline-wise spreadsheets, and verbal supervisor updates** — each in its own
format, language, and cadence, largely disconnected from the L5/L6 activity IDs in
the plan.

As a result:

- Actual progress data is **fragmented, delayed, and inconsistently structured** across disciplines and contractors.
- Manual reconciliation with the baseline lags the schedule-update cycle by **days or weeks**.
- Field execution is often more granular than the planned WBS, and different disciplines describe the same physical progress differently (e.g. *"spool erected"* vs the plan's *"Erect Line 24"-P-1023"*).
- Downstream analytics, delay/risk analysis and forecasting inherit this poor-quality, late data.
- When a project closes, the hard-won knowledge of what actually happened — real durations, real bottlenecks, real deviations — is rarely captured in a structured, queryable form, so it is lost rather than feeding future planning.

## 3. Proposed Solution

**SETU (सेतु — "bridge")** is an intelligent data-capture and schedule-linking
platform that automatically converts unstructured field reports — including voice
messages and text sent via Telegram in **Hindi, Assamese, or English** — into
**verified, traceable updates** in the Primavera P6 baseline. It is a real-time
bridge between site execution and project planning.

A supervisor sends one voice note — *"spool laga diya 24-P-1023"* — and the right
task in the plan updates itself in under a minute, with a confidence score and a full
audit trail. High-confidence updates auto-post; the rest are routed to a project
engineer for one-click approval. Every finished project banks its real durations and
delay causes, so the next project's plan starts smarter.

The baseline is **never overwritten**: SETU records actuals and hands the planner a
Primavera-importable delta to approve. Full production-grade OCR/ASR is not required
(the PS excuses it); SETU demonstrates ingestion of multiple real input formats end
to end.

## 4. Key Features

- **Multilingual capture** — Hindi, Assamese, English and code-mixed speech.
- **Voice-first, Telegram-native** — supervisors report the way they already chat; no forms, no training.
- **Heterogeneous ingestion** — free-text daily reports, discipline spreadsheets, photographed site diaries, voice notes, and Primavera/MSP exports.
- **3-layer AI fallback** — Vertex AI (Gemini) → NVIDIA NIM → deterministic rule engine, so extraction never dies, even on poor connectivity.
- **5-signal activity linker** — tag/line-number match, lexical wording, multilingual semantic similarity, discipline ontology, and schedule timing, combined to pin each report to the correct L5/L6 activity.
- **Confidence + trust gate** — high-confidence updates auto-post; medium-confidence ones go to one-click human review; low-confidence ones are flagged as new (never silently dropped).
- **Tamper-evident audit ledger** — every posted date is SHA-256 hash-chained with who reported it, when, and through which channel.
- **Real-time cockpit** — a live schedule/Gantt with delay badges and a "no report yet" state, updated instantly via Server-Sent Events.
- **Ask your project** — plain-language questions (any language) answered exactly from the real data, not guessed.
- **Institutional memory** — a growing, queryable repository of real durations, recurring delay causes, and discipline-wise productivity that future projects learn from.
- **One-click P6 handover** — verified actuals export as a Primavera-importable delta (XER/CSV/JSON); the planner stays in control.

## 5. Technology Stack

- **Frontend:** Next.js, TypeScript, Tailwind CSS, Framer Motion (multilingual UI — en/hi/as)
- **Backend:** Python, FastAPI, SQLAlchemy, LangGraph (agent orchestration)
- **AI / ML:** Vertex AI (Gemini — text, vision, audio) → NVIDIA NIM → deterministic rules; multilingual embeddings; scikit-learn reranker; rapidfuzz
- **Database:** Neon Postgres (pgvector + full-text), hash-chained audit ledger
- **Field interface:** Telegram Time Agent (voice + text, multilingual)
- **Deployment:** GCP Cloud Run (backend), Vercel (frontend), Neon (database)

## 6. Architecture

See [docs/architecture.md](docs/architecture.md) for the full breakdown.

```text
Field inputs (voice / text / photo / spreadsheet / P6 export)
        |
        v
   Telegram bot  /  Web capture
        |
        v
   SETU Backend (FastAPI)
     Extract  (3-layer AI: Vertex -> NVIDIA -> rules)
        |
     Link     (5-signal matcher -> confidence)
        |
     Reconcile (dedupe, contradictions)
        |
        +--> confidence >= 0.90 --> auto-post
        +--> 0.60 - 0.90        --> review inbox (one-click approve)
        +--> < 0.60             --> flagged as new
        |
        v
   Neon Postgres  (activities · actuals · audit ledger · memory)
        |
        +--> Live cockpit (schedule / Gantt / insights) via SSE
        +--> P6-importable delta (XER / CSV / JSON)
```

## 7. Repository Structure

```text
SIH_Submission_2026_Team-Epsilon/
├── README.md
├── SUBMISSION_GUIDE.md
├── submission/
│   ├── PRESENTATION.md
│   └── DEMO.md
├── src/
│   └── README.md          # points to the real product repo
├── docs/
│   └── architecture.md
├── assets/
│   └── screenshots/
│       └── README.md
├── requirements.txt
├── .gitignore
└── LICENSE
```

### What goes where?

| Item | Location |
|---|---|
| Source code (full product) | [github.com/KeeratMalhotra/SETU](https://github.com/KeeratMalhotra/SETU) — see [src/README.md](src/README.md) |
| Architecture / technical documentation | `docs/` |
| Project screenshots | `assets/screenshots/` |
| Final PPT / presentation | `submission/PRESENTATION.md` |
| Demo video link | `submission/DEMO.md` |
| Project overview | `README.md` |

## 8. Final Presentation

The final SIH presentation is on Google Drive — link in
[submission/PRESENTATION.md](submission/PRESENTATION.md).

## 9. Demo Video

A demo video walkthrough is linked in [submission/DEMO.md](submission/DEMO.md).

## 10. Screenshots

Key screenshots are in [assets/screenshots/](assets/screenshots/) — see its
[README](assets/screenshots/README.md) for the list and naming convention.

## 11. Installation

The full product (frontend + backend + Telegram bot) is in the
[SETU repository](https://github.com/KeeratMalhotra/SETU). To run it locally:

```bash
git clone https://github.com/KeeratMalhotra/SETU.git
cd SETU
make dev-setup     # Python (uv) + Node dependencies
make gen-demo      # a curated 36-activity demo schedule to import
```

Full step-by-step instructions are in that repo's `docs/RUN_LOCALLY.md`, and
production hosting (Cloud Run + Vercel + Neon) in `docs/HOSTING.md`.

## 12. Run

```bash
# Backend (FastAPI)
uv run uvicorn setu_api.main:app --app-dir apps/api --port 8000

# Frontend (Next.js)
cd apps/web && npm run dev

# Telegram bot (multilingual voice + text capture)
uv run python -m setu_bot.telegram_app
```

The AI features (voice transcription, diary-photo reading, multilingual semantic
matching) use Vertex AI; see `docs/GCP_SETUP.md` in the product repo. Everything
degrades gracefully without it — text and spreadsheet ingestion still work via the
deterministic fallback.

## 13. Future Scope

- **Live Primavera EPPM API write-back** (currently a reviewed, importable delta by design — a direct API sync is a clean add-on where a client grants access).
- **Synoptic Plant Twin** — a live interactive facility map, colouring equipment by real-time activity status.
- **Monte Carlo forecasting (P50/P90)** surfaced in the cockpit, driven by the institutional-memory duration distributions.
- **Plan Critic** — flag optimistic new schedules against historical actuals ("your 5-day hydrotest is historically 8 days").
- **On-device / offline capture queue** for genuinely no-signal sites, syncing when connectivity returns.
- **More languages** across the north-east and beyond.

## Important

This repository does **not** contain passwords, API keys, access tokens, `.env`
files, or any confidential credentials. Before submission, verify the repository and
all linked resources (PPT, demo video) are accessible to reviewers while logged out.
