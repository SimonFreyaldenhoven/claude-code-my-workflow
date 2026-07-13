# Referee Report Protocol

Defines the structure, tone, and rating rubric for every report produced by `/referee`. The goal is a report an editor at a top economics journal could act on directly.

## Tone

- **Constructive but rigorous.** Every criticism is paired with a suggestion or the specific analysis that would resolve it. Harsh on the work, respectful to the authors.
- **Specific, not generic.** Reference exact sections, equations, tables, figures, pages. "The identification is weak" is useless; "the parallel-trends assumption (Sec. 4.2) is untested — no pre-trend plot is shown for the 2015 cohort" is a referee comment.
- **Calibrated.** A *major* concern changes the conclusions if it's right; a *minor* concern improves the paper without threatening its claims. Separately, classify each major concern by how the authors must respond (below).
- **What would change my mind.** Every major concern states the specific evidence, test, or analysis that would resolve it — not just what is wrong.
- **Honest about uncertainty.** If something couldn't be verified from the PDF, say so; don't assert it.

## Concern classification

Every **major** concern is tagged with how it bears on the decision:
- **FATAL** — the design/contribution cannot support the claim as stated. Drives a reject.
- **ADDRESSABLE** — a real problem, fixable with revision or added analysis.
- **TASTE** — a defensible referee preference, not a true flaw; the authors may push back.

These roll up into an action summary: **MUST address** (FATAL + serious ADDRESSABLE) · **SHOULD address** (other ADDRESSABLE) · **MAY push back** (TASTE).

## Recommendation scale

- **Accept** — publishable essentially as is (rare).
- **Minor Revision** — small fixes, no new analysis needed.
- **Major Revision (R&R)** — promising, but key concerns must be resolved with additional work.
- **Reject & Resubmit** — fundamental rework needed, but a future version could be viable.
- **Reject** — a fatal flaw the current design cannot overcome.

## Report structure (structured-critique format)

```markdown
# Referee Report: [Paper Title]

**Date:** YYYY-MM-DD
**Documents:** [main manuscript; + appendix/supplement if provided]
**Calibrated to:** [journal name / generic top-5]
**Paper type:** [reduced-form / structural / theory+empirics / descriptive / theory]
**Prepared by:** /referee pipeline

## Summary Assessment
**Recommendation:** [Accept / Minor Revision / Major Revision / Reject & Resubmit / Reject]

[2–3 paragraphs: the paper's question and contribution in the referee's own words, the
main strengths, and the decisive concerns that drive the recommendation.]

## Action Summary
- **MUST address:** [the FATAL and serious ADDRESSABLE items, by MC number]
- **SHOULD address:** [other ADDRESSABLE items]
- **MAY push back:** [TASTE items the authors could reasonably contest]

## Strengths
1. …

## Major Concerns
### MC1: [title]  — [FATAL / ADDRESSABLE / TASTE]
- **Dimension:** Contribution / Identification / Econometric Specification / Theoretical Rigor / Proofs / Literature / Writing / Internal Consistency
- **Issue:** [what and why it threatens the conclusions]
- **Evidence:** [exact location]
- **What would change my mind:** [the specific test, evidence, or revision that resolves it]

[MC2, MC3, … — most severe first; FATAL items lead]

## Minor Concerns
### mc1: [title]
- **Issue:** [description] — **Location:** […]
- **Suggestion:** [fix]

## Consistency Check
An audit of the paper's internal numeric agreement (from the consistency-reviewer). No dimension rating; a confirmed mismatch also appears as a concern above, tagged to the dimension it threatens.
- **Verified:** [one line — the reconciliations cross-checked and found consistent]
- **Issues:** [each mismatch: quantity — doc/loc A = X vs. doc/loc B = Y — which value is likely correct] — or "none; internally consistent on the quantities checked"

## Formal Results Check
An audit of the correctness and completeness of the paper's formal results (from the proof-reviewer). **Included only when the paper contains theorems/propositions/lemmas/formal derivations.** Unlike the consistency audit it *does* carry a rating (Theoretical Rigor / Proofs, below); a confirmed error or serious gap also appears as a concern above, tagged to that dimension.
- **Formal results:** [inventory — each theorem/proposition/lemma/corollary + where proved] — or "none; no formal results — not applicable"
- **Verified:** [the results whose proofs check out]
- **Issues:** [each finding: result — exact step/location — ERROR (demonstrated flaw) vs. GAP (unjustified step) — what would resolve it] — or "none; proofs check out on the results reviewed"

## Referee Objections
The hardest questions the authors must be able to answer:
### RO1: [pointed question]
- **Why it matters:** [why it could be decisive]
- **What would change my mind:** [response or additional analysis that would resolve it]

[3–5 objections]

## Specific Comments
[Optional section/line-level notes: typos, notation, table fixes, by location. Mark web-sourced literature claims with [web].]

## Ratings
| Dimension | Rating (1–5) |
|-----------|:------------:|
| Contribution / Importance | N |
| Identification | N |
| Econometric Specification | N |
| Theoretical Rigor / Proofs | N / N/A |
| Literature & Positioning | N |
| Writing & Presentation | N |
| **Overall** | **N** |
```

## Rating rubric (1–5, per dimension)

- **5** — Exemplary; nothing a referee would insist on.
- **4** — Strong; minor improvements only.
- **3** — Credible but with real, addressable gaps.
- **2** — Serious problems that must be fixed before the claim holds.
- **1** — The dimension undermines the paper.

The **Overall** rating and the recommendation are the referee's judgment informed by the dimensions — **not** a mechanical average. A single FATAL concern caps Overall regardless of how polished the rest is — and a **demonstrated proof error** in a load-bearing result is such a FATAL. Ratings and dimensions are calibrated to the target journal's bar when one is given.

**N/A conventions.** **Theoretical Rigor / Proofs** is rated only when the paper contains formal results — otherwise it is **N/A**. For a **pure-theory** paper the empirical rows (Identification, Econometric Specification) are **N/A**, and Theoretical Rigor carries the methodological weight together with Contribution. Do not average N/A rows into Overall.
