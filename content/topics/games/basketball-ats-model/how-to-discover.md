---
type: note
series: games
title: "How prediction research actually works: discovering tweaks, theories, and the state of the art"
status: curated
tags: [games, basketball, machine-learning, data-science, research-methods, verification]
created: 2026-07-19
---

# How prediction research actually works

The companion note, [building a basketball spread model](../basketball-ats-model),
walks through constructing one specific model. This note answers the question
underneath it: if you want to get into predictive data analysis and *discover* things —
find your own tweaks, test your own theories, know what state-of-the-art even means —
how do you actually work? Basketball stays as the running example, but everything here
is the general craft. Labels as always: FACT is attributed to a named source;
Assessment is my read; Speculation is flagged.

## The engine of discovery is embarrassingly simple

Assessment: the way practitioners find improvements is not inspiration, it is a loop
run hundreds of times: ask one small question, hold everything else fixed, measure
against a benchmark you locked *before* you started, keep or kill the change, write it
down, repeat. The tabular machine-learning literature of the last five years is,
underneath the papers, a story about this loop being enforced on a whole field.

FACT: the 2021 study that reset the field (Gorishniy et al.) opens with the complaint
that proposed models "are usually not properly compared to each other" across
inconsistent benchmarks, and its lasting contribution was less any architecture than
the discipline of comparing everything "under the same training and tuning protocols."
FACT: the famous 2022 "trees win" paper (Grinsztajn et al.) published every point of a
20,000-compute-hour hyperparameter search as a public benchmark, precisely so others
could test claims against it. FACT: the current generation continues this: the TALENT
benchmark ships a 45-dataset core explicitly "for rapid, reproducible evaluation,"
and the AutoGluon team maintains TabArena, "a living benchmark for machine learning
on tabular data." Assessment: this is the single most transferable lesson for your
own project: your five seasons of basketball games, your fixed metric, and your
baseline model *are* your private TabArena, and any tweak that cannot beat the
baseline on the locked benchmark did not happen.

![Diagram: the discovery loop in six steps arranged in a cycle. One: question, for example do rested teams beat the lag-4 model. Two: locked benchmark, fixed seasons and fixed metric chosen first. Three: one change, a feature, a lag, or a model, never two. Four: measure, calibration and margin error versus the baseline. Five: keep or kill, no maybe pile, in or documented out. Six: log it, MLflow or a notebook recording what, why, and the result. A side panel gives the evidence: feature work beats model shopping (model rankings change considerably after feature engineering per arXiv 2407.02112), fields advance on shared benchmarks, and ensembles help everyone in every benchmark in the pool.](img/discovery-loop.png)
*The loop, and why the evidence says to run it in this order. Diagram.*

## Where improvements actually come from

