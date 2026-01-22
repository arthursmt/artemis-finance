# Artemis Finance – Credit Platform MVP

Product case study + working MVP of a multi-app credit platform for microfinance and small-ticket lending.

**Live demo (Published):** https://artemis-hub.replit.app

---

## Ecosystem map (MVP)

HUNT (Field App)  →  ARISE (Service Hub / API)  →  GATE (Backoffice)

- **Hunt:** creates proposals, captures client data and evidence (docs/photos)
- **ARISE:** validates, stores, and exposes proposal lifecycle endpoints (stages)
- **Gate:** reviews proposals by stage, supports workflow and operational visibility

---

## Repository structure (current)

- `client/` → Frontend app(s) (Hunt/Gate UI depending on module)
- `server/` → Backend runtime / API handlers
- `backend/` → Backend services / integration layers (ARISE components)
- `shared/` → Shared types/components/utilities across apps
- `attached_assets/` → Screenshots and diagrams

---

## About this project – my perspective

This project demonstrates how I:
- scoped product requirements from research
- designed interaction flows for agents and back office users
- implemented core screens with accessible validations
- translated business rules into UI + data constraints
- documented limitations, trade-offs, and next steps

---

## 1. Overview

This repository demonstrates the design and implementation of a digital credit platform aimed at supporting credit agents in field operations. The MVP focuses on the agent experience, capturing client proposals, enforcing business rules, and enabling workflow from data capture to proposal review.

Traditional microfinance and small-ticket lending still rely heavily on:
- paper forms and manual data entry  
- fragmented tools (WhatsApp, spreadsheets, legacy core systems)  
- little/no support for **group lending**  
- poor visibility of portfolio quality at the agent level  

Artemis Finance is a product concept designed around two realities:

1) Many borrowers are **thin-file customers** (immigrants, foreign nationals, informal workers).  
2) Most credit origination still happens via **human relationships in the field**, not inside branches.

This MVP focuses on the **field agent experience**: reduce operational risk with clear workflow + validations while keeping the UI fast and simple for agents working under time/connectivity pressure.

---

## 2. What this MVP does

From the perspective of a **credit agent**, the Artemis Hunt MVP allows them to:

- view an **On Going Proposals** list and resume partially completed applications  
- start a **new credit proposal** for an individual or group  
- build a **group loan** with up to **5 members**  
- capture each member’s:
  - loan configuration (**Loan Details**) ✅
  - personal data ✅
  - business data (planned)
  - simple P&L / financials (planned)

The current implementation focuses on two screens:

1) **Proposals List (Dashboard)**
   - shows ongoing proposals with client name, amount, status, and actions
   - allows the agent to open, continue filling, view details, or delete
   - preserves data so proposals can be resumed later

2) **Product Configuration / Member Form**
   - header with **Group ID**, **Leader name**, **base credit rate**
   - tabs for each member (1…N), keeping the leader always in position #1
   - per member, sub-tabs:
     - **Loan Details** ✅
     - **Personal Data** ✅
     - **Business Data** (placeholder)
     - **Financials (P&L)** (placeholder)

---

## 3. Loan Details – UX and business rules

The **Loan Details** tab is designed for tablet usability while enforcing key credit policies.

### Fields and behavior

- **Loan value ($)**
  - formatted as US currency (supports cents/decimals)
  - quick buttons: `+500`, `+1k`, `-500`, `-1k`
  - rules:
    - min: **$500**
    - max: **$50,000**
    - inline errors when out of range

- **Loan type**
  - dropdown (Working capital, Investment, Other)
  - default: **Working capital**

- **Interest rate (APR, % per year)**
  - non-editable
  - default: **14% APR fixed** (base credit rate shown in header)

- **Number of installments**
  - dropdown from **3 to 12 months**
  - recalculates monthly installment

- **First payment date**
  - date picker rules:
    - within **60 days** from today
    - on or before the **15th** of the chosen month
  - invalid choices show an explanatory error

- **Grace period (days)**
  - read-only
  - calculated as days between today and the selected first payment date

- **Loan goal**
  - dropdown (Inventory, Equipment purchase, Working capital, Debt consolidation, Other)
  - selecting **Other** reveals **Other goal (optional)** text field

---

## 4. Insurance logic & monthly payment summary

