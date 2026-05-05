# CPT304 Coursework — Code Wiki

## 1. Project Overview

This repository supports the **CPT304** university coursework. Its purpose is to provide a structured environment for a group project centered on auditing, researching, refactoring, and documenting a web application. The repository is organized into four top-level directories, each serving a distinct role in the course workflow.

**Course**: CPT304
**Group Repository**: https://github.com/NOSOLUTIONLOVE/CPT304_Coursework
**Primary Application**: Inventory Management System (UNIQLO-themed web app)

---

## 2. Repository Architecture

```
CPT304_Coursework/
├── coursework/          # Course materials, docs, templates, reports
├── projects/            # Writable workspace for the group's actual development
├── third_party/         # Read-only bundled third-party sample projects (10 apps)
├── artifacts/           # Generated outputs (Lighthouse JSON/HTML exports)
├── README.md            # English overview and navigation guide
├── README.zh.md         # Chinese overview and navigation guide
└── .gitignore           # Git ignore rules
```

### 2.1 Top-Level Directory Responsibilities

| Directory | Purpose | Read/Write | Key Contents |
|-----------|---------|------------|--------------|
| `coursework/` | Course requirements, workflow guides, brainstorming notes, plans, reports, and templates | Read + Write | Assignment details, workflow documentation, Lighthouse evaluation reports, project selection matrix |
| `projects/` | The group's active development workspace | **Write** | Inventory Management System (UNIQLO) — full application source with documentation |
| `third_party/` | Ten curated open-source sample apps for course analysis and comparison | Read-Only | Static front-end, full-stack (MERN, Django), and build tooling demos |
| `artifacts/` | Auto-generated analysis outputs | Generated | Lighthouse HTML/JSON reports per sample app |

---

## 3. Core Application: Inventory Management System (UNIQLO)

### 3.1 Application Summary

A single-page web application for managing inventory records with CRUD (Create, Read, Update, Delete) functionality. The UI theme is modeled after the UNIQLO clothing brand (red `#e50012` primary color).

**Technology Stack**:
- HTML5 (single-page)
- Bootstrap 5 (CSS framework)
- jQuery 3.x (DOM manipulation)
- DataTables (interactive table rendering)
- Local JSON file (`DB.txt`) as data persistence layer

**Source Locations**:
- **Active workspace**: `projects/inventory-management-system/app/`
- **Read-only sample**: `third_party/open_source_projects/inventory-management-system/`

### 3.2 Directory Structure

```
projects/inventory-management-system/
├── app/                          # Application source (SSOT)
│   ├── index.html                # Single-page application entry point
│   ├── DB.txt                    # Sample JSON data for import
│   ├── LICENSE                   # MIT License
│   ├── README.md                 # Upstream project documentation
│   ├── WORKSPACE-SOURCE.md       # Fork relationship and sync log
│   └── assets/
│       ├── bootstrap/
│       │   ├── css/
│       │   │   ├── bootstrap.min.css    # Bootstrap 5 core styles
│       │   │   ├── datatables.min.css   # DataTables styles
│       │   │   └── inventory.css        # Custom application styles
│       │   └── js/
│       │       └── bootstrap.min.js     # Bootstrap 5 JS (modals, tooltips)
│       ├── fonts/                       # FontAwesome icon fonts (woff2/woff/ttf/svg/eot)
│       ├── images/                      # UNIQLO logo, empty state animation
│       ├── img/                         # Avatars, dog images
│       └── js/
│           ├── jquery.min.js            # jQuery core
│           ├── datatables.min.js        # DataTables library
│           ├── chart.min.js             # Chart.js (unused but bundled)
│           ├── bs-init.js               # Bootstrap initialization helpers
│           └── theme.js                 # Theme-specific behaviors
├── docs/                         # Project documentation
│   ├── setup/
│   │   └── dev-environment.md    # Local development setup guide
│   ├── collaboration/
│   │   └── module-owners.md      # Module ownership and collaboration plan
│   ├── defects/                  # Defect analysis (Step 2)
│   │   ├── defect-identification.md
│   │   ├── source-defects-ranked.md
│   │   ├── step2-top4-defects.md
│   │   ├── code-review-2026-04-27.md
│   │   └── compare-audit-his-vs-ours.md
│   ├── literature/               # Literature research (Step 3)
│   │   ├── literature-workflow.md
│   │   ├── search-queries.md
│   │   ├── memory-leak-survey.md
│   │   └── p1-1-remediation-from-survey.md
│   ├── baselines/                # Baseline standards (Step 4)
│   │   └── professional-edition-checklist.md
│   ├── templates/
│   │   ├── defect-log-template.md       # Defect logging template
│   │   └── literature-matrix-template.md # Defect-literature mapping template
│   └── README.md                 # Documentation index
└── README.md                     # Workspace root documentation
```

