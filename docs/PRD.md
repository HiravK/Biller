# Product Requirements Document (PRD) — Biller — GST Invoice Generator (Spazorlabs LLP)

| Field | Value |
|---|---|
| Document ID | BILLER-PRD |
| Project | Biller — GST Invoice Generator (Spazorlabs LLP) |
| Repository | [`HiravK/Biller`](https://github.com/HiravK/Biller) |
| Version | 1.0 |
| Status | Approved — living document |
| Owner | Hirav Kadikar |
| Classification | Public |
| Last updated | 2026-09-25 |

> **Purpose:** Defines what the product must do, for whom, and how success is measured. It is the single source of truth for scope.


## 1. Revision history

| Version | Date | Author | Change |
|---|---|---|---|
| 1.0 | 2026-09-25 | Hirav Kadikar | Full documentation suite generated from a complete review of the repository. |


## 2. Executive summary

**Biller** is a self-contained invoice generator (one 90 KB `index.html`) titled *Spazorlabs LLP — Invoice Generator*.
It runs entirely in the browser: fill in the biller, buyer (with a one-click "copy buyer" to payer), invoice date, place
of supply and line items with HSN/SAC codes; the page calculates each row's pre-tax amount and tax, shows CGST/SGST/IGST
and grand totals, converts the total to words, and renders a print-ready invoice. Invoices get automatic numbers in the
format **SPZ/<financial-year>/<0001>**, duplicates are blocked, and every saved invoice goes into an on-device **history**
(browser `localStorage`) where it can be previewed, re-loaded into the form, reprinted or deleted.

No data leaves the device and nothing needs installing. The trade-off: history exists only in that one browser profile.


## 3. Problem statement

Small firms need GST-compliant invoices quickly, but accounting software is costly and spreadsheets produce
inconsistent layouts, wrong totals and duplicate invoice numbers.


## 4. Goals and non-goals


### 4.1 Goals

- Produce a clean, printable GST invoice in under two minutes.
- Never reuse an invoice number.
- Keep a searchable history without a server or account.


### 4.2 Non-goals (explicitly out of scope)

- E-invoicing (IRN/QR from the GST portal), payments or accounting ledgers.
- Multi-user sync.


## 5. Stakeholders (RACI)

| Stakeholder | Role | R/A/C/I | Interest |
|---|---|---|---|
| Hirav Kadikar | Developer and owner | R/A | Tool for Spazorlabs LLP |
| Spazorlabs LLP | Business user | C | Correct invoices |
| Clients (buyers) | Invoice recipients | I | Accurate tax invoices |


_R = Responsible, A = Accountable, C = Consulted, I = Informed._


## 6. Users and personas


### Operations / accounts person

Issues a few invoices a month.
needs[]: Pre-filled company details ;; Correct GST split ;; Print or save as PDF ;; Find old invoices


## 7. User stories

| ID | As a… | I want to… | So that… | Priority |
|---|---|---|---|---|
| US-01 | accounts person | invoice numbers generated automatically | I never duplicate or skip | Must |
| US-02 | accounts person | taxes and totals calculated per line | I avoid arithmetic mistakes | Must |
| US-03 | accounts person | the total in words | the invoice meets common format | Should |
| US-04 | accounts person | to print or save as PDF | I can send it | Must |
| US-05 | accounts person | a history of saved invoices | I can reprint or copy an old one | Should |


## 8. Functional requirements

| ID | Area | Requirement | MoSCoW | Status |
|---|---|---|---|---|
| FR-01 | Form | Capture biller/buyer/payer, GSTINs (15 chars), place of supply | Must | Done |
| FR-02 | Items | Row calculations and totals | Must | Done |
| FR-03 | Tax | Show the correct tax heads for intra-state (CGST+SGST) or inter-state (IGST) supply | Must | Partly — all three rows shown together |
| FR-04 | Numbering | Unique sequential numbers per financial year | Must | Partly — FY label wrong in Jan–Mar |
| FR-05 | Output | Print-ready invoice | Must | Done |
| FR-06 | History | Save, list, reload, delete | Should | Done |


## 9. Non-functional requirements

| ID | Category | Requirement | Current status |
|---|---|---|---|
| NFR-01 | Privacy | No data leaves the browser | Met |
| NFR-02 | Availability | Works offline from a file | Met |
| NFR-03 | Durability | Invoices safe if browser data is cleared | Not met — localStorage only |
| NFR-04 | Portability | Any modern browser | Met |


## 10. User experience and key flows


### Create and print an invoice

1. Open index.html
1. Fill buyer details (and "copy buyer" for payer if the same)
1. Add line items with HSN/SAC, quantity, rate and tax rate
1. Check totals and amount in words
1. Click Save & Print → invoice stored in history and the print dialog opens (choose "Save as PDF" if needed)


## 11. Success metrics (KPIs)

| Metric | Target | How it is measured |
|---|---|---|
| Time to issue an invoice | < 2 minutes | User timing |
| Duplicate invoice numbers | 0 | isInvNoDuplicate check |


## 12. Assumptions, constraints and dependencies


### Assumptions

_None recorded._


### Constraints

- Single HTML file; no build step, no backend.


### External dependencies

| Dependency | Used for | Risk if unavailable |
|---|---|---|
| Browser localStorage | Invoice history and counter | Lost if site data is cleared or on another device |
| Browser print | PDF output | Layout varies slightly by browser |


## 13. Release plan and roadmap

| Phase | Scope | Status |
|---|---|---|
| v1 (2026-04-01) | Invoice form, totals, print | Done |
| v1.1 (2026-04-01) | UX changes and invoice history | Done |
| v1.2 | Correct FY and GST heads, JSON export/import of history | Proposed |


## 14. Open questions

- Should history be exportable (JSON/CSV) for accounting backups?


## 15. Acceptance and sign-off

| Role | Name | Decision | Date |
|---|---|---|---|
| Product owner | Hirav Kadikar | Approved (baseline of current build) | 2026-09-25 |