The app helps the agent explain the **real monthly cost** of the loan.

### Borrower’s Insurance (Credit Life)

- toggle: Yes/No (default **Yes**)
- premium: **2% of principal** (no interest)
- UI shows:
  - total credit life cost
  - monthly share included in payment summary

### Optional insurances

Three cascading dropdowns:

1) optional insurance 1  
2) optional insurance 2 (only appears if #1 ≠ None)  
3) optional insurance 3 (only appears if #2 ≠ None)

Options and example pricing:

- **Health Plus** – $40/month  
- **Work Loss** – $20/month  
- **Income Protection** – $30/month  
- **None**

Behavior:
- dropdown label includes monthly price
- info icon (`i`) shows a short explanation
- Summary adds up selected premiums

### Summary box

At the bottom of Loan Details:

- base monthly installment (principal + interest, fixed-installment formula)
- credit life monthly share
- optional insurances total monthly
- interest rate (14% APR)
- first payment date + due day of month
- **total monthly payment** = installment + insurances

This supports transparent conversations and reduces disputes about the real payment.

---

## 5. Personal Data tab

Each member has a **Personal Data** form with validation for mandatory fields:

- First / Middle / Last name  
- Document type (SSN, Driver’s License, etc.)  
- Document ID  
- Country of origin  
- Birth date  
- Address 1 & 2  
- State, City, ZIP  
- Up to **3 contact numbers** (type + number)  
- Up to **2 references** (name + phone)  

Mandatory fields are clearly marked and validated when saving or navigating.

---

## 6. Group management & leader logic

The proposal is always a **group**, even if it has one member.

- add members up to **five**
- member tab ordering is stable (no unexpected reordering)
- leader is always **Member 1**
- option to **change leader**:
  - dialog lists existing members
  - chosen leader becomes #1
  - other members keep relative order

Member data is persisted so proposals can be resumed from the dashboard.

---

## 7. Architecture & tech stack

This repo is organized as a small monorepo:

- `client/` – **Artemis Hunt** web app (React + TypeScript + Vite), styled for tablet use  
- `server/` + `backend/` – API/runtime scaffolding (Replit stack) for future platform evolution  
- `shared/` – shared utilities/types (proposal store, domain models)  
- `attached_assets/` – design and iteration assets  

Key technologies:
- **React + TypeScript**
- **Vite** (frontend dev server)
- **Express** (API/server)
- Tailwind-style utility classes for layout/spacing
- Persistence layer behind the proposal store (SQL/ORM scaffolding)

> Product-first MVP: the strongest emphasis is **product design, UX, and business rules**, while keeping an engineering foundation that can evolve into a fuller platform.

---

## 8. Running the project

### Local

```bash
git clone https://github.com/arthursmt/artemis-finance.git
cd artemis-finance
npm install
npm run dev
The server runs on port 5000 (Replit default). To validate locally:
curl http://localhost:5000/
curl http://localhost:5000/healthz

**Replit: Development vs Published (important)**
This repo runs in two contexts:
- Development (Replit workspace / Port 5000): npm run dev
- Published app (replit.app): https://artemis-hub.replit.app
 (source of truth for demos)

Known issue: Replit Preview panel may fail
The embedded Replit Preview runs inside an iframe and may fail to load even when the server is healthy (Vite + CSP/iframe behavior can be inconsistent in Preview).

If Preview looks blocked/empty:
Open the published URL directly: https://artemis-hub.replit.app
Or validate via curl http://localhost:5000/
* For reviewers/recruiters, the published URL is the reliable demo environment.

---

## 9. Roadmap & next steps
- complete Business Data and Financials (P&L) tabs
- add a simple risk score based on member data + loan configuration
- role-based flows for back-office teams (credit committee, operations)
- audit trail and activity log per proposal
- export proposals to a core banking / LOS system

---

## 10. Notes for reviewers & recruiters
This repo is part of a product portfolio and demonstrates:
- ability to frame a lending problem and translate policies into product constraints
- user-friendly solutions
- careful handling of edge cases and validations (dates, amounts, group structure)
- hands-on execution with a modern web stack from concept → working MVP
Questions about product decisions, trade-offs, or roadmap: feel free to open an issue or reach out via LinkedIn: https://www.linkedin.com/in/arthur-silva-maciel-tonaco/
