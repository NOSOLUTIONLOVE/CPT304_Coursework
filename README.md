# CPT304 Coursework

**Languages:** English | [简体中文](README.zh.md)

## Project Overview

This repository holds **CPT304** coursework materials, organized into four top-level areas for fast navigation:

- `coursework/` — course workflow, docs, templates, reports
- `projects/` — writable project workspaces (your group’s actual development)
- `third_party/` — bundled third-party samples (read-only reference)
- `artifacts/` — generated outputs (e.g. Lighthouse JSON/HTML exports)

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
