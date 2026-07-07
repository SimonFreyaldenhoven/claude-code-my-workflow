# CLAUDE.md — Peer-Review (Referee Report) Workflow

<!-- Defaults target economics/econometrics. To retarget the domain, edit the
     reviewer lenses in .claude/agents/*-reviewer.md and the dimensions in
     .claude/rules/referee-report-protocol.md. Keep this file short — it loads
     every session. -->

**Purpose:** Supply a paper PDF → get a top-journal-quality referee report.
**Domain:** Economics / econometrics
**Branch:** main

---

## Core Principles

- **Grounded, never generated** — every claim in a report cites an exact location in the paper; nothing is invented (`review-verification.md`)
- **Multi-agent + adversarial** — parallel specialist reviewers, then a toughest-referee pass, then synthesis
- **Constructive & calibrated** — every criticism has a suggestion; fatal flaws separated from fixable ones
- **Gate before shipping** — a report saves only when the quality checklist passes (`quality-gates.md`)
- **[LEARN] tags** — when corrected, save `[LEARN:category] wrong → right` to MEMORY.md

---

## The Pipeline (`/referee`)

```
PDF in papers/  →  1. INGEST      read end-to-end; classify paper type; write a paper brief
                   2. REVIEW      4 specialists in parallel (calibrated to --journal if given)
                   3. ADVERSARIAL toughest-referee pass (FATAL / ADDRESSABLE / TASTE)
                   4. SYNTHESIZE  one report: dedupe, rank, rate, MUST/SHOULD/MAY, recommend
                   5. FACT-CHECK  verify every claim against the paper
                   6. DELIVER     save to referee_reports/
```

Reviewer dimensions: **Contribution** · **Identification** · **Econometric Specification** · **Literature & Positioning** · **Writing & Presentation**. Each rated 1–5; overall recommendation is the referee's judgment, not a mechanical average.

- **Paper-type aware** — reviews adapt to reduced-form / structural / theory+empirics / descriptive (no parallel-trends demand of a structural paper).
- **Optional journal calibration** — `--journal [X]` tunes the review to a journal's bar via `.claude/references/journal-profiles.md`; omitted → generic top-5.
- **"What would change my mind"** — every major concern names the specific evidence that would resolve it.

---

## Folder Structure

```
├── CLAUDE.md                # This file
├── README.md                # Quick start
├── MEMORY.md                # Cross-session learnings ([LEARN] entries)
├── .claude/                 # skills, agents, rules, references, hooks, settings
├── papers/                  # INPUT — drop the manuscript here
├── referee_reports/         # OUTPUT — generated reports (.work/ = scratch)
├── quality_reports/         # Plans + session logs (meta-work on the repo)
└── templates/               # Referee report + spec/log/quality templates
```

---

## Commands

```bash
# Referee a paper (main command)
/referee papers/manuscript.pdf                       # generic top-5 referee
/referee papers/manuscript.pdf --journal QJE         # calibrated to a target journal
/referee papers/manuscript.pdf papers/appendix.pdf   # main + separate appendix/supplement
/referee papers/mypaper/                             # a folder = the whole document set

# Inspect a PDF before/while reviewing
pdfinfo papers/manuscript.pdf
```

---

## Skills Quick Reference

| Command | What It Does |
|---------|-------------|
| `/referee [pdf] [--journal X]` | **Main** — full multi-agent + adversarial referee report; optional journal calibration |
| `/lit-review [topic]` | Literature search + synthesis (positioning / missing-citation support) |
| `/commit [msg]` | Stage, commit, PR, merge |
| `/context-status` | Session health + context usage |
| `/learn [skill]` | Extract a discovery into a persistent skill |

## Agents (`.claude/agents/`)

| Agent | Lens |
|-------|------|
| `methods-reviewer` | Identification strategy + econometric specification |
| `contribution-reviewer` | Question, novelty, whether conclusions follow |
| `literature-reviewer` | Citation completeness & positioning (may use web, flagged) |
| `writing-reviewer` | Clarity, notation, self-contained exhibits |
| `adversarial-referee` | Reasons to reject; what the specialists missed |

---

## Quality Gate

A report ships only when it is **grounded, specific, actionable, calibrated, complete, fair, and coherent** — see the checklist in `.claude/rules/quality-gates.md`. There is no compile step; the fact-check pass (`review-verification.md`) is the equivalent of "verify after".

---

## Current State

| Item | Status |
|------|--------|
| Pipeline | `/referee` — ingest → specialists → adversarial → synthesize → fact-check |
| Domain | Economics / econometrics (retarget via agent lenses + report protocol) |
| Input | `papers/` (PDFs gitignored by default — confidential) |
| Output | `referee_reports/` |