### 3.3 Key Components

#### 3.3.1 `index.html` — Single Page Application Entry

The entire application is contained in a single HTML file. It serves as the **HTML structure**, **CSS entry points**, and **JavaScript application logic** all in one file.

**HTML Structure**:
- `#wrapper` — Outer container with Bootstrap flex layout
- `#content-wrapper` — Main content area
- `#myTable` — DataTables-powered inventory table with 7 columns:
  - Item Name, Quantity, Type, Unit Price, Total Price, Edit Action, Delete Action
- **5 Modal Dialogs**:
  - `#deleteModal` — Confirmation before record deletion
  - `#updateConfirmationModal` — Success notification after update
  - `#addConfirmationModal` — Success notification after add
  - `#editModal` — Form for editing existing records
  - `#addModal` — Form for adding new records
- **Sidebar** — Decorative icon buttons (list, chart, calendar, comment, folder)

**JavaScript Logic (inline `<script>` block, ~278 lines)**:

| Function/Block | Responsibility |
|----------------|----------------|
| `$(document).ready()` | Main initialization — sets up DataTables, event handlers, and button visibility |
| DataTables config | Defines columns, scroll, paging (disabled), search, and 4 toolbar buttons (ADD NEW ITEM, SAVE DATA, NEW DATA, LOAD DATA) |
| `delete_Row(a)` | Removes a row from the table; toggles button visibility when table becomes empty |
| `td.editor-delete` click handler | Opens delete confirmation modal; binds delete action to confirmation button |
| `td.editor-edit` click handler | Populates edit form with row data; opens edit modal |
| `#edit_data` submit handler | Sanitizes inputs, formats prices with "RM " prefix, updates row data, shows confirmation |
| `#add_data` submit handler | Sanitizes inputs, formats prices, adds new row, toggles button visibility, clears form |
| `quantity` keyup handlers (edit/add) | Real-time total price calculation: `unit_price * quantity` |
| `unit_price` keyup handlers (edit/add) | Auto-prefixes "RM " format; recalculates total price in real-time |
| `buttonShow` initialization | Iterates all `<button>` elements, assigns `.add_button` / `.data_button` classes, sets initial visibility |
| `readSingleFile(e)` | Reads uploaded JSON file via `FileReader` API |
| `displayContents(contents)` | Parses JSON, clears and repopulates DataTable, updates button states |
| `getSubstring(string, char1, char2)` | Utility for substring extraction between two characters |
| `.menu_button` hover handler | Adds/removes `.menu_select_hover` class on sidebar icons |

**Data Flow**:
```
User Action → DataTable event → Modal Dialog → Form Submit → Data Sanitize → Row Update → Confirmation Modal
     ↓
LOAD DATA → FileReader → JSON parse → DataTable populate
     ↓
SAVE DATA → DataTables exportData → Blob → File download (DB.txt)
```

#### 3.3.2 `inventory.css` — Custom Styles

Custom styles that override and extend Bootstrap defaults:

| Selector | Purpose |
|----------|---------|
| `tr` | Fixed row height (50px) |
| `th` | Top and bottom borders |
| `input[type=search]` | Custom search input styling (Nunito font, rounded corners) |
| `button.add_button` | White button style for "ADD NEW ITEM" and "NEW DATA" |
| `button.data_button` | Black button style for "SAVE DATA" and "LOAD DATA" |
| `button.modal_button` | Red UNIQLO-branded modal action buttons |
| `.menu_select_hover` | Sidebar icon hover effect with opacity transition |

#### 3.3.3 `DB.txt` — Data File

JSON-formatted sample inventory data. Loaded via the `LOAD DATA` button and exported via `SAVE DATA`. Structure:

```json
{
  "body": [
    ["Item Name", "Quantity", "Type", "Unit Price", "Total Price"],
    ...
  ]
}
```

#### 3.3.4 Supporting JavaScript Files

| File | Purpose |
|------|---------|
| `assets/js/jquery.min.js` | jQuery core library (DOM manipulation, AJAX, events) |
| `assets/js/datatables.min.js` | DataTables library (table rendering, sorting, filtering, export) |
| `assets/js/chart.min.js` | Chart.js (bundled but not currently used in the app) |
| `assets/js/bs-init.js` | Bootstrap initialization helpers (likely from AdminBSB template) |
| `assets/js/theme.js` | Theme-specific JavaScript behaviors (likely from AdminBSB template) |

---

## 4. Coursework Module

### 4.1 Directory Structure

```
coursework/
├── CPT304_Coursework/
│   ├── CPT304_Coursework_01_Details.md     # Full analysis of the assignment PDF
│   ├── CPT304_Coursework_01_Guideline.md   # Assignment guidelines
│   └── cpt304-coursework-01-complete-workflow.md  # 6-step workflow with Mermaid diagrams
└── docs/
    ├── brainstorms/     # Requirement analysis and brainstorming documents
    ├── guides/          # Playbooks and how-to guides
    ├── plans/           # Feature and refactoring implementation plans
    ├── reports/         # Lighthouse comparative analysis and evaluation reports
    └── templates/       # Project selection matrix template
```

