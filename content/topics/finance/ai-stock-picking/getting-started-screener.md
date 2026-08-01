---
type: note
title: "Getting started: a 12-month factor screener"
status: curated
tags: [finance, investing, quant, factor-investing, verification]
created: 2026-07-22
---

# Getting started: a 12-month factor screener

This note is a concrete starting roadmap for building a stock screener you would hold for about
twelve months and rebalance once a year, as opposed to the day-trading robots in
[the landscape](the-landscape). It is the practical companion to the
[honest-method note](build-it-yourself) and the [evidence note](does-it-actually-work); read
those for the why, and this for the where-to-begin.

The encouraging part, up front: a 12-month buy-and-hold, annually-rebalanced screener is the most
defensible version of everything in this wing. The two forces that kill most strategies, trading
costs and the fragility of fast-decaying signals, barely touch you at annual turnover. FACT:
Novy-Marx and Velikov (2016) found that low-turnover anomalies mostly survive real trading costs
while high-turnover ones generally do not. Assessment: you are starting in the friendly regime,
not the minefield, which is exactly why this is the version worth building.

## Step 1: fix the benchmark and universe first

Assessment: decide what you are trying to beat before you write a line of code, because it
governs everything. The benchmark is a cheap index fund (total US market or the S&P 500), and the
screener's only job is to beat it net of costs and taxes. Define the universe with a liquidity and
market-capitalization floor, and exclude microcaps. Microcaps are where illusory edges hide (the
machine-learning literature finds signal profits concentrate there) and where you cannot actually
trade at size, so leaving them in flatters the backtest and misleads you.

## Step 2: start with factors, not machine learning

Assessment: for a 12-month hold, reach for the classic, replicated, low-turnover factor premia,
not a neural network. The neural net buys you almost nothing at this horizon and adds enormous
overfitting risk; the transparent factor composite is both more honest and more likely to survive
contact with real money. The core factors:

- **Value:** cheap relative to fundamentals (book-to-market, earnings or free-cash-flow yield).
- **Quality / profitability:** profitable, stable, low-debt companies (gross profitability, return
  on equity, low accruals).
- **Low volatility:** calmer stocks have delivered better risk-adjusted returns than theory
  predicts.
- **Momentum, weighted lightly:** recent winners. One real caveat for an annual hold: pure
  12-minus-1 momentum is higher-turnover because the signal shifts within the year, so it fits
  annual rebalancing less cleanly than value and quality. Weight it lighter, or refresh the
  momentum sleeve more often than the rest.

FACT: these premia replicate broadly (Chen and Zimmermann 2022 reproduced 98 percent of clearly
significant published predictors; Jensen, Kelly, and Pedersen 2023 found most factors replicate
across 93 countries), which is why they are the honest foundation rather than the newest signal.

## Step 3: get point-in-time, survivorship-free data (the make-or-break)

Assessment: this is the boring step where most do-it-yourself screeners silently die, so treat it
as the hard part, not an afterthought. You need two things. First, point-in-time fundamentals: the
numbers as they were first reported on each historical date, not later restatements, or the model
"knows" results before the market did. Second, a survivorship-free universe that keeps delisted
and bankrupt companies and their delisting returns, or the backtest never buys the stocks that
went to zero and every result is biased upward.

Realistic data options, roughly by cost: Sharadar or Norgate (affordable, roughly point-in-time);
Compustat and CRSP (the academic gold standard, expensive); and free sources such as yfinance,
SimFin, or Financial Modeling Prep. Assessment: free data almost always has survivorship and
look-ahead problems baked in, and it will lie to you in a specifically upward direction, so use it
only for learning the mechanics, not for a result you would trust with money.

## Step 4: build the pipeline in the honest order

The mechanical recipe, the same one every serious multi-factor shop runs:

1. Assemble the universe on each rebalance date (point-in-time, liquidity floor, no microcaps).
2. Compute the raw signal per stock for each factor; flip the sign so higher always means better.
3. Winsorize (clip the extreme 1st and 99th percentiles) so one data error cannot dominate.
4. Z-score each factor across stocks, usually within industry, so you compare tech to tech.
5. Equal-weight the factor z-scores into one composite. Equal weighting is the honest default;
   tuning the weights quietly invites data snooping.
