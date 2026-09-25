# Project Overview (In Depth) — Biller — GST Invoice Generator (Spazorlabs LLP)

| Field | Value |
|---|---|
| Document ID | BILLER-PROJECT |
| Project | Biller — GST Invoice Generator (Spazorlabs LLP) |
| Repository | [`HiravK/Biller`](https://github.com/HiravK/Biller) |
| Version | 1.0 |
| Status | Approved — living document |
| Owner | Hirav Kadikar |
| Classification | Public |
| Last updated | 2026-09-25 |

> **Purpose:** The complete, plain-English explanation of this project: why it exists, what it does, how every part works, how it evolved, its quality, security, risks and vocabulary.


## 1. The project in one paragraph

**Biller** is a self-contained invoice generator (one 90 KB `index.html`) titled *Spazorlabs LLP — Invoice Generator*.
It runs entirely in the browser: fill in the biller, buyer (with a one-click "copy buyer" to payer), invoice date, place
of supply and line items with HSN/SAC codes; the page calculates each row's pre-tax amount and tax, shows CGST/SGST/IGST
and grand totals, converts the total to words, and renders a print-ready invoice. Invoices get automatic numbers in the
format **SPZ/<financial-year>/<0001>**, duplicates are blocked, and every saved invoice goes into an on-device **history**
(browser `localStorage`) where it can be previewed, re-loaded into the form, reprinted or deleted.

No data leaves the device and nothing needs installing. The trade-off: history exists only in that one browser profile.


## 2. Background and why it exists

Small firms need GST-compliant invoices quickly, but accounting software is costly and spreadsheets produce
inconsistent layouts, wrong totals and duplicate invoice numbers.


## 3. Fact sheet

|  |  |
|---|---|
| Repository | Public — `HiravK/Biller` |
| Status | Active — internal tool |
| Live URL | — |
| Hosting | Static file — open index.html in a browser (no server) |
| Primary language | HTML / CSS / JavaScript (single file) |
| Default branch | `main` |
| History | 2 commits from 2026-04-01 to 2026-04-01 |
| Contributors | Hirav K (2 commits) |


## 4. Features explained


### Invoice form

Biller, buyer, payer (copy from buyer), invoice number/date, place of supply, currency, GSTINs


### Line items

Add/delete rows, header rows, HSN/SAC list (9983xx services), quantity × rate, tax rate → pre-tax and tax per row


### Totals

Pre-tax total, total tax, CGST and SGST (half each), IGST, grand total; amount in words (num2words)


### Numbering

SPZ/<FY>/<4-digit counter> with duplicate check


### Preview and print

Rendered invoice HTML and browser print (save as PDF)


### History

Saved invoices in localStorage - list, count, preview, load into form, delete, clear all


## 5. How it works end to end

One static HTML file containing markup, CSS and vanilla JavaScript. Two panels (form and history) are switched in the
page. JavaScript collects the form into an object (`collectData`), renders the printable invoice (`renderInvoiceHTML`),
and persists invoices as a JSON array in `localStorage` (`loadDB` / `saveDB`) alongside a counter used by
`generateInvNo`. Printing uses the browser's print dialog.


### Create and print an invoice

1. Open index.html
1. Fill buyer details (and "copy buyer" for payer if the same)
1. Add line items with HSN/SAC, quantity, rate and tax rate
1. Check totals and amount in words
1. Click Save & Print → invoice stored in history and the print dialog opens (choose "Save as PDF" if needed)

Diagrams and component detail: [ARCHITECTURE.md](ARCHITECTURE.md).


## 6. Technology choices

| Layer | Technology | Why it is used |
|---|---|---|
| UI | HTML + CSS | Form and invoice layout |
| Logic | Vanilla JavaScript | Calculations, numbering, history |
| Storage | localStorage | Persistence per browser |


## 7. Codebase tour

```text
Biller/
└── index.html   # the whole application
```

| Component | Location | What it does |
|---|---|---|
| Form and items | `index.html (addRow, delRow, calcRow, copyBuyer)` | Data entry and per-row math |
| Totals | `index.html (calcTotals, num2words)` | Tax heads, grand total, amount in words |
| Numbering | `index.html (getNextCounter, generateInvNo, isInvNoDuplicate)` | SPZ/FY/NNNN |
| Rendering and print | `index.html (generatePreview, renderInvoiceHTML, doPrint, saveAndPrint)` | Printable invoice |
| History | `index.html (loadDB, saveDB, renderHistory, previewSaved, loadIntoForm, deleteInv, clearHistory)` | Local storage |


## 8. Project timeline

| Phase | Scope | Status |
|---|---|---|
| v1 (2026-04-01) | Invoice form, totals, print | Done |
| v1.1 (2026-04-01) | UX changes and invoice history | Done |
| v1.2 | Correct FY and GST heads, JSON export/import of history | Proposed |

Recent commits:

```text
2026-04-01  Changed UX, Added history
2026-04-01  Initial Commit
```


## 9. Team and ownership

| Person / group | Role | Interest |
|---|---|---|
| Hirav Kadikar | Developer and owner | Tool for Spazorlabs LLP |
| Spazorlabs LLP | Business user | Correct invoices |
| Clients (buyers) | Invoice recipients | Accurate tax invoices |


## Quality and testing

No automated tests; verify with the checks below.

Acceptance checks to run before every release:

| # | Area | Check | Expected result |
|---|---|---|---|
| 1 | Totals | 2 items at 18% | CGST = SGST = half of tax; grand = pre-tax + tax |
| 2 | Numbering | Save two invoices | SPZ/<FY>/0001 then 0002 |
| 3 | Numbering | Save with an existing number | Blocked as duplicate |
| 4 | History | Save, reload page, open History | Invoice listed; can load and print |
| 5 | FY edge | Set system date to 15 Feb 2026 | Expected SPZ/2025-26/…; currently shows 2026-27 (bug) |


## Security and privacy

| Area | Current state |
|---|---|
| Authentication | None — no user accounts. |
| Authorisation | Not applicable. |
| Data handled | Business and customer details (names, addresses, GSTINs) stored in the user's browser only. |
| Secrets | No secrets required. |
| Transport | HTTPS via the hosting provider. |

| Threat | Scenario | Mitigation | Status |
|---|---|---|---|
| Information disclosure | Shared computer shows history | Use a personal browser profile; clear history | Open |
| Tampering | Edited invoice after issue | Print/archive PDFs at issue time | Open |

No data is transmitted. Anyone with access to the same browser profile can see invoice history.


## Risks and technical debt

| ID | Category | Risk | Score (L×I) | Mitigation |
|---|---|---|---|---|
| R-01 | Compliance | Incorrect tax heads or FY on statutory invoices | 16 (High) | Fix before use; review by accountant |
| R-02 | Data | Invoice history lost | 9 (Medium) | Export + PDF archive |

| Tech debt | Severity | Fix |
|---|---|---|
| Everything in one 90 KB file | Low | Acceptable for size; split if it grows |


## How to use it


### Issue an invoice

1. Open index.html
1. Fill buyer details and GSTIN; set place of supply
1. Add items (HSN/SAC, qty, rate, tax %)
1. Save & Print; choose "Save as PDF" to email it


### Find an old invoice

1. Open the History panel
1. Preview, reload into the form, or delete


## Glossary

| Term | Meaning |
|---|---|
| **CGST / SGST** | Central and State GST — used together for intra-state supplies |
| **FY** | Indian financial year, 1 April – 31 March |
| **GST** | Goods and Services Tax (India) |
| **GSTIN** | 15-character GST registration number |
| **HSN / SAC** | Codes classifying goods (HSN) and services (SAC); 9983xx are professional services |
| **IGST** | Integrated GST — used for inter-state supplies |
| **Place of supply** | Location that decides whether CGST+SGST or IGST applies |
