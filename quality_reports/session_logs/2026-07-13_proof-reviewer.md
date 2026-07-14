# Session Log: Add proof-correctness checking to `/referee`

**Date:** 2026-07-13
**Plan:** `/home/simon/.claude/plans/misty-bouncing-truffle.md`

## Goal

Add an explicit correctness check for formal results (theorems/propositions/lemmas/proofs)
to the referee pipeline, which previously had no proof-logic review.

## Approach

- New conditional specialist `proof-reviewer` (read-only), modeled on consistency-reviewer
  (feeds concerns + a dedicated report section) but *also* carries a 1–5 rating.
- New pure `theory` paper type; empirical rating rows go N/A for it.
- Output surfaces two ways: `## Formal Results Check` report section **and** a
  `Theoretical Rigor / Proofs` rating dimension (N/A when the paper has no formal results).
- Trigger is conditional: orchestrator detects formal results at ingest and only then launches it.

## Key design decisions (from user)

1. Both a section and a rating dimension, with N/A allowed.
2. Conditional launch (not always-on like consistency).
3. Add a pure `theory` type and adapt the ratings table.

## Boundaries codified

- proof-reviewer = correctness/completeness of formal results.
- methods-reviewer (theory+empirics) keeps prediction→test mapping, defers proof correctness.
- consistency-reviewer = numeric agreement; contribution-reviewer = importance/novelty.

## Anti-fabrication

Proof-gap claims are absence-class (highest bar): quote the exact step, distinguish
demonstrated error from suspected gap, check the appendix before asserting a proof is missing.

## Status

**Implemented.** New `proof-reviewer` agent + `theory` paper type wired through the pipeline:
14 files edited + 1 new agent. Structural verification passed (enum sweep clean, frontmatter
valid, fences balanced, stale counts fixed). Not yet committed (awaiting user). End-to-end dry
run on a formal-results paper still pending — `papers/` only holds a Nature (empirical) PDF.

## End-to-end verification (2026-07-13) — PASSED

Live `/referee` run exercised the new path cleanly:
- Ingest classified paper type and set the formal-results flag = yes (Thm 1–4, Prop 1–4,
  Lemmas 1–10) in the brief.
- `proof-reviewer` **launched conditionally** as the 6th specialist alongside the five core.
- The report rendered a populated **Formal Results Check** section and a **Theoretical Rigor /
  Proofs** rating; empirical rows applied (not a pure-theory paper).
- The feature drove the outcome: the proof reviewer's **demonstrated ERROR in Proposition 3** 
  is the FATAL that drove the **Reject** recommendation — a flaw the old five-reviewer panel had no lens for. Independently
  corroborated by the methods, contribution, consistency, and adversarial passes.
- Report saved to `referee_reports/` (gitignored); brief in `.work/` (gitignored).

Still pending: a pure-`theory` paper (no empirics) to confirm the empirical rating rows render
N/A. Not blocking.

## Open questions / follow-ups

- Open PR for `feat/proof-reviewer` → main (this session).
