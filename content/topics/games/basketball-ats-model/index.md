---
type: note
series: games
title: "Building your own model to beat the basketball spread (and why the spread usually wins)"
status: curated
tags: [games, basketball, machine-learning, betting, data-science, verification]
created: 2026-07-19
---

# Building your own model to beat the basketball spread

This is a build guide for training your own model to predict NBA and NCAA basketball
games against the spread, written the way this vault writes everything: the craft in
full, and the odds of success stated honestly up front. The one-sentence version: you
can absolutely build a good margin-prediction model with free tools and public data,
and the published evidence says the spread market will still be roughly as good as your
model, because the spread *is* a prediction market that has already digested most of
what you know. The realistic goals are a well-calibrated model, an honest measurement
habit, and the ability to recognize a real edge if you ever find one. Labels
throughout: FACT is attributed to a named source; Assessment is my read; Speculation
is flagged.

## First, the wall you are up against

FACT: at standard -110 odds you risk 110 to win 100, so the break-even cover rate is
11/21, or 52.38 percent; that arithmetic is stated in both Sport Journal studies cited
below and you can verify it on a napkin. Everything you build must clear that bar, not
50 percent.

FACT: in a study of 10,325 NBA games across eight seasons (2000-01 through 2007-08),
Compton and Sigler found underdogs covered 49.86 percent of the time, the over hit
49.87 percent, and the average closing spread (5.89 points) tracked the average actual
margin (5.38) closely; statistically, they could not reject that the market is a fair
coin. FACT: the same study's best anomaly, big home underdogs, covered 59.52 percent
over the full sample but was statistically profitable only in the first half of the
sample, and in the authors' words the inefficiency "fades in the most recent sample
period."

FACT: the pool's clearest contradiction is instructive: Byrnes and Farinella report
that betting *with* four-game streaks returned 56.5 percent against the spread across
2001-2013, while the earlier literature they summarize (Paul and Weinbach) found the
*opposite* strategy, fading streaks, was profitable across 1995-2002. Assessment: when
two published edges point in opposite directions in adjacent eras, the honest reading
is that these are unstable patterns dredged from finite samples, not durable laws, and
whatever you dig out of a backtest deserves the same suspicion.

FACT: the most complete recent attempt in the source pool, a 2026 study in
*Information* that trained uncertainty-aware models on fourteen NBA seasons with odds
data, found its spread models "neutral to mildly negative" even in a simulation the
authors themselves describe as an upper bound (no bet limits, no slippage); the only
profits appeared on moneylines, from calibrated probabilities plus disciplined staking,
and the authors read their own result as evidence that spreads "are more efficiently
priced."

![Diagram: the wall, by the numbers. 52.38 percent is the break-even cover rate at -110. 49.86 percent was the underdog cover rate over 10,325 NBA games from 2000 to 2008, almost exactly a coin flip. About 4,300 games are needed to statistically separate a true 53 percent bettor from a coin-flipper at 95 percent confidence. The 56.5 percent momentum edge of 2001-2013 directly contradicts the earlier fade-the-streak edge of 1995-2002, showing data-mined edges flip. The one full machine-learning spread model in the pool was flat-to-negative on spreads even in an idealized simulation. Choosing a model by calibration versus accuracy produced one-season moneyline ROI of plus 34.7 percent versus minus 35.2 percent.](img/the-wall.png)
*The wall, in six sourced numbers. Diagram.*

FACT: the one paper claiming "above market returns" across these leagues (arXiv
1910.08858, full text now verified) turns out to prove a subtler point: its strategy
bets moneylines only, taking the best price across sixteen sportsbooks, and its own
random-bet baselines show random *spread* bets losing about 4.4 percent everywhere
(the NBA confidence interval excludes zero), with the authors concluding they "cannot
reject the null hypothesis that sports betting markets are jointly efficient." Their
reported NBA returns of 9 to 11 percent came with selection thresholds tuned on the
same sample being scored, and much of the edge is price-shopping across books rather
than out-predicting any one of them. Assessment: even the pool's boldest inefficiency
claim, read in full, is a moneyline line-shopping result, not a spread-beating model.

Assessment: none of this means "don't build the model." It means the model's job is
different than beginners assume: you are not trying to predict basketball, you are
trying to out-predict a market's consensus number, and the gap between those two tasks
is the entire subject of this article.

## What "predicting against the spread" actually means

