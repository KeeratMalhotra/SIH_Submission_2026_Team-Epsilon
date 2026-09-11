# SIH 2026 Submission Guide — SETU (Team Epsilon)

Use this checklist before sharing the GitHub repository link.

## Required repository content

- [x] Source code is available (full product at [github.com/KeeratMalhotra/SETU](https://github.com/KeeratMalhotra/SETU); this repo links to it).
- [x] `README.md` explains the project clearly.
- [x] PS ID (26122) and PS title are included.
- [x] Problem statement and proposed solution are explained.
- [x] Key features are listed.
- [x] Technology stack is listed.
- [x] Setup and run instructions are provided (and detailed in the product repo).
- [ ] Team members and roles are mentioned. *(fill in `submission/PRESENTATION.md`)*
- [ ] Important screenshots are included in `assets/screenshots/`.
- [ ] Final PPT/presentation is placed in `submission/` or linked in `submission/PRESENTATION.md`.
- [ ] Demo video link is added to `submission/DEMO.md` (optional but recommended).
- [ ] Repository is accessible to reviewers.

## Recommended structure

```text
SIH_Submission_2026_Team-Epsilon/
├── README.md
├── SUBMISSION_GUIDE.md
├── submission/
│   ├── PRESENTATION.md
│   └── DEMO.md
├── src/
│   └── README.md
├── docs/
│   └── architecture.md
├── assets/
│   └── screenshots/
└── ...
```

## Presentation

Upload the final PPT/PPTX to the `submission/` folder when the file size is suitable
for GitHub, using a clear filename such as `TeamEpsilon_SIH2026_SETU_Presentation.pptx`.
If it is too large, use Google Drive or OneDrive and put the shareable viewer link in
`submission/PRESENTATION.md`.

## Demo video

The demo video is optional but strongly recommended for SETU (it has a working
prototype). Add the YouTube/Google Drive link to `submission/DEMO.md` and make sure
it is accessible without requesting permission. A full shot-by-shot demo plan and a
printable cue card already exist in the product repo
(`docs/DEMO_VIDEO_PLAN.md`, `docs/DEMO_CUE_CARD.md`).

## Screenshots

Put important screenshots in `assets/screenshots/`. Recommended for SETU: the landing
page, the live Timeline/Gantt, the Telegram capture, the Review inbox with a receipt,
the "Ask your project" answer, and the P6 handover/export. Use clear names
(`01-landing.png`, `02-timeline.png`, …).

## Do not upload

- Passwords, API keys, access tokens
- `.env` files containing secrets (Neon URL, Telegram token, GCP keys, Resend key)
- Private credentials or any confidential information

## README should answer

1. What problem are you solving?
2. What is your proposed solution?
3. How does it work?
4. Which technologies did you use?
5. How can a reviewer run it?
6. What does the final output look like?
7. What are the important features and expected impact?

## Before submission

Open this repository (and the linked SETU repo, PPT and demo video) in a
private/incognito window and verify a reviewer can access everything without
requesting permission.
