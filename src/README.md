# Source Code

The complete, working SETU source code lives in the product repository:

## → [github.com/KeeratMalhotra/SETU](https://github.com/KeeratMalhotra/SETU)

That repository contains everything:

```text
SETU/
├── apps/
│   ├── web/          # Next.js frontend (multilingual cockpit)
│   ├── api/          # FastAPI backend
│   ├── agents/       # LangGraph agents, 3-layer AI gateway, extractor, vision, speech
│   └── bot/          # Telegram Time Agent (voice + text)
├── packages/
│   ├── linker/       # the 5-signal matcher + reranker + calibration (the moat)
│   ├── schedule/     # CPM / critical path
│   ├── memory/       # institutional memory
│   └── parsers/      # spreadsheet / P6 / report parsers
├── data/generator/   # the curated demo dataset generator
└── docs/             # architecture, run-locally, hosting, demo plan, cue card
```

## Why the code is linked, not copied here

This submission repository is the SIH overview, presentation, architecture, demo and
screenshots. The product is an active multi-package monorepo (frontend + backend +
agents + Telegram bot + shared packages) that is developed, built and deployed from
its own repository. Linking to it keeps a single source of truth and avoids a stale
copy.

## Run it

```bash
git clone https://github.com/KeeratMalhotra/SETU.git
cd SETU
make dev-setup     # Python (uv) + Node dependencies
make gen-demo      # a curated 36-activity demo schedule to import
```

Local run: `docs/RUN_LOCALLY.md` · Production hosting: `docs/HOSTING.md` ·
AI setup: `docs/GCP_SETUP.md` — all in the product repo.
