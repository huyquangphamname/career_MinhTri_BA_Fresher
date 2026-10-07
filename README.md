# TSĐB Tự do — take-home BA Fresher

Take-home for **BA Fresher – Finance Transformation** (Minh Trí Consulting, Oct 2026).

The module answers one leadership question: *how much collateral is still free to pledge, by asset type, at a chosen as-of time?*

```
Tự do = parent book value − Σ still-effective allocations
```

Book-value free collateral is **not** a bank-approved limit (no haircut / LTV in this formula).

## Public files

| Path | What it is |
| --- | --- |
| `submission/takehome_tsdb.html` | Interactive mock: collapse/expand register + gate 3 (allocation cannot exceed parent) |
| `submission/TSDB_TuDo_MinhTri.drawio` | Flowchart, use cases, two table states, risk/controls |
| `submission/speaker_notes.md` | 10–15 minute walkthrough |
| `submission/email_xac_nhan.md` | Confirmation email sent with the Drive submission |
| `DESIGN.md` | Visual system for the register |

Open the HTML locally in a browser. Open the `.drawio` file in [diagrams.net](https://app.diagrams.net/).

## Not in this repo

`environment/` (brief, glossary, rehearsal dialogs) and interview-prep notes stay on the local machine. They are listed in `.gitignore` so a later `git add .` cannot publish them by accident.
