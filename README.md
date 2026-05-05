# CPT304 Coursework

**Languages:** English | [简体中文](README.zh.md)

## Project Overview

This repository holds **CPT304** coursework materials, organized into four top-level areas for fast navigation:

- `coursework/` — course workflow, docs, templates, reports
- `projects/` — writable project workspaces (your group’s actual development)
- `third_party/` — bundled third-party samples (read-only reference)
- `artifacts/` — generated outputs (e.g. Lighthouse JSON/HTML exports)

## Repository structure

```text
CPT304_Coursework/
├── README.md                    # This file
├── README.zh.md                 # Chinese README
├── CODE_WIKI.md                 # In-repo wiki / navigation notes (if present)
├── .gitignore
├── coursework/                  # Course workflow & deliverables (read/write for your team)
│   ├── README.md
│   ├── CPT304_Coursework/       # Guideline copies, assignment details, end-to-end workflow doc
│   └── docs/
│       ├── brainstorms/         # Requirements / ideation notes
│       ├── guides/              # Playbooks (e.g. prerequisite work)
│       ├── plans/               # Dated implementation / refactor plans
│       ├── reports/             # Written reports (e.g. Lighthouse evaluations)
│       └── templates/           # Reusable doc templates (e.g. selection matrix)
├── projects/                    # Your group’s active development workspaces
│   ├── README.md
│   └── inventory-management-system/
│       ├── README.md
│       ├── app/                 # Runnable inventory app (HTML/JS/CSS + assets; day-to-day coding)
│       └── docs/                # Project-specific documentation
│           ├── baselines/       # Checklists / “professional edition” baselines
│           ├── brainstorms/
│           ├── collaboration/    # e.g. module ownership
│           ├── defects/          # Defect lists, audits, code-review archives
│           ├── literature/       # Surveys, citations, remediation notes
│           ├── plans/
│           ├── setup/           # Dev environment & onboarding
│           ├── solutions/       # Fix logs & postmortem-style learnings
│           └── templates/
├── third_party/                 # Bundled upstream samples (prefer-read; avoid “living” edits)
│   ├── README.md
│   └── open_source_projects/    # One folder per sample app (see its README)
│       ├── README.md            # Index: how to run each sample locally
│       ├── advanced-finance-tracker/
│       ├── biztrack/
│       ├── budget-app/
│       ├── e-commerce/
│       ├── employee-management-system/   # client + server
│       ├── inventory-app-js/            # Webpack/Tailwind variant
│       ├── inventory-management-system/ # Static reference clone of baseline app
│       ├── polln/                       # Django-style full stack
│       ├── seat-booking-app/
│       └── taskify/                     # Node app with views/src
└── artifacts/                   # Generated or exported outputs (not hand-authored source)
    ├── README.md
    └── lighthouse/
        └── <sample-or-project-name>/    # Per-app Lighthouse *.json + *.html exports
```

### Folder notes (quick reference)

| Path | Role |
|------|------|
| `coursework/` | Central place for course instructions, plans, reports, and shared templates. |
| `coursework/CPT304_Coursework/` | PDF-aligned guideline text and the summarized coursework workflow document. |
| `coursework/docs/` | Living course documentation: brainstorms → plans → reports. |
| `projects/` | Where your group modifies code for the assignment; keeps third-party copies clean. |
| `projects/.../app/` | The deployable, runnable front-end (or app root) you iterate on. |
| `projects/.../docs/` | Specs, defects, literature, and solutions tied to that project only. |
| `third_party/open_source_projects/` | Vendored open-source demos; use as reference and for Lighthouse baselines. |
| `artifacts/lighthouse/` | Stored Lighthouse runs per app name; pair JSON (data) with HTML (report UI). |

## Features

- **Curated open-source samples** — front-end-only apps, full-stack examples (MongoDB/Express/React, Django/MySQL), and tooling demos (Webpack, Tailwind, etc.).
- **Documented local setup** — per-project install and run steps are described in `third_party/open_source_projects/README.md`.
- **Course artifacts** — Lighthouse JSON/HTML under `artifacts/lighthouse/`; notes, guides, and reports under `coursework/docs/`.
- **Assignment reference** — workflow summary in `coursework/CPT304_Coursework/cpt304-coursework-01-complete-workflow.md` (see course PDF for authoritative requirements).

## Installation

1. Clone this repository (SSH example):

   ```bash
   git clone git@github.com:NOSOLUTIONLOVE/CPT304_Coursework.git
   cd CPT304_Coursework
   ```

2. Open `third_party/open_source_projects/README.md` and follow the section for the project you need. Most static projects only require a browser or `python3 -m http.server`; Node or Python projects list `npm install`, virtualenv, or database steps there.

## Usage

- **Browse course materials**: Start at `coursework/README.md` and `coursework/docs/`.
- **Work on the main group project**: `projects/inventory-management-system/`.
- **Run a bundled sample app**: Use `third_party/open_source_projects/README.md`.
- **Review Lighthouse exports**: See `artifacts/lighthouse/`.

## License

Unless otherwise noted in a subproject’s own `LICENSE` or upstream terms, course materials in this repository are provided for educational use as part of CPT304 coursework. Respect each bundled project’s original license when redistributing or modifying their code.
