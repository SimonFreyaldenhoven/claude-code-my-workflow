---
name: referee
description: Produce a top-journal-quality economics referee report from a paper PDF. Orchestrates paper-type-aware parallel specialist reviewers (methods, contribution, literature, writing, consistency, plus a proof-checker when the paper has formal results), an adversarial toughest-referee pass, then synthesizes one structured report and fact-checks every claim against the paper. Optionally calibrates to a target journal. Use when the user supplies a paper to review or says "referee this", "review this paper", "write a referee report".
argument-hint: "[path to a PDF, or a filename in papers/] [--journal X]"
allowed-tools: ["Read", "Grep", "Glob", "Write", "Bash", "Task", "WebSearch", "WebFetch"]
---

# Referee — Multi-Agent Peer Review

Turn a paper PDF into the kind of report a conscientious referee at a top economics journal would write: specific, constructive, grounded in the actual text, and honest about what is fatal vs. fixable.

**Input:** `$ARGUMENTS` — a path to a `.pdf` (or `.tex`/`.qmd`), or a filename in `papers/`, plus optional flags:
- `--journal [X]` — calibrate the whole review to a target journal (e.g. `--journal QJE`). **Omitted → generic top-5 behavior.**

If no paper is given, list the PDFs in `papers/` and ask which one.

Follow the rules in `.claude/rules/`: `pdf-ingestion.md`, `referee-report-protocol.md`, `review-verification.md`, and `orchestrator-protocol.md`.

---

## Phase 1 — Locate, ingest & scope

1. **Resolve the document set.** A submission is often a main manuscript *plus* one or more companion PDFs (online appendix, supplement). Gather all of them:
   - **Explicit:** every non-flag path/filename in `$ARGUMENTS` is a document — the first is the main paper, the rest are companions. `--appendix [path]` adds one too.
   - **Folder:** if the argument is a directory (e.g. `papers/mypaper/`), include every PDF inside it.
   - **Auto-detect:** given a single main file `foo.pdf`, scan its directory for companions that share the stem or whose names contain `appendix` / `online` / `supplement` / `supplementary` (e.g. `foo_appendix.pdf`, `foo-online-appendix.pdf`). **List what you find and confirm** before including.
   Resolve each via direct path → `papers/…` → glob; if ambiguous, ask. Designate exactly one **main** document; the rest are companions.
2. **Resolve journal calibration.** If `--journal [X]` was given, read `.claude/references/journal-profiles.md` and find the profile. Record one of: the matched profile, "profile not on file → [name] + generic top-5", or (no flag) "generic top-5".
3. Get length first (`pdfinfo`) for **each** document, then **read every document** end-to-end in ~5-page chunks for long PDFs. The appendix is not optional — identification tests, robustness, and proofs frequently live there. Do not skim.
4. **Classify the paper type & detect formal results.** Classify as reduced-form / structural / theory+empirics / descriptive-measurement / **theory** (pure theory, no empirics). This selects the methods reviewer's dimensions and which rating rows apply (a pure-theory paper's Identification / Econometric Specification rows are **N/A**). Separately, note whether the document set contains **formal results** — theorems, propositions, lemmas, corollaries, or non-trivial formal derivations — and inventory them. This flag gates the conditional **proof-reviewer** in Phase 2, *independent of paper type* (a reduced-form paper with a formal proposition still triggers it; a purely narrative theory paper with no stated results does not).
5. Write a faithful **paper brief** to `referee_reports/.work/<name>_brief.md` capturing: the **document set** (main + companions, with page counts); title & authors; abstract; research question; **paper type**; the **formal-results flag (yes/no) + inventory** (each theorem/proposition/lemma/corollary: statement + where proved); data & sample; research design/estimator; identifying assumptions as stated; headline results (with numbers); main tables/figures and **which document each is in**; the reference list. When citing a location, **name the document** (e.g. "Online Appendix, Table A5"). The brief is orientation for the sub-agents — **not** a substitute for the sources; every reviewer verifies claims against the PDFs themselves.

## Phase 2 — Specialist reviews (parallel)

Launch the **five core specialists** in a single message (concurrent Task calls), **plus `proof-reviewer` when the brief's formal-results flag is `yes`** (so up to six run concurrently). Pass each: the **document paths** (main paper + any companion appendix/supplement), the **brief path**, the **paper type**, and the **journal calibration** (profile text, or "generic top-5"):

