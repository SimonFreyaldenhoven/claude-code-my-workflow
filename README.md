# Referee — AI-Assisted Peer Review

> Drop a paper PDF in, get a top-journal-quality referee report out.

**Last Updated:** 2026-07-07

A focused Claude Code setup for refereeing economics manuscripts. You supply a paper; Claude reads it end-to-end, runs a panel of specialist reviewers in parallel, adds an adversarial "toughest referee" pass, synthesizes everything into one structured report, and fact-checks every claim against the actual text before handing it back. Like a conscientious referee who does the whole job — and cites their sources.

---

## Quick Start

### 1. Clone

```bash
git clone https://github.com/YOUR_USERNAME/claude-code-my-workflow.git referee
cd referee
```

### 2. Add a paper and run

```bash
claude
```

Then drop your manuscript into `papers/` and ask:

> `/referee papers/manuscript.pdf`

Claude runs the full pipeline and writes the report to `referee_reports/`. With no argument, `/referee` lists the PDFs in `papers/` and asks which one.

To review as if for a specific journal, add `--journal`:

> `/referee papers/manuscript.pdf --journal QJE`

Without the flag, reviewers behave as generic top-5 referees.

**Separate appendix?** Pass the whole set — `/referee paper.pdf appendix.pdf`, or drop the files in `papers/mypaper/` and run `/referee papers/mypaper/`, or just name the appendix with the paper's stem (`paper_online_appendix.pdf`) and let `/referee paper.pdf` auto-detect it. The appendix is read in full — that's where robustness and proofs usually live — and reviewers cite which document each finding comes from.

---

## How It Works

```
PDF in papers/
   │
   1. INGEST       read the whole paper (+ appendix); classify paper type; write a "paper brief"
   2. REVIEW       4 specialist reviewers run in parallel (calibrated to --journal if given)
   3. ADVERSARIAL  a toughest-referee pass hunts for reject-worthy objections (FATAL/ADDRESSABLE/TASTE)
   4. SYNTHESIZE   dedupe, reconcile, rank, rate, MUST/SHOULD/MAY, recommend → one report
   5. FACT-CHECK   verify every location reference and claim against the paper
   6. DELIVER      save to referee_reports/ and print the summary + recommendation
```

### The reviewer panel

Five focused agents, each better at its narrow job than a generalist:

| Agent | Lens |
|-------|------|
| **methods-reviewer** | Identification strategy + econometric specification (assumptions, SEs, inference, robustness) |
| **contribution-reviewer** | Research question, novelty, whether the conclusions actually follow |
| **literature-reviewer** | Citation completeness and honest positioning (may use the web; web claims are flagged `[web]`) |
| **writing-reviewer** | Clarity, notation, structure, self-contained tables/figures |
| **adversarial-referee** | The reasons-to-reject pass — fatal objections and what the specialists missed |

### Grounded, not generated

The single most important rule: **every criticism cites an exact location** (section, table, equation, page), and nothing is invented. Before a report is saved it passes an anti-fabrication fact-check — a wrong objection destroys a referee's credibility faster than a missed one. See `.claude/rules/review-verification.md`.

### The report

Structured-critique format (`templates/referee-report.md`):

- **Summary Assessment** with an overall recommendation (Accept / Minor / Major Revision / Reject & Resubmit / Reject)
- **Action Summary** — MUST address / SHOULD address / MAY push back
- **Strengths**
- **Major Concerns** (most severe first; each tagged FATAL / ADDRESSABLE / TASTE, with **"what would change my mind"** — the specific evidence that resolves it)
- **Minor Concerns**
- **Referee Objections** — the hardest questions the authors must answer
- **Ratings** — 1–5 across five dimensions, calibrated to the target journal's bar

### Calibration & paper type

Reviews are **paper-type aware** — a structural paper is judged on parameter identification and model fit, not parallel trends; a descriptive paper on construct validity, not exclusion restrictions. When you pass `--journal [X]`, the panel calibrates to that journal's bar and conventions (including AEA no-stars table formatting) via `.claude/references/journal-profiles.md`.

---

## What's Included

```
papers/                  # INPUT — drop manuscripts here (PDFs gitignored by default)
referee_reports/         # OUTPUT — generated reports
.claude/
  skills/referee/        # the pipeline orchestrator (/referee)
  skills/{lit-review,commit,context-status,learn}/
  agents/                # the 5 reviewer agents
  rules/                 # referee-report-protocol, review-verification (anti-fabrication),
                         # pdf-ingestion, orchestrator-protocol, quality-gates, plan-first,
                         # session-logging, meta-governance
  hooks/                 # context monitoring, logging reminders, compaction survival
templates/               # referee-report + requirements-spec / session-log / quality-report
quality_reports/         # plans + session logs (for meta-work on the repo itself)
```

---

## Prerequisites

| Tool | Required For | Install |
|------|-------------|---------|
| [Claude Code](https://code.claude.com/docs/en/overview) | Everything | `npm install -g @anthropic-ai/claude-code` |
| `pdfinfo` / `pdftk` (poppler / pdftk) | Inspecting long PDFs | `apt install poppler-utils pdftk` / `brew install poppler pdftk-java` |
| [gh CLI](https://cli.github.com/) | Optional — PR workflow | `brew install gh` |

Claude Code reads PDFs natively; the PDF tools just help with very long manuscripts.

---

## Adapting to Another Field

The defaults target economics/econometrics. To retarget:

1. Edit the reviewer lenses in `.claude/agents/*-reviewer.md` (e.g. swap "identification strategy" for your field's core validity questions).
2. Adjust the dimensions and rubric in `.claude/rules/referee-report-protocol.md`.
3. Update the domain line and skill table in `CLAUDE.md`.

Keep changes framework-oriented — describe what each reviewer lens is *for*, then give a domain example.

---

## Origin

Specialized from the [academic Claude Code workflow template](https://github.com/pedrohcgs/claude-code-my-workflow) (Pedro Sant'Anna) by stripping it down to a single job: high-quality peer review. The contractor-mode backbone — plan-first for meta-work, quality gates, multi-agent orchestration, context survival — is inherited from that template.

The journal-calibration, paper-type-aware review, and "what would change my mind" ideas were adapted from Hugo Sant'Anna's [clo-author](https://github.com/hugosantanna/clo-author) (concepts only; all text re-authored here).

## License

MIT License. See [LICENSE](LICENSE).
