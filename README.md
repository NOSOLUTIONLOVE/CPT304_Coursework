# CPT304 Coursework

## Project Overview

This repository holds **CPT304** coursework materials. The main body of work lives under `Open_Source_Project/`, which bundles **ten open-source project** copies (static sites, Node/Express, Django, and mixed stacks), each with local run instructions. A `Lighthouse/` folder stores performance audit artifacts (for example, Taskify-related reports).

## Features

- **Curated open-source samples** — front-end-only apps, full-stack examples (MongoDB/Express/React, Django/MySQL), and tooling demos (Webpack, Tailwind, etc.).
- **Documented local setup** — per-project install and run steps for macOS, Linux, and Windows are described in `Open_Source_Project/README.md`.
- **Course artifacts** — Lighthouse JSON/HTML outputs for assignment or report use.

## Installation

1. Clone this repository (SSH example):

   ```bash
   git clone git@github.com:NOSOLUTIONLOVE/CPT304_Coursework.git
   cd CPT304_Coursework
   ```

2. Open `Open_Source_Project/README.md` and follow the section for the project you need. Most static projects only require a browser or `python3 -m http.server`; Node or Python projects list `npm install`, virtualenv, or database steps there.

## Usage

- **Browse coursework**: Inspect `Open_Source_Project/` and the nested project folders.
- **Run a specific app**: Use the matching section in `Open_Source_Project/README.md` (paths, env files like `.env`, and ports are documented per project).
- **Review audits**: Open files under `Lighthouse/` in a browser or editor as needed.

## License

Unless otherwise noted in a subproject’s own `LICENSE` or upstream terms, course materials in this repository are provided for educational use as part of CPT304 coursework. Respect each bundled project’s original license when redistributing or modifying their code.