### 4.2 Course Workflow (6 Steps)

The coursework follows a mandatory 6-step process:

| Step | Name | Key Deliverable |
|------|------|-----------------|
| 1 | Project Selection | Fork one of 10 specified projects |
| 2 | Audit Source Code | Identify 4 specific source code defects |
| 3 | Research | Find 1 external literature reference per defect (4 total) |
| 4 | Refactor | Fix 4 defects + meet 5 minimum baseline standards |
| 5 | Documentation | Write report.pdf (1500 ±10% words) |
| 6 | Assessment | Submit GroupXX.zip with all deliverables |

### 4.3 Five Minimum Baseline Standards (Step 4)

| ID | Standard | Requirement |
|----|----------|-------------|
| B1 | Live Uptime | Deployed on Vercel/Render, online ≥ 7 days |
| B2 | Test Coverage | ≥ 80% coverage via Codecov/Istanbul |
| B3 | Accessibility | Lighthouse Accessibility score ≥ 90 |
| B4 | Internationalization | At least 2 switchable languages |
| B5 | Legal Compliance | Functional Cookie banner + Privacy Policy page |

---

## 5. Third-Party Sample Projects

Located in `third_party/open_source_projects/`, these are **read-only** reference copies of 10 open-source web applications used for course analysis and Lighthouse benchmarking.

| # | Project | Type | Primary Technologies |
|---|---------|------|---------------------|
| 1 | `advanced-finance-tracker` | Static front-end | HTML/CSS/JavaScript |
| 2 | `biztrack` | Static front-end | HTML/CSS/JavaScript |
| 3 | `budget-app` | Static front-end | HTML/CSS/JavaScript |
| 4 | `e-commerce` | Static front-end | Vanilla JS, Bootstrap, pre-compiled SASS |
| 5 | `employee-management-system` | Full-stack | MongoDB, Express, React (Vite), Node.js |
| 6 | `inventory-app-js` | Front-end build | Webpack, Babel, Tailwind CSS |
| 7 | `inventory-management-system` | Static front-end | HTML, Bootstrap, jQuery, DataTables |
| 8 | `polln` | Full-stack | Python, Django, MySQL |
| 9 | `seat-booking-app` | Static front-end | HTML/JS, optional Sass |
| 10 | `taskify` | Server-rendered | Node.js, Express, EJS templates |

Detailed installation and run instructions are documented in `third_party/open_source_projects/README.md`.

---

## 6. Artifacts Module

Located in `artifacts/`, this directory stores **regenerable** analysis outputs.

```
artifacts/
└── lighthouse/          # Per-app Lighthouse report exports
    ├── advanced-finance-tracker/    # .html + .json
    ├── biztrack/                    # .html + .json
    ├── budget-app/                  # .html + .json
    ├── e-commerce/                  # .html + .json
    ├── employee-management-system/  # .html + .json
    ├── inventory-app-js/            # .html + .json
    ├── inventory-management-system/ # .html + .json
    ├── polln/                       # .html + .json
    ├── seat-booking-app/            # .html + .json
    └── taskify/                     # .html + .json
```

Each subdirectory contains:
- `.html` — Human-readable Lighthouse report
- `.json` — Machine-readable Lighthouse data (scores, audits, metrics)

---

## 7. Dependency Relationships

### 7.1 Application Dependencies

```
index.html
├── bootstrap.min.css     (Bootstrap 5 core styles)
├── datatables.min.css    (DataTables table styles)
├── inventory.css         (Custom app styles)
├── fontawesome-all.min.css / font-awesome.min.css (Icon fonts)
├── Google Fonts (Nunito, Alata, Alatsi, etc.)
├── jquery.min.js         (jQuery 3.x — loaded before DataTables)
├── datatables.min.js     (DataTables — depends on jQuery)
├── bootstrap.min.js      (Bootstrap 5 JS — modals, tooltips)
├── chart.min.js          (Chart.js — bundled, unused)
├── bs-init.js            (Bootstrap init helpers)
└── theme.js              (Theme behaviors)
```

### 7.2 Module Dependencies

```
coursework/
  └── Depends on analysis of third_party/ projects

projects/inventory-management-system/
  ├── app/ is a copy of third_party/open_source_projects/inventory-management-system/
  ├── docs/ references coursework/ templates and guidelines
  └── artifacts/ are generated by running Lighthouse against apps in third_party/
```

### 7.3 Data Flow