FACT: a 2024 data-centric study (Tschalzev et al.) rebuilt expert-level,
dataset-specific preprocessing for ten Kaggle competition datasets and found that
after real feature engineering "model rankings change considerably, performance
differences decrease, and the importance of model selection reduces," and that recent
models "still significantly benefit from manual feature engineering." FACT: its full
text puts a number on the priority order: without feature engineering, the best
achievable average leaderboard position was the 14.5th percentile; without model
selection, the 3rd — feature work mattered roughly five times more than model choice
on that measure. FACT: the TabPFN authors, whose model is the strongest argument ever made for
automated tabular learning, say the same in their Nature paper's guidance: data
scientists "should continue to apply their skills and insights in feature
engineering, data cleaning and problem framing." Assessment: read those two together
and the priority order for a discoverer is settled: the leverage is in what you feed
the model and how you frame the question, not in swapping architectures, which is
also exactly what the basketball literature found ("attributes seem to be more
important than models").

Assessment, from practice rather than the captured sources (label it accordingly):
the richest vein for new features is **error analysis**, studying the games your
current model got most wrong and asking what they share. Were they back-to-backs?
Early-season games before your rolling averages stabilize? Small-conference NCAA
teams with ten games of history? Each cluster of shared failure is a hypothesis, and
tools like SHAP (a game-theoretic method for attributing a model's output to its
inputs) turn a black-box model into a hypothesis generator: when the model leans
hard on a feature in exactly the games it loses, you have found either a bug or a
theory. FACT: the one rigorous SHAP usage in this pool is the Nature paper's, which
compares what different models *learned* feature-by-feature — the same move, used to
audit rather than to discover.

## The state of the art, told honestly (2021-2026)

The question "what's state of the art for prediction on tables of numbers" has a
genuinely interesting answer, because it changed three times in five years, and the
argument was settled the way all good arguments are: on shared benchmarks.

**Act one — trees win.** FACT: Grinsztajn et al. (2022) benchmarked deep learning
against tree methods on 45 datasets and concluded "tree-based models remain
state-of-the-art on medium-sized data (~10K samples)," explaining the gap through
inductive bias: trees are robust to uninformative features and happily learn the
irregular, jumpy functions real tabular data contains. FACT: its most elegant
experiment, visible only in the full text: randomly *rotating* the feature space
reverses the ranking entirely, neural networks beating trees — evidence that trees
win because tabular columns carry individual meaning, which is precisely why
engineered features (an efficiency differential, a rest-day flag) beat learned
mixtures on data like basketball's.

**Act two — deep learning claws back, one tweak at a time.** FACT: TabR (2023)
added a retrieval component (a nearest-neighbors-like lookup inside a feed-forward
network) and beat gradient-boosted trees on average, with tuning, "even on the
recently proposed 'GBDT-friendly' benchmark" (not on every dataset, per its full
results); TabM (2024) got the best tabular deep-learning results
by making one network imitate an ensemble of MLPs, with predictions that are "weak
individually, but powerful collectively"; and a 2024 seventeen-method benchmark
declared "a paradigm shift, where Deep Learning methods outperform classical
approaches" — and its full text shows what that shift actually rests on: the top of
its leaderboard is in-context learners (TabICL, TabPFNv2) and TabM, with median
ranks of roughly 2-5 versus CatBoost's 5.5 and XGBoost's 7, while fine-tuned
foundation models ranked *worst* of the deep family, and the paper concedes the gap
between the boosted trees and the top three is not statistically significant in its
critical-difference analysis. FACT: the most balanced current summary (TALENT, 300+
datasets) lands close by: gradient-boosted trees "remain very strong baselines,"
newer pretrained models "match or surpass them on many tasks, narrowing — but not
eliminating — the historical advantage," and which family wins is largely determined
by *feature-space heterogeneity* (the interplay of categorical and numerical
attributes, sparsity, and entropy variance), not raw dataset size. FACT: a
111-dataset meta-study makes the size point directly: dataset size did **not**
significantly predict whether deep learning wins (p = 0.97); heavy-tailed feature
distributions and classification-versus-regression did, and its meta-model predicts
the winner with 86.1 percent accuracy (trained on the 36 datasets with statistically
significant gaps). Assessment: notice what the
winning tweaks were — retrieval and ensembling, not exotic architectures — and that
every benchmark in this pool agrees on one thing: **ensembling helps everyone,
always**.

**Act three — foundation models arrive.** FACT: TabPFN, published in *Nature* in
2025, is a transformer trained once on roughly 130 million synthetic datasets that
then makes predictions on your new table by in-context learning, no training run at
all: on datasets up to 10,000 rows it "outperforms all previous methods... by a wide
margin," with its 2.8-second default beating baselines tuned for four hours, and it
won on the very benchmark where trees had been declared unbeatable. FACT: the same
paper is admirably blunt about limits: inference is ~1,000x slower per prediction
than CatBoost, memory scales with dataset size, and "for larger datasets and highly
non-smooth regression... CatBoost, XGB or AutoGluon are likely to outperform
TabPFN"; its successors claim to push the ceiling to 50,000 rows and 2,000 features, with
a "100 percent win rate against default XGBoost" on small-to-medium *classification*
(default versus default; regression needs real tuning to match AutoGluon, per the
full text). Those numbers are author-reported by a team with a disclosed commercial
stake (PriorLabs), and one practical caveat matters for this article's readers: the
newest model's license bars commercial use of its outputs without an enterprise
agreement, which includes betting decisions, so check licensing before building on
it. FACT: one contextual number explains why
this matters so much: 76 percent of the datasets on OpenML have fewer than 10,000
rows. Assessment: an NBA season is about 1,230 games and even five seasons of
basketball is a *small* dataset by these standards — meaning your project sits
squarely in the regime where the newest tools are strongest, and a sensible 2026
workflow is: TabPFN and an AutoGluon ensemble as your automated ceiling, XGBoost as
your fast workhorse, and your feature engineering as the thing that actually moves
all three.

## The toolkit and the on-ramp

