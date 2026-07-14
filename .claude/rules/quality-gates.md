# Quality Gates — Referee Reports

A referee report ships only when it would be useful to a real editor. There is no compile step; the gate is a **quality checklist**, not a numeric score.

## Ship checklist (all must hold before the report is saved)

| # | Gate | Why |
|---|------|-----|
| 1 | **Grounded** — every specific claim cites an exact location; no invented numbers or citations (`review-verification.md` passed) | A wrong objection destroys credibility |
| 2 | **Specific** — every concern names a section/table/eq/page; no vague "the paper is weak" | An editor must be able to act on it |
| 3 | **Actionable** — every concern carries a suggestion or the analysis that would resolve it | Referees help, not just judge |
| 4 | **Calibrated** — majors genuinely threaten the conclusions; minors are separated out; FATAL vs. ADDRESSABLE distinguished | Not everything is equally important |
| 5 | **Complete** — all five core dimensions covered (contribution, identification, specification, literature, writing), the adversarial and consistency passes ran, and the **proof/formal-results pass ran when the paper contains formal results** | No blind spots |
| 6 | **Fair** — genuine strengths acknowledged; tone constructive | Balanced reports carry more weight |
| 7 | **Coherent** — reviewer overlaps deduped, disagreements adjudicated, one clear recommendation with justification | It reads as one referee, not five |

## Dimension ratings

Each dimension is scored 1–5 per the rubric in `referee-report-protocol.md`. Ratings inform — but do not mechanically produce — the overall recommendation. A single fatal flaw caps the overall regardless of polish elsewhere.

## Enforcement

- **Any gate fails →** fix before saving (loop back through synthesis / fact-check). This is the orchestrator's job; see `orchestrator-protocol.md`.
- The user can always override with justification (e.g. "quick take, skip the web literature check").

## Quality reports (process meta-work)

Merge-time quality reports for changes to *this repo's infrastructure* still use `templates/quality-report.md` → `quality_reports/merges/`. Referee reports themselves are the product and live in `referee_reports/`.