- `methods-reviewer` — identification/estimation on the type-specific dimensions + mandatory sanity checks
- `contribution-reviewer` — question, novelty, whether conclusions follow (judged against the journal's bar)
- `literature-reviewer` — citation completeness & positioning against the journal's frontier (may use the web; flags web claims)
- `writing-reviewer` — clarity, notation, self-contained exhibits, journal table-format convention
- `consistency-reviewer` — internal consistency: the same quantity **and any repeated stated fact (sample period, N, definitions)** agree across tables/figures/text/appendix, the prose never contradicts itself, and stated formulas/aggregates/derived statistics reconcile (light re-derivation, read-only)
- `proof-reviewer` **— only when the paper contains formal results** — correctness and completeness of every theorem/proposition/lemma/corollary and its proof (logical validity, stated-vs-hidden assumptions, correct use of invoked external results, claim↔proof match, counterexamples/edge cases), and whether the stated result actually supports the claims built on it; reads the appendix in full, where proofs live

Each returns the structured block defined in its agent file, with **"what would change my mind"** on every major concern.

## Phase 3 — Adversarial pass

Launch `adversarial-referee` with the document set, the brief, the paper type + journal calibration, and the specialist outputs (including the **proof review** when it ran). It hunts for reject-worthy objections and anything the specialists missed, labeling each `FATAL` / `ADDRESSABLE` / `TASTE` against the calibrated bar.

## Phase 4 — Synthesize (you act as editor)

Merge everything into one report — do not just concatenate:
- **Deduplicate & reconcile.** Fold overlapping points together. Where reviewers disagree, adjudicate and explain your call.
- **Classify every major concern** `FATAL` / `ADDRESSABLE` / `TASTE`, judged against the calibrated bar.
- **Rank** by how much the conclusions depend on the point (FATAL first), not by which reviewer raised it.
- **Compile the Consistency Check.** Carry the consistency-reviewer's audit into the report's `## Consistency Check` section (verified reconciliations + each mismatch with **both** locations and values); fold any confirmed mismatch into the ranked concerns, tagged to the existing dimension it threatens (usually Econometric Specification) — it earns **no** separate rating.
- **Compile the Formal Results Check** *(when the proof-reviewer ran)*. Carry its audit into the report's `## Formal Results Check` section (the formal-results inventory + per-result verdict; verified results, and each GAP/ERROR with its exact step and location); fold any confirmed ERROR or serious GAP into the ranked concerns, tagged to the **Theoretical Rigor / Proofs** dimension. If the proof-reviewer did not run (no formal results), the section reads "none; no formal results — not applicable" and the Theoretical Rigor rating is **N/A**.
- **Produce a MUST / SHOULD / MAY summary:** MUST = FATAL + serious ADDRESSABLE; SHOULD = other ADDRESSABLE; MAY push back = TASTE.
- **Recommend.** Choose an overall recommendation and set the 1–5 dimension ratings — the referee's judgment informed by (not a mechanical average of) the specialists. Set **Theoretical Rigor / Proofs** from the proof-reviewer (or **N/A** if it did not run). For a **pure-theory** paper, mark the empirical rows (Identification, Econometric Specification) **N/A** and lean the overall on Contribution + Theoretical Rigor. A demonstrated proof ERROR in a load-bearing result is FATAL and caps the overall.
- Write the report in the **structured-critique format** from `referee-report-protocol.md`, header stating *Calibrated to* and *Paper type*.

## Phase 5 — Fact-check (anti-fabrication)

Before saving, run the checklist in `review-verification.md` over your draft: every location reference real, every "they didn't do X" actually confirmed **against the main paper AND every appendix/supplement**, no invented coefficients or citations, web claims flagged `[web]`. Drop or soften anything you cannot stand behind. Verify the load-bearing claims (those driving a MUST item or the recommendation) against the source documents. For each **Consistency Check** ISSUE, re-read **both** cited locations and confirm both numbers — a claimed mismatch is a two-sided factual assertion and is dropped if either side can't be verified. For each **Formal Results Check** finding, re-read the cited step: keep **ERROR** only if the flaw is demonstrable (a counterexample or a step that plainly does not follow), otherwise downgrade to a **GAP** ("the authors should justify step X"); and confirm any "the paper does not prove X" against the appendix and every companion before keeping it — proofs live in appendices.

## Phase 6 — Deliver

- Save to `referee_reports/referee_report_<sanitized-name>_<YYYY-MM-DD>.md` using `templates/referee-report.md`.
- Print the **Summary Assessment**, the recommendation, and the MUST-address items to the user, with a pointer to the saved file.
- Leave the brief in `referee_reports/.work/` (gitignored) for traceability.

---

## Principles

- **Grounded, not generated.** Every criticism cites an exact location; nothing is invented. A wrong objection destroys the report's credibility faster than a missed one.
- **What would change my mind.** Every major concern names the specific test/evidence that would resolve it — not just "this is wrong".
- **Calibrated.** Distinguish fatal flaws from things a good revision fixes, and taste from substance. Judge against the target journal's bar when one is given.
- **Paper-type aware.** Don't demand parallel trends from a structural paper or an exclusion restriction from a descriptive one.
- **Fair.** Acknowledge genuine strengths; good work deserves recognition.
- **The referee, not the editor's rubber stamp.** Recommend, justify, and raise the hard questions — the report should let an editor make the call.