Assessment on sequencing (the tools are sourced; the order is my read): start with
**fast.ai's free course** (nine ninety-minute lessons, code-first, explicitly
covering tabular analysis, random forests and gradient boosting; it asks for about a
year of coding and high-school math, and its compute advice is to use free cloud
notebooks). Get a strong baseline embarrassingly fast with **AutoGluon** — FACT: its
canonical example is literally three lines (`TabularPredictor(label=...).fit(train)`,
then predict), and its stacked ensembles are a standard strong baseline the field's
papers benchmark against (the TabPFN line reports matching AutoGluon's four-hour
ensembles as its headline result). Track every experiment in **MLflow** (experiment tracking, model
registry, lifecycle management) so that "which tweak helped" is a query, not a
memory. Inspect models with **SHAP**. And treat **Kaggle** as the apprenticeship:
FACT: the March Machine Learning Mania competition has run NCAA-prediction contests
for a decade (the companion article's Kaggle-winning-entry-was-mostly-luck study
came from one), and competition post-mortems are where working methodology gets
written down. Speculation: six months of that loop, run honestly on one stubborn
question, teaches more real data science than any credential.

## Where parlays and modern sportsbook products fit

You asked how the stuff on modern sportsbooks relates, and the answer is: it is the
same mathematics, run by the other side, at industrial scale.

FACT: the hold ladder across the cited explainers runs roughly: straight bets 4-5
percent (the 52.38 percent game from the companion article); player props 8-12
percent per leg; parlays around 20 percent overall — the one regulator-adjacent
figure is New Jersey books holding 19.9 percent on parlays in January 2024; and
same-game parlays 15-25 percent or more, with cross-game SGP products reported at
25-35 percent. FACT: the compounding works in both directions: a genuine 55 percent
bettor at -110 earns about 5 percent on straights but 10.1 percent on two-leg
parlays, while a 50 percent bettor loses 4.6 percent on straights and 9 percent on
parlays — and a Kelly analysis in the same source still favors straight bets for
bankroll growth (31 percent versus 21 percent over 100 simulated iterations).

![Diagram: what each bet type costs you in typical hold. Straight bets about 4.8 percent, the 52.38 percent game. Player props 8 to 12 percent per leg. Cross-game parlays about 20 percent, with New Jersey books holding 19.9 percent in January 2024. Same-game parlays 15 to 25 percent or more, the correlation tax. SGP-plus products 25 to 35 percent reported. A footer notes the same ten dollars of opinion costs about 22 cents of expected value as three straight bets versus about 1.49 dollars as one three-leg same-game parlay, roughly seven times more.](img/hold-ladder.png)
*The hold ladder: why the products the apps push hardest are the worst bets in the
building. Diagram.*

FACT: same-game parlays are the interesting case for a modeler, because they are a
*joint-probability modeling product*: books banned correlated parlays for decades
(correlation favored bettors), then around 2019 flipped and productized them with
proprietary correlation engines — Gaussian copulas and historical correlation
matrices per the Wizard of Odds walkthrough — charging a "correlation tax" that
prices a three-leg combination worth roughly +594 independent at around +350, for a
worked house edge near 15 percent versus 4-5 on singles; the same source computes
that the same ten dollars of opinion costs about seven times more expected value as
an SGP than as singles. Assessment: for someone whose interest is the *modeling*,
the takeaways are three: the book's SGP engine is exactly the kind of calibrated
joint-outcome model this article teaches you to build, which is why you should
respect it; the occasional mispriced *negatively* correlated combination is the one
computed case in these sources where the math tips toward the bettor, and all three
explainer sources warn it is fragile; and identical SGPs price 10-20 percent apart
across books, so if you play at all, line shopping is the only free lunch on the
menu. (A *Journal of Prediction Markets* paper studies correlated-parlay
profitability in college football; our capture of it failed beyond the title, so it
goes in further reading, unverified.)

## The starting checklist, discovery edition

