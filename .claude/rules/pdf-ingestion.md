# PDF Ingestion

How to read a manuscript PDF thoroughly and cheaply so the review is grounded in the real text.

## Read the whole thing

A referee reads the entire paper — introduction, model, data, results, robustness, **and the appendix** (where the identifying-assumption tests and honest caveats often hide). Do not review from the abstract and intro alone.

## Separate appendix / companion PDFs

A submission is frequently a **main manuscript plus one or more separate PDFs** — an online appendix, a supplement, proofs. Treat these as one document set:

- **Gather them all.** Take companions from explicit arguments (`--appendix`, or extra file paths), from a per-paper folder (`papers/mypaper/`), or by auto-detecting files that share the main file's stem or contain `appendix` / `online` / `supplement` in the name (confirm before including).
- **Read every one.** The online appendix is where pre-trend plots, robustness batteries, first-stage diagnostics, and proofs usually live — precisely the material a referee must check. Skipping it produces false "they didn't do X" objections.
- **Cite the document by name.** Location references must disambiguate which document — e.g. "main text, Table 3" vs. "Online Appendix, Table A5", "Supplement, Sec. B.2". A bare "Table A5" is ambiguous when there are two PDFs.
- **Absence is checked across the set.** Before asserting something is missing, search the main paper *and* every companion (see `review-verification.md`).

## Handling length

1. **Size it up first.** Use `pdfinfo <file>` for the page count, or read the first page / table of contents for the section structure. Decide chunking from there.
2. **Chunk long PDFs.** Read in ~5-page ranges with the Read tool's `pages` parameter. Long papers (40+ pages with appendix) should be read in sequence, not sampled.
3. **Note locations as you go.** Record section/table/figure/equation numbers with page numbers while reading — these become the exact-location citations the report requires.
4. **Tables and figures matter.** Read table notes and figure captions carefully: the SE definition, sample, units, and significance conventions live there and are frequently where problems surface.

## The paper brief

After reading, write a faithful structured brief to `referee_reports/.work/<name>_brief.md`:
- Title, authors, venue/date if shown
- Abstract (as written)
- Research question and claimed contribution
- **Paper type** — classify as reduced-form / structural / theory+empirics / descriptive-measurement. This selects which dimensions and sanity checks the methods reviewer applies (don't demand parallel trends from a structural paper, or an exclusion restriction from a descriptive one).
- Data, sample, and time period
- Research design / estimator; identifying assumptions **as the authors state them**
- Headline results **with the actual numbers**
- Each main table/figure: number + one line on what it shows
- The reference list

If the run was invoked with `--journal [X]`, also record the resolved calibration (matched profile / "profile not on file" / "generic top-5") so every reviewer shares it.

The brief orients the sub-agents. It is **not** authoritative — the PDF is. Every reviewer verifies claims against the source before putting them in the report (see `review-verification.md`).

## Non-PDF inputs

`.tex` / `.qmd` sources can be read directly (and grepped for `\cite`, `\label`, equation environments). The same thoroughness applies. If only a PDF is available and text extraction is poor (scanned image), say so — do not guess at unreadable content.

## Permitted tools

`pdfinfo`, `gs`, and `pdftk` are pre-approved for inspecting/splitting PDFs. The Read tool renders PDF pages directly and is the primary path.
