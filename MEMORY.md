# Project Memory

Corrections and learned facts that persist across sessions.
When a mistake is corrected, append a `[LEARN:category]` entry below.

---

<!-- Append new entries below. Most recent at bottom. -->

## Peer Review

[LEARN:review] Grounded > generated: every criticism in a referee report must cite an exact location (section/table/eq/page) and nothing is invented. A wrong objection destroys credibility faster than a missed one → always run the anti-fabrication fact-check (`review-verification.md`) before saving.

[LEARN:review] Absence claims ("no clustered SEs", "doesn't cite X", "no pre-trend test") are the highest-risk statements — verify the absence against the full PDF *including the appendix* before asserting it.

[LEARN:review] Multi-agent + adversarial beats single-pass review: 4 parallel specialists (methods, contribution, literature, writing) + a toughest-referee pass, then a synthesizer that dedupes/reconciles/ranks. The overall recommendation is the referee's judgment, not a mechanical average of dimension ratings.

[LEARN:review] The paper brief is orientation for sub-agents, not authoritative. Reviewers must verify load-bearing claims against the source PDF, and the orchestrator re-verifies anything that drives a major concern or the recommendation.

[LEARN:review] Journal calibration is opt-in: `--journal [X]` tunes reviewers to a target journal's bar via `.claude/references/journal-profiles.md`; with no flag, reviewers default to generic top-5. Don't force a journal choice on the common case.

[LEARN:review] Reviews are paper-type aware (reduced-form / structural / theory+empirics / descriptive). The methods reviewer applies type-specific dimensions + sanity checks — don't demand parallel trends from a structural paper or an exclusion restriction from a descriptive one.

[LEARN:review] Every major concern must state "what would change my mind" (the specific test/evidence that resolves it) — sharper and more actionable than a bare "suggestion".

[LEARN:review] Classify major concerns FATAL / ADDRESSABLE / TASTE and roll up into MUST / SHOULD / MAY-push-back. Keep our judgment-based recommendation — do NOT adopt mechanical weighted 0–100 scoring.

[LEARN:review] Default table-format convention is **no significance stars** (SEs in parentheses; exact p-values/CIs for key results). The writing reviewer flags stars unless a journal profile explicitly permits them.

[LEARN:review] Journal-calibration / paper-type / "what-would-change-my-mind" concepts were adapted from clo-author (Hugo Sant'Anna). It has no LICENSE → all-rights-reserved, so adapt ideas and re-author text; credit the source. See [[meta-governance]].

[LEARN:review] A submission is a **document set**, not one file: main manuscript + separate online appendix/supplement. `/referee` gathers companions explicitly (extra paths / `--appendix`), by folder (`papers/mypaper/`), or by auto-detect (stem or appendix/online/supplement keyword, confirm first). Read the appendix in full — robustness, first-stage diagnostics, pre-trend plots, and proofs live there — and check absence claims against every document. Reviewers cite which document each finding comes from.

## Workflow Patterns

[LEARN:workflow] Requirements specification (AskUserQuestion, 3-5 questions) before planning catches ambiguity early → reduces rework 30-50%. Use for complex/ambiguous tasks (>1 hour or >3 files).

[LEARN:workflow] Plans, specs, and session logs must live on disk (`quality_reports/`) — not just in conversation — to survive compression and session boundaries.

[LEARN:workflow] Context survival before compression: (1) update MEMORY.md with [LEARN] entries, (2) session log current, (3) active plan saved to disk, (4) open questions documented. The pre-compact hook displays the checklist.

[LEARN:workflow] Plan-first applies to *meta-work* on this repo (changing the pipeline, adding reviewer lenses). Refereeing a single paper is a well-scoped job — run the pipeline directly.

## Documentation Standards

[LEARN:documentation] Keep README and CLAUDE.md in sync with the actual skills/agents/rules present. Stale docs that list removed tools break user trust.

[LEARN:documentation] Date fields in README/frontmatter must reflect the latest significant change.

## Design Philosophy

[LEARN:design] Framework-oriented > prescriptive. This repo is both a working referee setup and a forkable template: describe what a reviewer lens is *for*, then give a domain example — don't hard-code one field as the only option.

## File Organization

[LEARN:files] Input PDFs → `papers/` (gitignored by default; confidential). Reports → `referee_reports/`. Scratch (paper briefs) → `referee_reports/.work/` (gitignored). Plans/specs/session logs → `quality_reports/`. Templates → `templates/`.

## Memory System

[LEARN:memory] Two-tier memory: MEMORY.md (generic, committed, <200 lines) vs `.claude/state/personal-memory.md` (machine/user-specific journals + domain tweaks, gitignored). Solves the template-vs-working-project tension.

## Meta-Governance

[LEARN:meta] Repository dual nature (working setup + public template) requires explicit generic-vs-specific governance → prevents template pollution. When unsure: "Would someone refereeing in another field benefit?" Yes → commit. No → keep local.

[LEARN:meta] Dogfood our own patterns: fact-check before shipping, gate before saving, plan-first for meta-work, keep [LEARN] entries and session logs current.