6. Rank, take the top quintile, rebalance once a year.

Assessment: transparent and auditable beats clever. If you cannot explain in one sentence why a
stock scored where it did, you have built something you cannot trust.

## Step 5: backtest for truth, not for a pretty curve

Walk-forward: fit and tune on early years, then test on strictly later years you never looked at,
and roll forward. Model transaction costs even though turnover is low. Then run the two tests that
separate a real edge from a lucky fit:

- **Factor-attribution regression.** Regress your screen's returns on the known factor returns
  (market, size, value, momentum, profitability). If the alpha intercept is statistically
  indistinguishable from zero, you have not built skill, you have rebuilt a factor fund. That is
  fine to know, but then you should just buy the fund (Step 6). Kenneth French's data library
  provides the factor returns to regress against.
- **Deflate the Sharpe by your trial count.** If you tried forty factor combinations and kept the
  best, that best one looks good partly by luck; the Deflated Sharpe Ratio discounts it for how
  many you tried. Assessment: honestly counting your trials is the single most-skipped and
  most-important step.

## Step 6: the gut-check that saves you months

Assessment: before trusting any homemade screen, benchmark it against simply buying the
equivalent factor exchange-traded funds, for example VTV (value), QUAL (quality), MTUM
(momentum), USMV (low volatility), or a single multifactor fund. Done honestly, a factor screener
largely reconstructs what those funds already sell for a few basis points a year. If your screen
cannot clearly beat a cheap ETF net of your costs and your time, the ETF is the answer, and that
is a genuine result, not a failure.

The legitimate reasons to build it anyway: to learn the machinery deeply, to customize tilts the
funds do not offer (concentration, exclusions, a specific factor blend), or to test whether you
can find something beyond the known factors. That last one is the actually-interesting frontier,
and it is hard precisely because the known factors are already priced.

## Expectations, set honestly

Assessment: the realistic prize is a modest tilt, perhaps a point or two of annualized excess
return over a full decade, with real tracking error and multi-year stretches of underperformance.
The value factor was close to dead money from roughly 2007 to 2020 before rebounding; a real
premium can go missing for over a decade, and you have to be able to hold it through that. If your
backtest instead shows large, smooth alpha, the base case is not that you are brilliant; it is
that you have a look-ahead or survivorship bug, or you are overfit. A big backtest number is best
read as a red flag pointed at yourself.

## The stack

Assessment: this is the same machinery as the [basketball spread model](topics/games/basketball-ats-model/),
so if you built that you already have the skills. Python and pandas for the data and features; a
simple custom cross-sectional backtester (cleaner than backtrader or zipline for a ranked,
periodically-rebalanced basket, which those timing-oriented frameworks handle awkwardly);
statsmodels for the attribution regression; and Kenneth French's data library for the factor
returns. Start with a single factor on a small universe end to end before adding the rest, so
every piece is debugged in isolation.

## See also

- **[How to emulate it without fooling yourself](build-it-yourself)** — the discipline behind
  every step here.
- **[Does any of it actually beat the market?](does-it-actually-work)** — the base rate this
  screener is also up against.
- **[Building your own basketball spread model](topics/games/basketball-ats-model/)** — the same
  pipeline applied to sports.

## Sources

- Novy-Marx & Velikov (2016), "A Taxonomy of Anomalies and Their Trading Costs," *Review of Financial Studies* — **[peer-reviewed]** — https://academic.oup.com/rfs/article-abstract/29/1/104/1844518
- Chen & Zimmermann (2022), "Open Source Cross-Sectional Asset Pricing," *Critical Finance Review* — **[peer-reviewed]** — https://www.nowpublishers.com/article/Details/CFR-0112
- Jensen, Kelly & Pedersen (2023), "Is There a Replication Crisis in Finance?," *Journal of Finance* — **[peer-reviewed]** — https://onlinelibrary.wiley.com/doi/abs/10.1111/jofi.13249
- Bailey & López de Prado (2014), "The Deflated Sharpe Ratio," *Journal of Portfolio Management* — **[peer-reviewed]** — https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2460551
- Kenneth R. French Data Library (factor returns for attribution) — **[data repository]** — https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/data_library.html