```
third_party/ samples  ──Lighthouse──▶  artifacts/lighthouse/
                                         │
coursework/ guidelines ───────────────▶  projects/ (group development)
                                         │
projects/ improvements ──Lighthouse──▶  artifacts/ (updated reports)
```

---

## 8. Project Setup and Running

### 8.1 Prerequisites

- Modern browser (Chrome / Edge / Firefox / Safari)
- Text editor (VS Code recommended)
- **Optional**: Python 3 **or** Node.js (for local HTTP server)
- **Optional**: Node.js (if introducing test coverage or build tooling)

### 8.2 Running the Inventory Management System

**Method A — Python HTTP Server (recommended for static preview)**:
```bash
cd projects/inventory-management-system/app
python3 -m http.server 5500
```
Then open: `http://127.0.0.1:5500/`

**Method B — VS Code Live Server**:
1. Install the "Live Server" extension
2. Right-click `index.html` → "Open with Live Server"

**Method C — Node.js server**:
```bash
cd projects/inventory-management-system/app
npx --yes serve -p 5500
```

**Important**: Do not open `index.html` directly via `file://` protocol. Using a local HTTP server ensures consistent script behavior and allows proper Lighthouse analysis.

### 8.3 Running Third-Party Samples

See `third_party/open_source_projects/README.md` for per-project setup instructions. Static projects use `python3 -m http.server`, while full-stack projects (employee-management-system, polln, taskify) require Node.js or Python environments with database setup.

### 8.4 Common Ports

| Application | Default Port |
|-------------|-------------|
| Inventory Management System | 5500 |
| Python http.server (samples) | 8080 |
| employee-management-system (backend) | 8000 |
| employee-management-system (Vite frontend) | 5173 |
| polln (Django) | 8000 |
| taskify | 3000 (set via `.env`) |

---

## 9. Module Collaboration Model

The application is divided into **5 functional domains** for parallel development:

| Domain | Responsibilities | Key Files |
|--------|-----------------|-----------|
| Shell & Accessibility | `lang` attribute, landmarks, heading hierarchy, skip links, focus management | HTML skeleton, global CSS |
| Table & CRUD | DataTables config, add/edit/delete modals, toolbar, data export | `index.html` table section, inline scripts |
| Local Persistence | LOAD DATA / SAVE DATA, DB.txt format, error handling for malformed data | FileReader API, Blob export |
| Styling & Branding | Bootstrap overrides, `inventory.css`, responsive design, color contrast | CSS files, icon font loading |
| Quality & Baselines | Test coverage, deployment, Lighthouse audit, i18n, Cookie/privacy compliance | CI config, hosting, checklist |

**Collaboration conventions**:
1. Pull from `main` before starting; make small, frequent commits
2. Changes to persistence format must be communicated to the whole team
3. Accessibility and i18n strings are intertwined — dual review recommended before merging

---

## 10. Key Design Decisions

| Decision | Rationale |
|----------|-----------|
| Single HTML file | Original upstream design; all logic is inline for simplicity |
| No database backend | Uses local JSON file for portability; data is imported/exported manually |
| DataTables for rendering | Provides built-in sorting, searching, scrolling, and export capabilities |
| jQuery event delegation | Used for dynamically created table cells (edit/delete buttons) |
| "RM " currency prefix | Hardcoded Malaysian Ringgit format in price input/output |
| No `file://` protocol | HTTP server required for consistent behavior and Lighthouse compatibility |

---

## 11. Known Issues and Technical Debt

| Issue | Location | Severity |
|-------|----------|----------|
| All JavaScript is inline in `index.html` | `index.html` | High — difficult to test, maintain, or reuse |
| No automated tests | Entire app | High — required for 80% coverage baseline |
| Hardcoded currency ("RM ") | Price input handlers | Medium — not internationalized |
| Unused Chart.js bundled | `assets/js/chart.min.js` | Low — increases load size |
| Decorative sidebar buttons with `alert()` | Sidebar icon click handlers | Low — poor UX |
| No semantic HTML landmarks | `index.html` body structure | Medium — accessibility impact |
| Missing `lang` attribute on `<html>` | `index.html` | Medium — accessibility and i18n impact |
| Mixed inline styles and CSS | Throughout `index.html` | Medium — violates separation of concerns |

---

## 12. Glossary

| Term | Definition |
|------|------------|
| CPT304 | Course code for this coursework module |
| CRUD | Create, Read, Update, Delete — core data operations |
| DataTables | jQuery plugin for advanced HTML table interactions |
| Lighthouse | Google's automated tool for auditing web page quality (performance, accessibility, SEO, best practices) |
| axe DevTools | Accessibility testing browser extension |
| i18n | Internationalization — supporting multiple languages |
| a11y | Accessibility — making web content usable by all users |
| SSOT | Single Source of Truth — authoritative location for code |
| Mermaid | Markdown-compatible diagram generation tool |
| Turnitin | Plagiarism detection system used for submission |
