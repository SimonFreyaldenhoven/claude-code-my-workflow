# papers/ — input

Drop the manuscript you want refereed here (a `.pdf`, or a `.tex` / `.qmd` source), then run:

```
/referee <filename>
```

`/referee` resolves the argument against a direct path first, then `papers/<filename>`, then a glob match. With no argument it lists the PDFs here and asks which one.

### Papers with a separate appendix

Many submissions arrive as a main manuscript **plus** a separate online appendix or supplement. Give `/referee` all of them — three ways, pick whichever suits you:

- **List them:** `/referee paper.pdf paper_appendix.pdf` (first = main, rest = companions), or `/referee paper.pdf --appendix appendix.pdf`.
- **Folder per paper:** put the set in `papers/mypaper/` and run `/referee papers/mypaper/`.
- **Auto-detect:** name the appendix with the paper's stem or an `appendix`/`online`/`supplement` keyword (e.g. `paper.pdf` + `paper_online_appendix.pdf`) and just run `/referee paper.pdf` — it will find the companion and confirm before including it.

The appendix is read in full — robustness, first-stage diagnostics, pre-trend plots, and proofs usually live there, and the reviewers cite which document each item comes from.

The generated report is written to `referee_reports/`.

> PDFs placed here are **not** committed by default (see `.gitignore`) — manuscripts under review are usually confidential. Remove that ignore rule if you want to track example papers in the repo.
