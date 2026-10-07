# DESIGN.md — ABC Collateral Register

## Direction

Design an evidence-led finance register: executive answer first, traceable detail second. Keep screens quiet, dense, and scannable. Use contrast and alignment to explain status; avoid marketing-style decoration. This is an original system informed by clear fintech data presentation, not a reproduction of any company's brand.

## Color roles

| Token | Value | Use |
| --- | --- | --- |
| Paper | `#F5F7F2` | Main page background |
| Ink | `#1A2620` | Main text, page bars, high-priority figures |
| Muted | `#5A6A62` | Captions, helper text, secondary labels |
| Line | `#C3CDC6` | Table rules and quiet separators |
| Free | `#1B6A48` | Available collateral and positive balances |
| Free wash | `#D7EEE3` | Background for free-value KPI and state |
| Pledged | `#3A4760` | Active allocation / pledged state |
| Pledged wash | `#E2E6EE` | Active allocation row and status |
| History | `#B86700` | Audit/history events and effective-date changes |
| History wash | `#FFF3D8` | Historical change strip |
| Exception | `#9B3418` | Validation failures and actionable risks only |
| Exception wash | `#F6E2DA` | Error or blocked state |

Do not use green to imply a bank-approved borrowing limit. Label the metric as book-value free collateral when that distinction matters.

## Typography

- Page and section titles: Georgia, bold; keep titles compact and editorial.
- Body and labels: Helvetica, regular; use sentence case for explanatory text.
- Amounts, identifiers, formulae, and timestamps: Courier New with tabular-looking alignment.
- Use clear size steps: title 24–28 px, body 12–14 px, compact labels 10–12 px, KPI value 28 px.

## Layout and data behavior

- Put the as-of timestamp beside the register/report title, not in a footnote.
- The first screen answers the question: total parent value, active allocations, and free balance at one consistent point in time.
- The register screen drills into the parent asset, then current allocation, bank/contract, effective period, status, and audit history.
- Distinguish event-recorded time from business-effective time. An as-of view includes an allocation only when its effective interval contains that timestamp and its status is active.
- Released/expired rows remain discoverable in history but do not reduce the as-of free balance. A reallocation is counted once at its current destination.
- Align all monetary values to the right. Keep parent and child values visually distinct; never label a child allocation as the parent's remaining value.
- Use flat rectangular table geometry and hairline separators for spreadsheet-like scanability. Use surface contrast instead of shadows; reserve colored fills for semantic states.

## Slide sequence

1. Executive total at selected snapshot.
2. Collateral Register drill-down and audit history for that same snapshot.
3. Creation, validation, and lifecycle flow.
4. Actors and use cases.
5. Data risks and controls.

## Guardrails

- Do not treat a status change alone as legal release; require the business-confirmed effective release date.
- Do not mix values from different snapshot timestamps.
- Do not present book-value free collateral as an approved loan limit.
- Do not copy proprietary logos, exact brand assets, or branded components from a reference system.
