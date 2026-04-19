# Artemis Finance – Credit Platform MVP

Product case study + working MVP of a multi-app credit platform 
for microfinance and small-ticket lending.

**Live demo:** https://artemis-hub.replit.app  
**Docs & source:** https://github.com/arthursmt/artemis-finance

---

## How this was built — AI-assisted product development

This project was built end-to-end by a solo PM using an 
AI-assisted development workflow:

| Tool | Role in this project |
|---|---|
| **Replit AI** | Code generation, UX/UI scaffolding, architecture, prototyping |
| **Claude (Anthropic)** | Critical evaluation, QA, bug fixing, product logic review |
| **ChatGPT** | Brainstorming, documentation drafting, bug fixing |

No traditional engineering team. No designer. This is what 
AI-augmented product building looks like in practice: a PM 
who can scope, design, build, and ship a working product 
using AI as the execution layer.

This workflow mirrors how lean product teams will operate 
going forward — and this project is proof of concept.

---

## Ecosystem map

HUNT (Field App) → ARISE (Service Hub / API) → GATE (Backoffice)

- **Hunt:** field agent app — creates proposals, captures 
  client data and evidence
- **ARISE:** validates, stores, and exposes proposal lifecycle 
  endpoints
- **Gate:** back-office review by stage, workflow and 
  operational visibility

---

## Why this product exists

Traditional microfinance and small-ticket lending still rely on:
- Paper forms and manual data entry
- Fragmented tools (WhatsApp, spreadsheets, legacy core systems)
- No support for group lending
- Poor portfolio visibility at the agent level

Artemis Finance is designed around two market realities:

1. Many borrowers are **thin-file customers** — immigrants, 
   informal workers, foreign nationals with no credit history
2. Most credit origination happens via **human relationships 
   in the field**, not inside branches

The MVP focuses on the **field agent experience**: reduce 
operational risk with clear workflow and validations while 
keeping the UI fast and simple under time and connectivity 
pressure.

---

## What the MVP does

From the perspective of a credit agent:

- View ongoing proposals and resume partially completed 
  applications
- Start a new credit proposal for an individual or group
- Build a group loan with up to 5 members
- Capture loan configuration, personal data, and business 
  data per member

**Implemented:**
- Proposals dashboard with status, actions, and data persistence
- Group loan structure with leader logic and member ordering
- Loan Details tab: APR calculation, installment engine, 
  insurance logic, payment summary
- Personal Data tab: full validation, contact numbers, references

**Roadmap:**
- Business Data and Financials (P&L) tabs
- Risk score based on member data + loan configuration
- Role-based flows for credit committee and operations
- Audit trail and activity log
- Export to core banking / LOS system

---

## Product decisions worth noting

**Loan Details UX:** Designed for tablet usability with quick 
input buttons (+500, +1k) to minimize typing in field conditions. 
Business rules enforced inline — no silent failures.

**Insurance logic:** Cascading optional insurances with 
transparent monthly payment summary. Built to reduce agent 
disputes about real loan cost — a known pain point in 
microfinance field operations.

**Group lending:** Member tab ordering is stable with 
intentional leader assignment. Proposals persist so agents 
can resume across sessions — critical for low-connectivity 
environments.

**Thin-file design:** Document type flexibility (SSN, 
Driver's License, foreign ID) and reference capture built 
into personal data — addressing the real customer profile 
of underserved borrowers.

---

## Tech stack

| Layer | Technology |
|---|---|
| Frontend | React + TypeScript + Vite |
| Backend | Express (Node.js) |
| Styling | Tailwind utility classes |
| Persistence | SQL/ORM scaffolding |
| Hosting | Replit (published) |
| AI development | Claude, ChatGPT, Replit AI |

Repository structure:
- `client/` — Hunt/Gate frontend apps
- `server/` + `backend/` — API and runtime handlers
- `shared/` — shared types, proposal store, domain models
- `attached_assets/` — design and iteration assets

---

## Running locally

```bash
git clone https://github.com/arthursmt/artemis-finance.git
cd artemis-finance
npm install
npm run dev
```

Server runs on port 5000. Validate with:
```bash
curl http://localhost:5000/
curl http://localhost:5000/healthz
```

**Note on Replit Preview:** The embedded preview may fail 
due to Vite + CSP/iframe behavior. Use the published URL 
directly for demos: https://artemis-hub.replit.app

---

## For recruiters and reviewers

This project demonstrates:

- **Product thinking:** lending problem framed from user 
  research, policies translated into product constraints
- **AI-augmented execution:** built solo using Claude, 
  ChatGPT, and Replit AI as the engineering layer
- **UX judgment:** tablet-first flows for field agents 
  under time and connectivity pressure
- **Edge case handling:** date validation, amount rules, 
  group structure, cascading logic
- **Modern PM skill set:** concept → working MVP, 
  documented and deployed, without a traditional eng team

The goal is not to demonstrate coding skills. It is to 
demonstrate that a PM who understands product deeply enough 
can ship real software using AI — and document it honestly.
