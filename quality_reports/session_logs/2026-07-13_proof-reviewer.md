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

## Open questions / follow-ups

- Live `/referee` dry run once a theorem-heavy paper is available, to confirm the conditional
  launch fires, the Formal Results Check section populates, and empirical rows show N/A for a
  pure-theory paper.
- Commit/PR when the user asks (currently on `main`; branch first).
