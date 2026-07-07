---
name: referee
description: Produce a top-journal-quality economics referee report from a paper PDF. Orchestrates paper-type-aware parallel specialist reviewers (methods, contribution, literature, writing), an adversarial toughest-referee pass, then synthesizes one structured report and fact-checks every claim against the paper. Optionally calibrates to a target journal. Use when the user supplies a paper to review or says "referee this", "review this paper", "write a referee report".
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
4. **Classify the paper type:** reduced-form / structural / theory+empirics / descriptive-measurement. This determines which review dimensions the methods reviewer applies.
5. Write a faithful **paper brief** to `referee_reports/.work/<name>_brief.md` capturing: the **document set** (main + companions, with page counts); title & authors; abstract; research question; **paper type**; data & sample; research design/estimator; identifying assumptions as stated; headline results (with numbers); main tables/figures and **which document each is in**; the reference list. When citing a location, **name the document** (e.g. "Online Appendix, Table A5"). The brief is orientation for the sub-agents — **not** a substitute for the sources; every reviewer verifies claims against the PDFs themselves.

## Phase 2 — Specialist reviews (parallel)

Launch **all four** specialists in a single message (concurrent Task calls). Pass each: the **document paths** (main paper + any companion appendix/supplement), the **brief path**, the **paper type**, and the **journal calibration** (profile text, or "generic top-5"):

- `methods-reviewer` — identification/estimation on the type-specific dimensions + mandatory sanity checks
- `contribution-reviewer` — question, novelty, whether conclusions follow (judged against the journal's bar)
- `literature-reviewer` — citation completeness & positioning against the journal's frontier (may use the web; flags web claims)
- `writing-reviewer` — clarity, notation, self-contained exhibits, journal table-format convention

Each returns the structured block defined in its agent file, with **"what would change my mind"** on every major concern.

## Phase 3 — Adversarial pass

Launch `adversarial-referee` with the document set, the brief, the paper type + journal calibration, and the four specialist outputs. It hunts for reject-worthy objections and anything the specialists missed, labeling each `FATAL` / `ADDRESSABLE` / `TASTE` against the calibrated bar.

## Phase 4 — Synthesize (you act as editor)

Merge everything into one report — do not just concatenate:
- **Deduplicate & reconcile.** Fold overlapping points together. Where reviewers disagree, adjudicate and explain your call.
- **Classify every major concern** `FATAL` / `ADDRESSABLE` / `TASTE`, judged against the calibrated bar.
- **Rank** by how much the conclusions depend on the point (FATAL first), not by which reviewer raised it.
- **Produce a MUST / SHOULD / MAY summary:** MUST = FATAL + serious ADDRESSABLE; SHOULD = other ADDRESSABLE; MAY push back = TASTE.
- **Recommend.** Choose an overall recommendation and set the 1–5 dimension ratings — the referee's judgment informed by (not a mechanical average of) the specialists.
- Write the report in the **structured-critique format** from `referee-report-protocol.md`, header stating *Calibrated to* and *Paper type*.

## Phase 5 — Fact-check (anti-fabrication)

Before saving, run the checklist in `review-verification.md` over your draft: every location reference real, every "they didn't do X" actually confirmed **against the main paper AND every appendix/supplement**, no invented coefficients or citations, web claims flagged `[web]`. Drop or soften anything you cannot stand behind. Verify the load-bearing claims (those driving a MUST item or the recommendation) against the source documents.

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
