# Session Handover — Biller — GST Invoice Generator (Spazorlabs LLP)

| Field | Value |
|---|---|
| Document ID | BILLER-HANDOVER |
| Project | Biller — GST Invoice Generator (Spazorlabs LLP) |
| Repository | [`HiravK/Biller`](https://github.com/HiravK/Biller) |
| Version | 1.0 |
| Status | Approved — living document |
| Owner | Hirav Kadikar |
| Classification | Public |
| Last updated | 2026-09-25 |

> **Purpose:** Lets the next person (or AI session) pick up the work cold: what exists, what state it is in, what is unfinished, and exactly what to do next.


## 1. Handover summary

| Item | Detail |
|---|---|
| Handover date | 2026-09-25 |
| Handed over by | Hirav Kadikar |
| Repository state | `main` @ `4cd972e` — 2 commits, last change 2026-04-01 |
| Overall status | Active — internal tool |
| Live URL | — |
| Health | Green — usable; two tax-format bugs to fix |


## 2. Current state (plain English)

Built and refined on 2026-04-01 for Spazorlabs LLP. It works offline and keeps history in the browser. Two correctness
issues should be fixed before relying on it for statutory invoices (see known issues).


## 3. What is done

- Invoice form, row and total calculations, amount in words
- Automatic numbering with duplicate check
- Print-ready invoice and local history


## 4. In progress / partially done

- Nothing.


## 5. Known issues and bugs

| # | Issue | Impact | Suggested fix |
|---|---|---|---|
| 1 | CGST, SGST and IGST rows are all shown when tax > 0 | Invoice appears to charge both intra- and inter-state tax | Show CGST+SGST when place of supply = supplier state, otherwise IGST only |
| 2 | Financial year uses the calendar year | Invoices dated Jan–Mar get the next FY label (e.g. 2026-27 instead of 2025-26) | FY = month ≥ April ? yr-(yr+1) : (yr-1)-yr |
| 3 | Counter never resets per FY | Numbers keep growing across years | Reset counter when FY changes |
| 4 | History only in localStorage | Loss on data clear / new device | Export/import JSON |


## 6. Next steps (prioritised)

1. Fix tax heads and FY logic.
1. Add export/import of history.


## 7. How to resume work in 10 minutes

```bash
git clone https://github.com/HiravK/Biller.git
open Biller/index.html      # or double-click it
```


## 8. Access, accounts and secrets

Secrets are **never** stored in this repository. The table lists where each credential lives, not its value.

| System | What you need | Where it lives |
|---|---|---|
| GitHub HiravK/Biller | Write | GitHub |


## 9. Gotchas and tribal knowledge

_None recorded._


## 10. Key files to read first

| File | Why |
|---|---|
| `index.html` | Entire app (calcTotals and generateInvNo need fixes) |


## 11. Recent history

```text
2026-04-01  4cd972e  Changed UX, Added history
2026-04-01  e43e1bb  Initial Commit
```


## 12. Handover checklist

- [ ] Repository builds from a clean clone using the README steps
- [ ] Environment variables documented in the README / runbook
- [ ] Open risks recorded in the risk register
- [ ] Next steps above agreed with the product owner
- [ ] Access to hosting / third-party accounts transferred or shared