The clean formulation: build a model that predicts the **point margin** of a game,
then compare your predicted margin to the posted spread. If the line says home -6.5
and your model says home by 9 with enough confidence, the home side is your bet; the
market's implied probability at -110 is roughly 50 percent either way, so your edge is
whatever honest probability your model assigns beyond 52.38 percent. FACT: the
margin-prediction route has real support in the literature: an arXiv study predicted
NCAA margins from team rankings alone and found even simple regressions "fairly
accurate," and the best pre-game NBA score model in the pool (Chen et al., *Entropy*
2021) reached a mean error of about 8.2 percent on team scores (RMSE roughly 11.5
points) using only statistics from previous games. FACT: the same NCAA study's full text
gives the head-to-head this article needs: on 1,506 identical validation games, the
Vegas line predicted margins with an RMSE of 10.34 points while the best rankings-only
model managed 10.92, and the authors compute an irreducible noise floor around 11.2,
meaning the models were already near the ceiling and the line *still* beat them by
half a point. Assessment: that is the game you are in, stated as one number: you and
the market are both throwing darts with wide error bars at the same board, the
market's aim is measurably better, and you win only by being *slightly* less wrong,
thousands of times.

## The data stack (all of it accessible to one person)

**NBA.** FACT: `nba_api` is a free, MIT-licensed Python client for NBA.com's stats and
live endpoints (career stats, box scores, play-by-play, today's scoreboard), with the
documented caveat that NBA.com does not announce endpoint changes, so the community
maps them as they break. FACT: `hoopR`, the sportsdataverse R package, wraps the NBA
Stats API (127 functions) and loads ESPN play-by-play at scale; its docs cite 619,841
rows from 1,279 NBA games loading in about 3.4 seconds.

**NCAA.** FACT: `hoopR` also loads men's college play-by-play (its docs cite 2.9
million rows across 6,275 games in about 11 seconds) and includes a KenPom scraper
that works only with an active KenPom subscription. FACT: Bart Torvik's free T-Rank
site publishes adjusted offensive and defensive efficiency, tempo, and a power rating
("Barthag") for all 365 Division I teams, plus returning-minutes, experience, and
talent columns; its methodology is undisclosed (the site jokes about "sacred data and
secret formulas"), so treat it as a useful black box, not ground truth.

**Odds — the ingredient beginners skip.** FACT: The Odds API serves point spreads,
moneylines, and totals for the NBA and NCAA across 40+ bookmakers as JSON, with a free
tier of 500 credits a month; historical odds, which any honest backtest requires, are
available only on paid tiers (from \$30/month). Assessment: without a historical odds
feed matched game-by-game to your features, you cannot evaluate against the spread at
all, and the join must use the odds *as they stood before tip-off*, ideally both an
opening and closing snapshot; grabbing today's lines and yesterday's box scores is how
backtests quietly lie.

## Features: where the actual work is

FACT: two independent sources in the pool make the same claim in almost the same
words: Zimmermann's foundational NCAA study concluded "attributes seem to be more
important than models," and the Bunker review quoted in the graph-network paper says
appropriate feature selection "appears to be more important for the accuracy than the
availability of a large number of games." Assessment: spend your time here, not
shopping architectures.

What the evidence says earns its place:

- **Efficiency differential above all.** FACT: in the one study that ran formal
  feature selection across 44 team statistics (Zhao et al., *Entropy* 2023), the
  efficiency differential dominated both selectors, leading random-forest importance
  at 0.38 (defensive rating and floor impact next by that measure) and LASSO at
  0.36; nothing else came close.
- **Rolling averages of prior games only.** FACT: Chen et al. tuned how many previous
  games to average and found four games optimal for score prediction, with defensive
  rebounds, two-point percentage, free-throw percentage, offensive rebounds, assists,
  and three-point attempts the six features that survived ensemble ranking.
- **Home court, rest, schedule.** FACT: home teams won 55.4 percent of games in the
  PLOS One sample (2020-2023), and days-off appears as a standard feature in the
  *Entropy* datasets. Assessment: for NCAA, home court is bigger and travel wilder;
  rankings and adjusted efficiency (Torvik's AdjOE/AdjDE) carry most of the signal,
  which is why margin-from-rankings works at all.
- **The line itself.** FACT: in the 2026 *Information* study, the market-implied
  probability was the single most important feature by SHAP, and a well-calibrated
  logistic model built mostly on market features lost money precisely because it
  "effectively reweights existing market information rather than uncovering
  independent signal"; their fused market-plus-team-features model beat both
  market-only and stats-only variants. Assessment: this is the crucial design insight.
  Use the line as a feature and a prior, then ask what your model knows that the line
  does not; a model that cannot articulate its disagreement with the market is just an
  expensive mirror.

What is missing from the published pool and must be your own work: injuries and
lineup news (the single most time-sensitive input in basketball, absent from every
study captured here), coaching changes, and NCAA conference-depth effects.
Speculation: to the extent any private edge still exists for a solo modeler, it most
plausibly lives in reacting to injury and lineup information faster than small-market
NCAA lines adjust, not in better aggregate statistics.

## Modeling: boring first, calibrated always

Assessment: the pool's results justify a plain recipe. Start with two baselines you
must beat: the closing line itself (predict the spread as the margin; this is the
market) and a ridge or logistic model on efficiency differentials. Then a gradient
boosted tree (XGBoost was the repeated winner or near-winner across three of the
empirical papers) on your lagged feature set, predicting margin as a regression.
Deep options exist (an LSTM/Transformer NCAA study reached AUC 0.847), but nothing in
the pool suggests they matter more than features and validation.

Then calibrate, because the betting decision consumes probabilities, not accuracy.
FACT: the pool's most directly on-point experiment (Walsh and Joshi, arXiv 2303.06021,
published in *Machine Learning with Applications*; full text verified) trained NBA
models on the 2014-2018 seasons and bet the 2018-19 season at published closing
odds. Selecting the model by *calibration* versus by *accuracy* produced an average
return of +34.69 percent versus -35.17 percent, across roughly 1,100 bets. FACT: read
the fine print, which we did in the full text: the experiment is **moneyline** betting,
not spreads; both branches used the same algorithm (an SVM) differing in feature
choice; and the accuracy branch's -35 percent average hides a split of +5.56 percent
on flat stakes versus -75.9 percent under eighth-Kelly, so the real lesson is that
*miscalibrated probabilities plus Kelly sizing equals ruin*. The authors themselves
warn a single season proves little.
FACT: the conformal-prediction NCAA study makes the same argument from the other
direction: its win probabilities were "better calibrated than other methods" across
seven seasons of college data, and its full text explicitly extends the method to
spread betting by estimating the probability that the margin of victory lands at or
under the spread, a distribution-free statistical basis for exactly the
margin-to-cover-probability step in the skeleton below. FACT: the NCAA deep-learning
study adds loss-function-level support: training on Brier loss roughly halved
calibration error versus standard cross-entropy (ECE 2.3 versus 4.1 percent in its
men's-tournament table).
Assessment: isotonic regression or Platt scaling on a held-out validation season is
the practical move; check the calibration curve, not just the score.

```python
# The skeleton, honestly ordered (Python, sklearn/xgboost pseudocode)
for test_season in [2023, 2024, 2025]:            # walk-forward, never shuffled
    train = games[games.season < test_season - 1]  # past only
    val   = games[games.season == test_season - 1] # calibration season
    test  = games[games.season == test_season]
    X_cols = lagged_features + [closing_spread]    # nothing from the game itself
    m = xgboost.XGBRegressor(**params).fit(train[X_cols], train.margin)
    cal = IsotonicRegression().fit(                # margins -> cover probabilities
        cover_prob_raw(m.predict(val[X_cols]), val.spread), val.covered)
    p = cal.predict(cover_prob_raw(m.predict(test[X_cols]), test.spread))
    bets = test[(p > 0.5238 + edge_buffer)]        # only past break-even + buffer
    log_results(bets, p)                           # every bet, forever, no edits
```

## Validation: the section that decides whether you fooled yourself

This is where most published work stumbles, and the pool documents it vividly.

FACT: a *Scientific Reports* stacked-ensemble paper reports 83.27 percent accuracy
predicting NBA winners, and a PLOS One XGBoost paper reports 93.3 percent, but both
feed the model the *predicted game's own box score*: the PLOS paper is explicit that
accuracy is 72 percent at halftime, 79.8 percent through three quarters, and 93.3
percent with the full game, none of which exists before tip-off. FACT: honest pre-game
NBA winner accuracy in the pool's survey runs roughly 60 to 76 percent, and the
graph-network paper that assembled that survey itself wired each game's graph to the
team's *next* game, letting future structure leak into training. Assessment: the recurring failure is
letting the future touch the training set, and the fix is mechanical: **temporal
splits only**, walking forward season by season, exactly as the 2026 *Information*
study does (train through 2022, validate 2023, test 2024) — the only study in the
pool that both did this and evaluated against real odds.

Then there is sample size. FACT (arithmetic, checkable): distinguishing a true 53
percent cover rate from 50 percent at 95 percent confidence requires on the order of
4,300 bets; an NBA season offers about 1,230 games, most of which you will not bet.
FACT: the strongest cautionary tale in the pool is the Kaggle NCAA study whose entry
*won* the tournament-prediction contest outright and whose authors then estimated, by
10,000 simulated tournaments, that the winning entry had "no more than about a 12
percent chance" of winning, under 50 percent odds of even a top-ten finish, and in
their words: "even if you knew the true probabilities of a win for every single game
with certainty, you'd still only win about 1 in 8 times." FACT: the same full text
hands the article its best design exemplar: the winning model was simply an ensemble
of a logistic regression on the Vegas point spread and a logistic regression on
KenPom adjusted efficiencies — the line as a prior, blended with public efficiency
numbers. Assessment: a season of good results
proves almost nothing; plan to judge yourself over years, or on the sharper proxy in
the next section.

## Measuring success like a professional

Assessment, drawing on practitioner consensus rather than the academic pool (label it
accordingly): the metric serious bettors actually track is **closing line value**,
whether the number you bet consistently beats the closing number, because the close
is the market's most-informed price and beating it repeatedly is measurable in dozens
of bets instead of thousands. The pool supports the premise indirectly: FACT: Compton
and Sigler cite prior literature that the betting public "removes biases in sport
book's opening lines in NBA betting by game time," which is why efficiency tests run
against the close. So: log every simulated bet with the line you "took" and the
closing line; if you are not beating the close, your profit-and-loss is noise no
matter how it looks.

For staking, the 2026 *Information* study's discipline is the sane template. FACT:
it used fractional Kelly at 0.3x with a minimum expected-value threshold and per-bet
caps, and most stakes landed between 3 and 9 percent of bankroll; its authors still
labeled the resulting equity curves an upper bound on reality. Assessment: paper-trade
with flat stakes or 0.2-0.3x Kelly for months before a single real dollar moves, and
treat any simulated return as optimistic by construction, since no simulation includes
limits, worse prices, or your own discipline failing on a Tuesday.

![Diagram: the honest spread-model pipeline in eight steps. Data (games, box scores, odds history), features (lagged averages only, efficiency differentials, rest), margin model (ridge or XGBoost), calibrate (turn margins into honest cover probabilities), compare to the line (your probability versus the spread's implied 50 percent), paper-trade first (flat stakes or 0.2 to 0.3 Kelly), track versus the close (did the line move toward your number), retrain weekly (walk-forward only). A panel lists the two traps that fake success: leakage (training on the game's own box score, which produces 83 to 93 percent accuracies that don't exist before tip-off, versus a realistic 60 to 76 percent pre-game) and random splits (split by time, always).](img/pipeline.png)
*The build loop, with the two traps that fake success. Diagram.*

## NBA versus NCAA: where to point the effort

Assessment, from the pool's shape: the NBA offers the best data (every study with
full text was NBA) and the sharpest market; the 2026 study's finding that NBA spreads
were unbeatable while marginal moneyline value existed is consistent with a market
that has already priced the stats you can compute. The NCAA offers 365 teams, wilder
variance, free adjusted-efficiency infrastructure (Torvik), and thinner betting
attention on small-conference games. FACT: the NCAA-side evidence in the pool is
about winners and brackets, not spreads: rankings alone predict margins tolerably,
and a rank-fusion study's ensemble of five machine-learning models beat the best of
ten public ranking systems 74.60 to 73.02 percent (a correction from this article's
first edition, which misread the abstract as fusing the ranking systems themselves;
the full text says otherwise). Direct NCAA spread-efficiency tests remain absent from
the pool, though the "Beating the House" full text's random-bet baseline had random
NCAAB spread bets losing about 4.4 percent, and older literature it summarizes found
at most limited, era-bound inefficiencies. Speculation: that
absence cuts both ways, and if you want a research project rather than a donation to
a sportsbook, small-conference NCAA spreads are the least-studied corner in this
entire source pool.

## The build checklist

1. Get historical odds first (paid tier of The Odds API or equivalent); no odds, no
   project.
2. Pull five-plus seasons of games and box scores (`nba_api`/`hoopR`; Torvik for
   NCAA efficiency).
3. Build lagged features only: rolling efficiency differentials, four-factors,
   rest, home; join the pre-game line to every row.
4. Baselines: the line itself, then ridge on efficiency differential.
5. XGBoost on margin; walk-forward by season; calibrate on the held-out season.
6. Bet simulation with an edge buffer past 52.38 percent, flat stakes, every bet
   logged with the closing line.
7. Judge on calibration curves and closing-line value, not on a hot month.
8. Only then consider deep models, injury feeds, or real money, in that order.

## How much to trust this

Assessment: the build mechanics (data sources, features, walk-forward validation,
calibration) rest on full-text peer-reviewed studies and tool documentation and are
solid. Since first publication, the full texts of every arXiv source have been pulled
and verified line by line, which upgraded this article twice over: the headline
calibration ROI is confirmed but is a *moneyline* result with a Kelly blow-up inside
its accuracy branch, the boldest inefficiency claim turned out to be moneyline
line-shopping across sixteen books with in-sample-tuned thresholds, and one claim in
the first edition was corrected outright (the bracket-fusion study fused five machine
learning models, not the ten public ranking systems it was benchmarked against). The
market-efficiency picture still leans on one large but dated NBA study (2000-2008)
plus one modern full-text modeling study (2026), and direct NCAA spread-efficiency
tests remain absent beyond random-bet baselines. And
the deepest caveat is structural: every published edge in this pool either faded,
reversed in the next era's data, or survived only in an idealized simulation, so the
honest prior for your own model is that it will be well-calibrated, instructive, and
roughly break-even against the vig, and that a durable edge, if you find one, will
announce itself through months of closing-line value, not a winning streak.
Speculation: the most valuable output of this project is not betting profit; it is
that you will never read a "60 percent guaranteed winners" pitch the same way again.

## See also

- **[How prediction research actually works](how-to-discover)** — the companion: the
  discovery loop behind this build, the state of the art in tabular prediction
  (trees vs deep learning vs foundation models), and the parlay/SGP math of modern
  sportsbooks.
- **[AI agent engineering](topics/ai-engineering/)** — the same discipline on different
  models: evaluation, leakage, and not fooling yourself, applied to LLMs.
- **[How to read the evidence](topics/leadership/how-to-read-the-evidence)** — the
  vault's decoder for calibration, effect sizes, and why "statistically significant"
  is a low bar.
- **[Using AI well](connections/using-ai-well)** — a confident model output
  is a claim to test, not a prediction to bet.

## Sources

- Compton & Sigler, "NBA Gambling Inefficiencies: A Second Look", The Sport Journal
  (2012) — https://thesportjournal.org/article/nba-gambling-inefficiencies-a-second-look/
- Byrnes & Farinella, "The Effect of Momentum on the NBA Point Spread Market", The
  Sport Journal (2016) — https://thesportjournal.org/article/the-effect-of-momentum-on-the-nba-point-spread-market/
- Uncertainty-Aware Machine Learning for NBA Forecasting in Digital Betting Markets,
  *Information* 17(1):56 (2026) — https://doi.org/10.3390/info17010056
- Walsh & Joshi, "Machine learning for sports betting: should model selection be
  based on accuracy or calibration?", *Machine Learning with Applications* (full text
  verified; moneyline experiment) — https://arxiv.org/abs/2303.06021
- Zhao, Du & Tan, GCN with feature selection for NBA prediction, *Entropy* (2023) —
  https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10217531/
- Chen, Jhou, Lee & Lu, hybrid two-stage score prediction, *Entropy* (2021) —
  https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8073849/
- Ouyang et al., XGBoost + SHAP by game stage, *PLOS One* (2024) —
  https://pmc.ncbi.nlm.nih.gov/articles/PMC11265715/
- He & Choi, stacked ensemble NBA prediction, *Scientific Reports* (2025) —
  https://www.nature.com/articles/s41598-025-13657-1
- Zimmermann et al., NCAAB match prediction (arXiv, full text) — https://arxiv.org/abs/1310.3607
- Lopez & Matthews, Kaggle NCAA contest luck analysis (arXiv, full text) — https://arxiv.org/abs/1412.0248
- Margin of victory from rankings, NCAA (arXiv, full text; line-vs-model RMSE) — https://arxiv.org/abs/1701.07316
- Conformal win probability for March Madness (arXiv, full text) — https://arxiv.org/abs/2208.08598
- LSTM/Transformer NCAA forecasting (arXiv, full text) — https://arxiv.org/abs/2508.02725
- Combinatorial fusion for brackets (arXiv, full text) — https://arxiv.org/abs/2603.10916
- "Beating the House" (arXiv, full text; moneyline line-shopping, thresholds tuned
  in-sample) — https://arxiv.org/abs/1910.08858
- Gibbs, "Point Shaving in the NBA", Stanford honors thesis (2007, abstract) —
  https://purl.stanford.edu/nh108vv2047
- Tools: nba_api — https://github.com/swar/nba_api · hoopR —
  https://github.com/sportsdataverse/hoopR · The Odds API — https://the-odds-api.com/
  · Bart Torvik T-Rank — https://barttorvik.com/
