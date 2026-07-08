# Journal Profiles

Optional calibration for `/referee`. When a run is invoked with `--journal [X]`, the
reviewers read this file, find the matching profile, and shift their priorities and bar
to match that journal's review culture. **With no `--journal` flag, reviewers behave as
generic top-5 referees** — this file is not consulted.

Resolution:
1. `--journal [X]` and a profile exists below → calibrate to the full profile.
2. `--journal [X]` but no matching profile → use the journal name + generic top-5
   conventions, and note "profile not on file" in the report header.
3. No `--journal` → generic top-5 behavior.

> The *concepts* here (per-journal calibration, table-format conventions) are adapted from
> Hugo Sant'Anna's [clo-author](https://github.com/hugosantanna/clo-author). All profile
> text was re-authored for this repo. Descriptions are the referee's working priors, not
> official journal policy — verify specifics against each journal's own guidelines.

## Table-format convention

**Default (all journals): no significance stars.** This reflects the modern top-journal
norm, not any single publisher's house rule — report standard errors in parentheses and
give exact p-values or confidence intervals for key results. The writing-reviewer flags
significance stars as a formatting problem unless a specific profile below explicitly
permits them.

---

## Top-5 general interest

### American Economic Review (AER)
- **Focus:** all fields; the broadest audience in economics.
- **Bar:** must interest economists outside the subfield — a big question, clean execution, a clearly stated contribution.
- **Domain lens:** would a non-specialist care? Position against the *general* frontier, not just the subfield. Insight suffices; policy is welcome, not required.
- **Methods lens:** identification convincing to non-specialists; clean and transparent beats technically elaborate; robustness thorough but not padded.
- **Typical concerns:** "Why should economists outside this field care?" "Is the contribution big enough?" "Is this too narrow?"

### Econometrica (ECMA)
- **Focus:** theory and empirical work with formal rigor.
- **Bar:** methodological innovation, or empirics with near-airtight identification and formal results.
- **Domain lens:** theory valued highly; formal results expected, with welfare/normative analysis *where the question calls for it*; less policy narrative, more mechanism.
- **Methods lens:** formal/near-formal arguments for key results; asymptotics discussed; novel estimators need theoretical justification and finite-sample (Monte Carlo) evidence.
- **Typical concerns:** "Where is the formal result?" "What are the asymptotic properties?" "Methods or applied contribution?"

### Journal of Political Economy (JPE)
- **Focus:** all fields; strong emphasis on economic mechanisms and structural thinking.
- **Bar:** deep economic insight — *why*, not just *that*.
- **Domain lens:** mechanism and economic insight are prized; a conceptual framework (even informal) strengthens the case; strong reduced-form work is welcome when the economics is deep.
- **Methods lens:** strong identification plus mechanism evidence; heterogeneity that illuminates the mechanism; some identification imperfection tolerated if the economics is deep.
- **Typical concerns:** "What is the mechanism?" "Can you decompose the effect?" "What does this teach us about behavior?"

### Quarterly Journal of Economics (QJE)
- **Focus:** all fields; prizes an important question and a surprising answer.
- **Bar:** the question must matter and the result should change how we think about something.
- **Domain lens:** narrative matters enormously; broad implications; clever data/settings rewarded.
- **Methods lens:** identification clean *and intuitive* — explainable in one sentence; visual evidence (event studies, RD plots) valued.
- **Typical concerns:** "Is this surprising?" "Does it change how we think about X?" "Can you explain identification in one sentence?"

### Review of Economic Studies (REStud)
- **Focus:** all fields; technically excellent empirical and theoretical work.
- **Bar:** top-tier technical quality; completeness over storytelling.
- **Domain lens:** address every plausible objection; careful, complete literature treatment.
- **Methods lens:** every specification justified; thorough robustness; sensitivity analysis (e.g., Oster 2019; Rambachan & Roth 2023); careful inference and multiple-testing corrections.
- **Typical concerns:** "Have you checked robustness to X?" "What about specification Y?" "Inference needs more care."

---

## Top field journals

### AEJ: Applied Economics
- **Focus:** empirical microeconomics across fields — labor, health, education, development, public, environmental, urban, and related.
- **Bar:** clean applied-micro paper, credible identification, clear results; slightly below top-5 in ambition, same rigor.
- **Methods lens:** top-5 identification standards; modern estimators for staggered DiD (no naïve TWFE); replication package expected.
- **Typical concerns:** "Is this incremental vs. [closely related paper]?" "Better suited to a field journal?"

### AEJ: Economic Policy
- **Focus:** economics with a policy dimension, across fields (public, labor, health, education, environmental, urban, development, IO) — broader than formal program evaluation.
- **Bar:** a policy-relevant question with credible identification; the policy angle may be direct evaluation *or* economic analysis that informs policy.
- **Domain lens:** policy relevance is central, but welfare/cost-benefit is valued *where the question calls for it* — not a publication requirement; institutional detail and generalizability strengthen the paper.
- **Methods lens:** credible identification (clean pre-trends where DiD is used); policy-relevant heterogeneity; back-of-envelope welfare where it fits.
- **Typical concerns:** "What does this imply for policy?" "Does it generalize?" "Is the identification credible?"

### Journal of Human Resources (JHR)
- **Focus:** applied microeconomics of human capital and well-being — labor, education, health, and demography — with policy relevance.
- **Bar:** strong empirical contribution, clear policy relevance, careful identification.
- **Domain lens:** external validity and representativeness; policy-relevant heterogeneity (by race/gender/income); institutional knowledge.
- **Methods lens:** careful identification is central; modern staggered-DiD estimators; clean pre-trends shown; replication package at acceptance.
- **Typical concerns:** "What is the policy implication?" "Does this generalize beyond your sample?" "Heterogeneity by subgroup?"

### Journal of Public Economics (JPubE)
- **Focus:** the role of government in the economy — taxation, public goods, redistribution, social insurance, government programs, and the public-economics side of environmental, political-economy, and behavioral questions.
- **Bar:** a public-economics question, clean identification, command of the relevant institutional mechanics.
- **Methods lens:** credible identification for the question; typical designs include bunching at kinks/notches, RDD at eligibility thresholds, and DiD around reforms — no single design is required; attend to extensive vs. intensive margins.
- **Typical concerns:** "What is the elasticity?" "Extensive or intensive margin?" "Welfare implications of the reform?"

### Journal of Labor Economics (JLE)
- **Focus:** labor economics broadly, including labor-adjacent topics — labor supply/demand, human capital and education, wage determination, discrimination, migration, search/matching, and personnel/family economics.
- **Bar:** a genuine labor-economics contribution with careful identification and institutional understanding; topic breadth is not a barrier.
- **Methods lens:** identification appropriate to the question; selection addressed where relevant (e.g., Heckman, Lee bounds); decompositions and event studies where they fit — no single required method.
- **Typical concerns:** "Supply or demand effect?" "Selection into employment?" "General-equilibrium effects?"

### Journal of Development Economics (JDE)
- **Focus:** development economics broadly — poverty, health, education, finance, firms and agriculture, labor, institutions, political economy, conflict, and trade — in low- and middle-income settings.
- **Bar:** credible evidence (RCT or strong quasi-experiment) plus field knowledge of the setting.
- **Domain lens:** context matters enormously; external validity to other settings; cost-effectiveness; sustainability; equity dimensions.
- **Methods lens:** RCTs — randomization/attrition/compliance/spillovers, pre-analysis plan, power; quasi-experiments — strong first stage, clean RD/parallel trends; clustering at the right level.
- **Typical concerns:** "Does this generalize beyond this context?" "Attrition?" "Cost-effectiveness?" "Long-run effects?"

---

## Econometrics & measurement

### Journal of Econometrics
- **Focus:** econometric theory and methods — identification, estimation, inference, testing, ML for causal inference.
- **Bar:** a methodological contribution with formal results; the *method* is the contribution, not the application.
- **Domain lens:** state precisely what existing methods cannot do that yours can; empirical illustration showcases the method.
- **Methods lens:** formal proofs for key results (consistency, asymptotic normality, rates); regularity conditions stated and discussed; Monte Carlo finite-sample evidence; comparison to existing estimators; computational feasibility; code availability.
- **Typical concerns:** "Asymptotic properties?" "How does it compare to [existing method]?" "Finite-sample behavior?" "Are the regularity conditions plausible?"

### Review of Economics and Statistics (RESTAT)
- **Focus:** broad empirical economics — applied micro and macro — with high econometric standards.
- **Bar:** technically excellent empirics; highest econometric standards short of Econometrica.
- **Domain lens:** credible identification and careful inference; novel data valued; replication studies welcome.
- **Methods lens:** every assumption tested or bounded; sensitivity analysis; careful standard errors; pre-registration viewed favorably.
- **Typical concerns:** "Is the identification airtight?" "Have you tested (or bounded) every key assumption?" "Is the inference careful enough?"

---

## Short format

### AER: Insights
- **Focus:** AER breadth in a short, punchy format.
- **Bar:** one clean, compelling result; brevity is a feature.
- **Methods lens:** core identification must be clean; fewer robustness checks acceptable, but the main result must be robust; visual evidence valued.
- **Typical concerns:** "Can this be told in ~10 pages?" "Is the single result compelling enough?"

### Economics Letters
- **Focus:** short papers across all fields.
- **Bar:** one clear result in 5–8 pages; the result must stand alone.
- **Methods lens:** identification clean; one well-executed specification enough; standard errors correct; no room for a 10-table robustness section.
- **Typical concerns:** "Can this be said in 5 pages?" "Is the single result robust?" "Novel enough to stand alone?"

---

## Add your own journal

Copy this block above and fill it in:

```markdown
### [Journal Name] ([Abbrev])
- **Focus:** [fields/topics]
- **Bar:** [what it takes to publish here]
- **Domain lens:** [what matters most to substance reviewers here]
- **Methods lens:** [rigor expectations, preferred methods, required checks]
- **Typical concerns:** [common referee questions at this journal]
- **Table format:** [only if it deviates from the default]
```
