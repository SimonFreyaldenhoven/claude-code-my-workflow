# Review Verification (Anti-Fabrication)

A referee report is only as good as its grounding. One invented objection or misquoted result destroys the report's credibility — and wastes the authors' time. Before any report is saved, it passes this checklist.

## The core rule

**Every specific claim in the report must be traceable to the actual paper (or to a flagged external source).** If you cannot point to where in the PDF something is (or isn't), you cannot assert it.

## Fact-check checklist (run over the drafted report before saving)

1. **Location references are real.** Every "Section 4.2", "Table 3", "eq. (7)", "p. 12" points to something that exists and says what you claim. Spot-read the PDF to confirm, don't trust the brief.
2. **"They didn't do X" is verified absence, not unchecked assumption.** Before writing "no clustered SEs", "no pre-trend test", "doesn't cite Y" — confirm it's actually missing. Search the **main paper AND every companion PDF** (online appendix, supplement, proofs). A separate appendix is the single most common place a "missing" robustness check, first-stage diagnostic, or proof actually lives — check it before asserting absence. Absence claims are the most dangerous; hold them to the highest bar.
3. **No invented numbers.** Coefficients, sample sizes, p-values, and magnitudes quoted in the report must match the paper exactly.
4. **No invented citations.** A referenced prior paper must be one you are confident exists. Web-sourced references are marked `[web]` with authors/year/venue so the editor can verify. If unsure, write "possible — verify", never a confident cite.
5. **Quotes are verbatim.** Anything in quotation marks appears in the paper as written.
6. **Severity matches evidence.** A concern is only "major/fatal" if the grounding supports it. Downgrade objections whose evidence is thin to "possible concern — worth checking".

## What to do with unverifiable claims

- **Drop it** if it's not central and you can't ground it.
- **Soften it** to an explicit uncertainty ("If the SEs are not clustered — this was not clear from the text — then…") when the point matters but the evidence is ambiguous.
- **Flag the ambiguity** so the editor/authors know it's a question, not a finding.

## Applies to sub-agents too

The specialist and adversarial agents are instructed to cite exact locations and not fabricate. The orchestrator does **not** take their claims on trust: during synthesis, verify the load-bearing ones (anything that drives a major concern or the recommendation) against the PDF before promoting them into the report.