1. Pick one narrow question you actually care about ("do rest days move NBA margins
   more than the market prices?").
2. Lock the benchmark first: which seasons, which metric (margin error and
   calibration), which baseline. Write it down before touching data.
3. Get the automated ceiling in a day: AutoGluon preset and TabPFN on your table.
   Your job is now to beat robots with domain knowledge.
4. One change at a time, logged in MLflow, keep-or-kill.
5. Read your errors weekly; every failure cluster is a candidate feature.
6. Judge yourself on calibration and the locked metric, never a hot streak (the
   companion article's sample-size math applies to research exactly as to betting).
7. When a tweak survives, try to kill it on a season you never touched. What
   survives that is a finding.

## How much to trust this

Assessment: the trust profile improved substantially after first publication: the
full texts of all ten arXiv papers were pulled and every cited claim verified against
them, which confirmed the quotes verbatim, sharpened several (the TALENT
heterogeneity finding, the meta-study's size result, TabR's on-average caveat), and
surfaced the TabPFN licensing restriction. The load-bearing numbers now rest on one
full-text *Nature* paper (authors disclose a commercial affiliation) plus verified
full texts; the sequencing advice and the error-analysis workflow remain practitioner
judgment, labeled as Assessment throughout. The parlay economics come from betting-
industry explainers of varying quality: the hold ladder converges across four
independent sources and one near-regulatory figure (NJ 19.9 percent), but exact
operator numbers are unsourced in the originals, one source is a content site of
low provenance, and our copy of the Wizard of Odds piece may be a localized mirror;
treat every specific SGP price as illustrative. The one peer-reviewed parlay paper
in the pool could not be read past its title. The core claims that survive all
these caveats: feature work beats model shopping, shared benchmarks are how truth
gets settled, ensembles always help, small-data foundation models are real, and
every step up the parlay ladder pays the house more.

## See also

- **[Building a basketball spread model](../basketball-ats-model)** — the companion:
  this note's loop, applied end-to-end to one concrete model.
- **[AI agent engineering](../../ai-engineering)** — the same evaluation discipline
  aimed at LLMs instead of gradient boosting.
- **[How to read the evidence](../../leadership/how-to-read-the-evidence)** — the
  vault's decoder for calibration, effect sizes, and benchmark claims.
- **[Using AI well](../../../connections/using-ai-well)** — the habit underneath all
  of it: a confident number is a claim to test.

## Sources

- Gorishniy et al., "Revisiting Deep Learning Models for Tabular Data" (arXiv, full
  text) — https://arxiv.org/abs/2106.11959
- Grinsztajn, Oyallon & Varoquaux, "Why do tree-based models still outperform deep
  learning on tabular data?" (arXiv, full text) — https://arxiv.org/abs/2207.08815
- Hollmann et al., "Accurate predictions on small data with a tabular foundation
  model" (TabPFN), *Nature* 637:319-326 (2025, full text; PriorLabs affiliation
  disclosed) — https://www.nature.com/articles/s41586-024-08328-6
- TabPFN v1 (arXiv, full text) — https://arxiv.org/abs/2207.01848 · TabPFN-2.5
  (arXiv, full text; author-reported, non-commercial license) — https://arxiv.org/abs/2511.08667
- TabR (arXiv, full text) — https://arxiv.org/abs/2307.14338 · TabM (arXiv, full
  text) — https://arxiv.org/abs/2410.24210
- Zabërgja et al., "Tabular Data: Is Deep Learning All You Need?" (arXiv, full text;
  revised 2025) — https://arxiv.org/abs/2402.03970
- TALENT benchmark (arXiv, full text) — https://arxiv.org/abs/2407.00956 ·
  111-dataset meta-study (arXiv, full text) — https://arxiv.org/abs/2408.14817
- Tschalzev et al., "A Data-Centric Perspective on Evaluating Machine Learning
  Models for Tabular Data" (arXiv, full text) — https://arxiv.org/abs/2407.02112
- Tools: fast.ai course — https://course.fast.ai/ · AutoGluon —
  https://github.com/autogluon/autogluon · MLflow — https://mlflow.org/docs/latest/
  · SHAP — https://shap.readthedocs.io/en/latest/
- Parlay math: Establish The Run, "The Math Behind Parlay Betting" (2024) —
  https://establishtherun.com/the-math-behind-parlay-betting/ · Wizard of Odds,
  "Same-Game Parlays: The Mathematics of Correlation" (capture may be a mirrored
  page) — https://wizardofodds.com/article/same-game-parlays-the-mathematics-of-correlation/
  · OddsIndex SGP correlation guide (affiliate site) —
  https://oddsindex.com/guides/same-game-parlay-correlation · shutterb.org SGP
  pricing (unsourced tables) — https://shutterb.org/same-game-parlay-pricing-correlation-limits/
  · tech-insider.org parlay explainer (low provenance; its unique figures are
  unverified) — https://tech-insider.org/sports-betting/parlay-betting-explained/
- Further reading, unverified beyond its title: "Correlated Parlay Betting" ,
  *Journal of Prediction Markets* — https://www.ubplj.org/index.php/jpm/article/view/1562
