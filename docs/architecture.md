# System Architecture — SETU

Written so a non-technical reviewer can follow it. Only three moving parts to
understand, plus the AI the backend phones for language tasks.

## High-level flow

```text
Field inputs
  voice note · text · photographed diary · spreadsheet · P6 export
        |
        v
  Capture channels
    Telegram bot (voice + text, multilingual)   /   Web app
        |
        v
  SETU Backend (FastAPI)
    1. Extract    -- pull activity, tag, time, quantity from messy input
    2. Link       -- match to the correct L5/L6 plan activity
    3. Reconcile  -- dedupe, resolve contradictions
    4. Route      -- by confidence
        |
        +---- confidence >= 0.90 ----> auto-post the actual
        +---- 0.60 - 0.90         ----> Review inbox (one-click approve/correct/reject)
        +---- < 0.60              ----> flagged as new (never dropped)
        |
        v
  Neon Postgres  (all state: activities · actuals · audit ledger · memory)
        |
        +----> Live cockpit: schedule / Gantt / insights  (via Server-Sent Events)
        +----> P6-importable delta  (XER / CSV / JSON) for the planner to approve
```

## Components

### Capture channels
- **Telegram Time Agent** — supervisors send a voice note or text in Hindi, Assamese
  or English. No new form, no training; it is the app they already use.
- **Web capture** — paste a daily report, upload a spreadsheet/PDF, or photograph a
  diary page; the same pipeline runs server-side.

### Extract — 3-layer AI fallback
The one language step. Providers are tried in order and it stops at the first
success, so a normal request is a single call:

```text
Vertex AI (Gemini)  --fails-->  NVIDIA NIM  --fails-->  deterministic rule engine
```

Every layer returns the same validated data shape, so the system never dies — it
degrades. The rule engine also runs as a cross-check (it is excellent at reading tag
numbers).

### Link — the 5-signal matcher (the moat)
Oil-and-gas task names follow VERB + OBJECT + TAG + LOCATION. SETU breaks both the
plan task and the field report into those parts and scores five independent signals:

1. **Tag / line-number** match (near-perfect precision when present)
2. **Lexical** wording similarity
3. **Semantic** meaning similarity (multilingual embeddings)
4. **Discipline ontology** (is this the right kind of work?)
5. **Schedule prior** (is this activity even due now — are its predecessors done?)

A learned reranker combines them and calibration turns the score into an honest
confidence, which drives the routing above.

### Database — Neon Postgres
Holds **all** state: the plan, the actuals, the tamper-evident audit ledger, and the
institutional-memory tables. The AI layer (Vertex) is stateless — it holds no data —
so cloud accounts can be rotated without migrating anything.

### Trust — hash-chained audit ledger
Every posted actual date is written into a ledger where each entry's hash depends on
the previous one. Editing any past entry breaks every following hash, so tampering is
instantly detectable — the traceability a PSU needs for extension-of-time claims.

### Cockpit + institutional memory
A live schedule/Gantt (planned vs actual, delay badges, a first-class "no report yet"
state) updated via Server-Sent Events; a plain-language "Ask your project" that
answers exactly from the real rows; and a growing repository of real durations and
delay causes that future projects learn from.

## Deployment

| Piece | Runs on |
|---|---|
| Frontend (Next.js) | Vercel |
| Backend (FastAPI) | GCP Cloud Run |
| Database | Neon Postgres |
| AI (inference only) | Vertex AI (Gemini) |
| Field interface | Telegram bot (webhook or polling worker) |

The database is on Neon (not GCP), so GCP holds zero state — the whole system can run
offline via `docker compose up`, and cloud credentials rotate by swapping env vars.
