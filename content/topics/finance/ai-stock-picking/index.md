---
type: note
title: "How AI and quant stock-picking actually works"
status: curated
tags: [finance, investing, quant, machine-learning, factor-investing, verification]
created: 2026-07-22
---

# How AI and quant stock-picking actually works

This note explains, mechanically and plainly, how the AI and "quantitative" stock-picking
tools you see advertised actually work: multi-factor scores like the Zacks Rank, "fair value"
numbers like InvestingPro's, machine-learning return models, news and social-media sentiment
engines, and chart-pattern robots like Trade Ideas' Holly and Tickeron. For each one I cover
what data goes in, what comes out, and the specific ways it can fool you. AI here means
artificial intelligence; quantitative (quant) just means driven by numbers and rules instead
of a human's judgment.

The honest through-line, up front: every method below is really doing one of two jobs. It is
either **ranking** stocks against each other (which will beat which next month) or printing a
**single number** for one stock (a score, a fair value, a confidence level). None of them
predict the future. They find statistical regularities in past data and bet the regularity
repeats. And they all share the same five enemies, defined once in the next section so I can
name them throughout: overfitting, data snooping, look-ahead bias, regime change, and
crowding.

Assessment: this is an explainer, not investment advice. The individual mechanics are drawn
from the vendors' own documentation and the peer-reviewed finance literature (cited at the
bottom, each marked peer-reviewed vs. vendor). The judgments about how well any of it works
in real money are labeled Assessment or Speculation, because that is exactly what they are.

**This note is the "how it works" part of a four-note wing.** The others answer the rest of the
question:

- **[The AI stock-pick landscape](the-landscape)** — what the named services (InvestingPro,
  Danelfin, Zacks, Trade Ideas, Tickeron, and the rest) actually sell, and the marketing tells to
  distrust.
- **[Does any of it actually beat the market?](does-it-actually-work)** — the peer-reviewed base
  rates (SPIVA, skill-versus-luck, the machine-learning evidence, and what erodes it).
- **[How to emulate it without fooling yourself](build-it-yourself)** — the honest backtesting
  method and the red-flag checklist for separating wheat from chaff.
- **[Getting started: a 12-month factor screener](getting-started-screener)** — a concrete
  where-to-begin roadmap for the low-turnover, buy-and-hold version that the evidence treats most
  kindly.

## The five traps (read this first)

These five failure modes show up in every method, so here they are once, in plain English.

**Overfitting** is when a model learns the noise in the past instead of a real pattern. Stock
returns are mostly randomness with a tiny thread of signal, so a flexible model will happily
"explain" last year's luck and then fail on next year's.

**Data snooping** (also called the multiple-testing or "factor zoo" problem) is what happens
when you try thousands of ideas and keep the ones that looked best. Some will look brilliant
by pure chance. If you test 1,000 useless rules, about 50 will clear a "1-in-20" significance
bar for no reason at all. Keeping the winner without accounting for how many you tried is the
single most common way quant research lies to itself.

**Look-ahead bias** is accidentally using information the model would not actually have had at
the time. The classic version: a company's earnings get restated or reported weeks late, but
the backtest lines the number up with the earlier date, so the model effectively "knew" the
result before the market did. Backtest looks great; live trading fails.

**Regime change** (non-stationarity) is when the world changes and the old pattern stops
working. Interest-rate eras, 2008, COVID, the long 2010s stretch when "value" stocks
underperformed: a model trained on one regime can quietly rot in the next.

**Crowding** (alpha decay) is when a signal works, gets discovered and traded by everyone, and
the edge shrinks or flips. Publishing or selling a signal partly destroys it, because the act
of everyone buying the cheap stocks makes them no longer cheap.

Keep these five names in mind. The rest of the note is largely about which trap bites which
method hardest.

## 1. Multi-factor scoring (value, momentum, quality, size, low-volatility)

**The plain idea:** rate every stock on a handful of proven characteristics, turn each rating
into a comparable score, add the scores up, and buy the top-ranked names. This is the oldest
and best-understood branch of quant investing, and it rests on decades of published research.

A "factor" is a stock characteristic that has historically earned an extra return beyond just
being in the market. The academic canon defines a handful:

- **Value:** cheap stocks relative to their fundamentals. Measured as book-to-market (the
  company's accounting net worth divided by its market price) or a yield like earnings-to-price.
- **Size:** small companies. Measured as market capitalization (share price times shares
  outstanding). Small has historically beaten large, on average and with caveats.
- **Momentum:** recent winners. Measured as the past 12-month return *skipping the most recent
  month* (the last month tends to reverse, so it is dropped). This "12-minus-1" definition is
  standard.
- **Quality:** profitable, stable, growing companies with clean balance sheets. Built from
  profitability, growth, safety (low debt, low bankruptcy risk), and shareholder payout.
- **Low-volatility / low-beta:** calmer stocks. Measured by past volatility or past beta
  (sensitivity to the market). Counterintuitively, boring low-risk stocks have delivered
  better risk-adjusted returns than the theory says they should.

FACT: these are grounded in a well-known literature. The Fama-French three-factor model (1993)
established market, size, and value; their five-factor model (2015) added profitability and
investment; Carhart (1997) added momentum as a fourth factor, building on Jegadeesh and
Titman's 1993 momentum discovery; Asness, Frazzini, and Pedersen defined the quality factor
("Quality Minus Junk," published 2019); and Frazzini and Pedersen's "Betting Against Beta"
(2014) plus Ang, Hodrick, Xing, and Zhang (2006) established the low-risk anomaly. All are
peer-reviewed and cited below.

### How a composite score is built

The mechanical recipe is the same everywhere, and it is worth knowing because it demystifies
the whole thing:

1. **Compute a raw signal per stock** for each factor (book-to-market for value, 12-minus-1
   return for momentum, and so on). For "lower is better" signals like volatility, flip the
   sign so that higher always means better.
2. **Clip the outliers** (winsorize): cap extreme values at, say, the 1st and 99th percentiles
   so a single data error or tiny stock cannot dominate.
3. **Z-score each factor across all stocks** on each date: subtract the average and divide by
   the standard deviation, so every factor becomes a comparable "how many standard deviations
   above or below average" number. This is often done within an industry, so you compare tech
   to tech, not tech to utilities.
4. **Weight and add** the z-scores into one composite. Equal weighting is the honest default;
   fancier shops tune the weights, which quietly invites data snooping.
5. **Rank and sort.** Buy the top decile (top 10%), often short the bottom decile, and
   rebalance monthly or quarterly.

That is the entire machine. What comes out is a single ranked list. There is no crystal ball,
just five old characteristics blended into one number.

### The Zacks Rank as a worked example

The Zacks Rank is a well-known commercial version, and it is instructive because it runs on a
*single* engine, not the five factors above. FACT (per Zacks' own methodology): the Zacks Rank
is a 1-to-5 rating (1 = Strong Buy, 5 = Strong Sell), recomputed nightly, driven almost
entirely by **changes in analysts' earnings estimates**. When Wall Street analysts raise their
earnings-per-share (EPS) forecasts for a stock, its rank improves; when they cut, it worsens.
Zacks combines four inputs: agreement (are analysts revising in the same direction), magnitude
(how big the revision is), upside (Zacks' own "most accurate" estimate versus consensus), and
surprise (the recent pattern of beating or missing earnings). Assessment: it is really a
momentum-on-estimates signal, and Zacks itself positions it as a short-horizon tool (one to
three months), not a buy-and-hold rating.

### Where multi-factor scoring misleads

The strength is that these factors are real, published, and have worked across decades and
countries. The failure modes are specific:

FACT: signals decay after publication. McLean and Pontiff (2016) studied 97 published
predictors and found their returns were about 26% lower out-of-sample and about 58% lower
*after publication*. That is **crowding** measured directly: once a signal is known, arbitrage
eats roughly a third of it.

FACT: the "factor zoo" is a **data-snooping** warning. Harvey, Liu, and Zhu (2016) catalogued
316 published factors and argued that with so much searching, the usual "t-statistic above 2"
bar is far too easy; they propose a hurdle nearer 3.0. Assessment: most published factors are
probably false positives, so the fact that a service uses "40 factors" is not reassuring.

FACT of **regime change**: the value factor endured its deepest, longest drawdown on record
from roughly 2007 to 2020 before rebounding. A real, respected factor can go missing for over
a decade. Assessment: this is the honest reason a clean backtest overstates what a live
multi-factor model actually delivers.

## 2. "Fair value" and intrinsic-value estimates (InvestingPro, Morningstar)

**The plain idea:** instead of ranking stocks, compute one dollar number for what a share is
"really" worth, then compare it to the market price to call the stock cheap or expensive. This
is the automation of old-fashioned fundamental valuation.

### The valuation methods being automated

Under the hood, every fair-value engine is running some mix of four classic methods:

**Discounted cash flow (DCF)** is the main one. Estimate the cash a business will throw off for
the next several years, then "discount" each future year back to today (a dollar next decade is
worth less than a dollar now). The key ingredients are the projected cash flows, the discount
rate (usually the weighted average cost of capital, or WACC, the blended return debt and equity
holders demand), and a **terminal value** for everything past the forecast window. Sum the
discounted pieces and you get a value.

**Multiples (relative valuation)** value a company by what similar companies sell for: apply a
price-to-earnings (P/E), enterprise-value-to-EBITDA (EV/EBITDA, where EBITDA is earnings before
interest, taxes, depreciation, and amortization), or price-to-book ratio from comparable firms.

**Dividend discount model** values a stable dividend-payer as next year's dividend divided by
(required return minus dividend growth). It is a special case of DCF.

**Analyst price targets** are the consensus of Wall Street's 12-month targets, used either as a
sanity check or as an input assumption.

FACT: Aswath Damodaran of NYU Stern, the standard academic-practitioner reference on
valuation, is blunt that a DCF is only as good as its handful of judgment-call inputs ("garbage
in, garbage out") and that a single point-estimate hides enormous uncertainty.

### How InvestingPro and Morningstar actually compute it

FACT (per InvestingPro's own materials and independent reviews): InvestingPro's "Fair Value"
runs a stock through a bank of roughly 15 to 17 separate valuation models at once (several DCF
variants, multiples, dividend discount, and others) and reports the **average** of those
outputs, trimming outliers, then shows the gap to the current price as a percentage. It also
displays the analyst consensus target *separately*, as a parallel signal, not baked into the
Fair Value number. Assessment / correction worth flagging: it is widely repeated that
InvestingPro weights the models by each model's historical accuracy for that specific stock. I
could not verify that in any vendor or third-party source; every accessible description says a
plain mean with outlier trimming. Treat the "accuracy-weighted" claim as unverified.

FACT (per Morningstar's methodology): Morningstar takes the opposite approach: one analyst-built
three-stage DCF per company, where the company's "economic moat" (durable competitive
advantage) sets how many years it is assumed to earn high returns (wide moat ~20 years, narrow
~10, none ~little). Crucially, Morningstar wraps the number in an **Uncertainty Rating** and
only flags a stock cheap once the price is a set margin below fair value (20% for low
uncertainty, 40%+ for high). Assessment: that margin-of-safety band is Morningstar explicitly
refusing to pretend the single number is precise, which is the honest way to present a fragile
estimate.

### Where fair value misleads

The strength is transparency of logic: you can see the assumptions. The fragility is that the
answer is hostage to those assumptions.

FACT: DCF is violently sensitive to two inputs. Because terminal value uses a formula with
(discount rate minus growth rate) in the denominator, nudging the terminal growth rate or the
discount rate by half a percentage point can swing the estimate by tens of percent. And the
terminal value, the most speculative part, is usually the majority of the total. Damodaran's
discipline: terminal growth can never exceed the growth rate of the whole economy, or the
company eventually becomes the economy.

Assessment: multiples are not an independent anchor. If the whole peer group is euphoric, a
"cheap relative to peers" stock is just cheap relative to a bubble. Multiples re-price the
stock in the market's current mood.

FACT: analyst targets run optimistic and herd. Studies across many countries find consensus
price targets are systematically too high and are hit less than half the time within the year.
Assessment: feeding them into a fair-value engine imports that optimism.

The subtle **look-ahead / circularity** trap here is reverse-DCF: because you can run a DCF
backward from today's price to find the growth the market implies, an engine seeded with
consensus inputs can quietly re-derive the consensus and call it independent "fair value."
Damodaran calls DCF-as-sales-pitch exactly this. Assessment: a single clean fair-value number
implies a precision that four fragile assumptions cannot support.

## 3. Machine-learning return models (trees, forests, neural nets)

**The plain idea:** instead of a human choosing five factors, let an algorithm learn, from
hundreds of characteristics, which combinations have predicted next month's relative returns,
including nonlinear and interacting patterns a human would miss. This is the branch people mean
by "AI stock-picking" in the strict sense.

### What goes in and what comes out

Each stock, each month, is described by a row of numbers: fundamentals (valuation ratios,
profitability, growth), technicals (past returns over various windows, volume, volatility), and
increasingly alternative data (sentiment, web traffic, satellite or card data). The **target**
the model learns to predict is next period's return, and what actually matters is the
cross-section: which stocks will beat which others. So it is fundamentally a **ranking**
problem. Sort by predicted return, buy the top, short the bottom.

Three model families dominate:

**Random forests** grow many decision trees, each on a random resample of the data and a random
subset of the features, then average them. Every tree overfits differently, so averaging cancels
the individual mistakes. This reduces variance and is robust to noise.

**Gradient-boosted trees** (the XGBoost and LightGBM family) build shallow trees one after
another, each new tree trained to fix the errors the previous ones left behind. Slowed with a
small "learning rate," this bends flexibly to the data, and overfits if you add too many trees.

**Neural networks** stack layers of simple units, each passing a weighted sum through a
nonlinear function, which lets them approximate complicated interacting relationships. They
learn by feeding data forward, measuring the error, and using backpropagation and gradient
descent to nudge every internal weight downhill, with regularization, dropout, and early
stopping to keep them from memorizing noise.

### The canonical study

FACT: the reference paper is Gu, Kelly, and Xiu, "Empirical Asset Pricing via Machine Learning"
(Review of Financial Studies, 2020). They ran roughly 30,000 US stocks from 1957 to 2016
through about 94 firm characteristics (interacted with macro variables and industry dummies,
producing over 900 signals), and held a horse race between linear regression, penalized linear
models, random forests, gradient-boosted trees, and neural networks under one clean
out-of-sample design.

Their findings are the honest calibration for this whole field. FACT: trees and neural nets
won, because they captured nonlinear interactions linear models miss; the most important
predictors were price trends (momentum, short-term reversal) and liquidity, and that ranking
was stable across methods. FACT: the out-of-sample predictive power was tiny but real, a
monthly R-squared (the share of variation explained) around 0.3 to 0.4% at the individual-stock
level. Assessment: "tiny but real" is the point. The signal in monthly returns is mostly noise,
and the money is made by applying a small edge across thousands of names. The paper reports a
value-weighted long-short strategy from the neural-net forecasts earning an annualized Sharpe
ratio (return per unit of risk) around 1.35; note that equal-weighted versions score higher but
lean on tiny illiquid stocks you cannot actually trade at scale, which is itself the
tradability lesson.

Important framing, per the authors: they treat machine learning as a **measurement** tool for
expected returns, not a black-box money machine.

### Where machine learning misleads

This family is the most powerful and the most dangerous, because flexible models are
exceptionally good at fitting noise.

**Overfitting and data snooping** are the headline risks. FACT: Bailey, Borwein, López de
Prado, and Zhu ("Pseudo-Mathematics and Financial Charlatanism," Notices of the American
Mathematical Society, 2014) show that trying just a handful of strategy configurations makes a
great-looking backtest almost trivial to produce by luck, and that a backtest that does not
report *how many* variants were tried cannot be trusted. Their companion work formalizes the
"probability of backtest overfitting."

FACT of **look-ahead bias**: fundamentals get restated and reported late, so a model must use
point-in-time data (what was actually public on each date) or it "knows" earnings before the
market did. A related trap is feature leakage, where a feature accidentally encodes the answer,
for example by normalizing over the full sample including the test period.

Assessment of **regime change**: returns are non-stationary. A model trained on one era can
silently degrade in the next, which is why serious work uses walk-forward validation (train on
the past, test on the strictly later future, roll forward) with "purging" of overlapping periods
rather than ordinary random cross-validation.

Assessment of **crowding**: once a machine-learned signal is known and traded, its edge decays,
which is precisely why the honest, tradable Sharpe is the modest value-weighted one, not the
eye-catching microcap number.

## 4. NLP and sentiment analysis (news, filings, calls, social media)

**The plain idea:** a lot of market-moving information arrives as words, not numbers, so read
the words automatically, score whether they are upbeat or downbeat about a company, and trade
the score. NLP means natural language processing, the automatic reading of text.

### What goes in and what comes out

The inputs are four kinds of text: news and newswires (fast, reactive), regulatory filings (the
annual 10-K and quarterly 10-Q that US companies must file), earnings-call transcripts (and
sometimes the audio, for vocal tone), and social media (Twitter/X, StockTwits, Reddit including
WallStreetBets).

There are two ways to score them. The old way is a **dictionary/lexicon** method: keep a list of
positive and negative words, count them, and produce a polarity score. The whole trick is the
list. FACT: Loughran and McDonald (2011) is the reason a finance-specific list exists. They
showed that a generic psychology word list badly misreads finance, because words like
"liability," "tax," "cost," and "depreciation" are flagged negative in everyday English but are
neutral, routine terms in a filing. Roughly three-quarters of the words a standard list flagged
as negative are not negative in a financial document. Their finance-tuned dictionary (from
Notre Dame) became the standard.

The new way is **machine learning / transformers**: show a model many example sentences already
labeled positive, negative, or neutral, and let it learn context and negation. FinBERT (Araci,
2019) is the well-known example, a version of Google's BERT further trained on financial text.
Modern large language models are used the same way. These handle "not a bad quarter" and
context far better than word counting, at the cost of being a black box.

What comes out is a sentiment score per document, aggregated to something useful per stock per
day: net positivity, the *change* in tone versus last quarter (often more informative than the
level), and disagreement/dispersion. That is then either a signal directly or one feature among
hundreds in a larger model. FACT: commercial vendors sell this pipeline ready-made: RavenPack
(rebranded Bigdata.com in 2024), LSEG/Refinitiv MarketPsych, and Bloomberg's news sentiment
feeds all ship scored text to quant funds.

### The honest academic result

FACT: Tetlock (2007) is the landmark, and it is instructive in the sober direction. He found
that high pessimism in a Wall Street Journal column predicted *temporary* downward price
pressure that then reverted to fundamentals. Assessment: a lot of the measurable sentiment
effect is short-lived pressure, not durable, tradable edge.

### Where sentiment misleads

The strengths are speed and breadth (a machine reads every filing in seconds). The failure
modes are sharp:

FACT of the core problem: sarcasm, negation, and finance-specific meaning break naive scoring;
Loughran-McDonald exists precisely because generic dictionaries misread finance, and even
transformer models reduce rather than eliminate this.

Assessment of **crowding**: sentiment is now commoditized. A signal that earned money as a
hard-to-get filing reading in 2011 is a cheap API call today, so the edge has thinned as
everyone trades the same feed.

Assessment of **manipulation**: social feeds are the easiest input to poison. Bots, coordinated
posting, and paid promoters can manufacture positive "sentiment" to pump a thin stock, and the
2021 meme-stock episode is the loud example of coordinated posting a naive reader would chase
into a trap.

Assessment of **look-ahead and reflexivity**: the signal is only real if you knew it before you
could trade on it, so aligning news to the moment it was truly public (not when it was written,
and not confusing a company's self-serving press release with independent reporting) is
essential. And sentiment often *reacts* to price rather than predicting it: after a stock jumps,
coverage turns positive, so a careless model mistakes an echo for a forecast.

## 5. Technical-pattern recognition (Trade Ideas "Holly," Tickeron)

**The plain idea:** scan price and volume charts for shapes and rules that have paid off before
(breakouts, head-and-shoulders, moving-average crossovers), keep the ones that backtested best,
and fire real-time buy/sell alerts with a confidence level and a target price. This is the
automation of chart-reading, and it is the branch most prone to the data-snooping trap.

### What goes in and what comes out

The inputs are the price/volume history (OHLCV: open, high, low, close, volume), derived
technical indicators (moving averages; RSI, the relative strength index; MACD, moving-average
convergence-divergence; Bollinger bands), and chart-pattern templates. Pattern matching works
either by smoothing the price line and checking whether the bumps match a pattern's shape, or by
a machine-learning classifier trained on labeled examples. The output is real-time alerts with
entry, exit, and stop levels plus win-rate and confidence statistics.

### How Holly and Tickeron actually work

FACT (per Trade Ideas' own Holly guide): "Holly" is not a neural net that reads charts. She is
an automated backtest-and-select engine wrapped around a fixed library of pre-built rule
strategies (breakouts, VWAP bounces, gap plays, and so on). Every night after the close, Holly
backtests roughly 60-plus strategies against recent data, optimizes each one's parameters, then
**selects only the strategies with the highest statistical chance of profit for the next
session**. During the day she scans in real time and fires an alert when a live stock matches a
selected strategy. There are several Hollys (the original "Grail," a more aggressive 2.0, and
the newer "Neo"). Assessment: the design is literally "keep whatever backtested best last
night," which is the exact setup that manufactures false winners (see below).

FACT (per Tickeron's own pages): Tickeron scans thousands of stocks for classic chart patterns
(head-and-shoulders, triangles, wedges, flags, cup-and-handle) and candlesticks, and for each
hit reports a confidence level, a breakout price, a target price, and backtested success
statistics. Its "AI Robots" package those signals into semi-automated trading agents with
entries, exits, and stated odds.

### The academic grounding, stated narrowly

FACT: the canonical academic paper is Lo, Mamaysky, and Wang, "Foundations of Technical
Analysis" (Journal of Finance, 2000). They used kernel regression to define classic chart
patterns mathematically so a computer could detect them objectively, then tested US stocks from
1962 to 1996. Their actual finding, quoted narrowly: several technical patterns "provide
incremental information and may have some practical value." Assessment: read that carefully.
"Incremental statistical information" is a weak result, strongest in liquid stocks, and the
authors are explicit it does **not** establish that chart-reading is profitable after trading
costs. It is not a green light.

### Where pattern recognition misleads

This is the branch where **data snooping** is not a side risk, it is the central design flaw.

FACT: Sullivan, Timmermann, and White (1999) took an earlier promising study (Brock, Lakonishok,
and LeBaron, 1992, which found simple moving-average rules seemed to work), expanded it to a
universe of about 7,800 trading rules, and applied White's "Reality Check" bootstrap to correct
for how many rules were searched. Once you account for the full search, the apparent superiority
of the best rule largely disappears. Assessment: this is the direct verdict on Holly and
Tickeron's approach. Picking the best backtested strategy out of dozens every night, or
surfacing the highest-confidence pattern from a giant scan, is exactly the setup that produces
lucky winners that do not repeat, unless the confidence is discounted for the number of rules
tried (which vendors do not disclose).

FACT of **overfitting**: Bailey and colleagues (2014) prove that the more configurations you
try, the higher the odds your backtest is overfit, and that overfit strategies can post
*negative* real-world returns, not merely zero.

Assessment of the practical erosion: **look-ahead and survivorship bias** (backtesting only on
stocks that still exist, quietly dropping the failures), plus transaction costs and slippage,
eat the thin edges these methods find. A rule that looks profitable on paper can be a net loser
after spreads and fees. And advertised "win rates over 60%" are typically gross, in-sample, and
cherry-picked; a high win rate is fully compatible with losing money (many small wins, a few
large losses).

## How much to trust any of this

The mechanics above are real and, in the peer-reviewed cases, honestly established. FACT: factor
premiums, a small machine-learning edge, and modest information in some chart patterns all show
up in serious published research. The mechanics are not snake oil.

The gap the operator should hold onto is between **capability** and **delivered**. Assessment: a
clean backtest is close to the best case, and four forces stand between it and a live dollar of
profit. Signals decay once published and crowded (McLean-Pontiff measured roughly a third to a
half of the return gone). Data snooping means many advertised signals were never real (the
factor zoo, the Reality Check). Regime change can switch off a genuine premium for a decade
(value, 2007 to 2020). And costs, taxes, and slippage eat thin edges, which is why the
academically honest strategies use large, liquid, value-weighted portfolios rather than the
microcap-driven numbers that look best in marketing.

Assessment: this maps cleanly onto who is running it. The institutional versions (a well-built
multi-factor sleeve, a Gu-Kelly-Xiu-style model across thousands of names, with point-in-time
data, walk-forward testing, and real cost modeling) can extract a small, real, diversified edge.
The retail-facing versions (a "fair value" number that hides four fragile assumptions in one
tidy figure, a nightly-reselected chart robot with an undisclosed search count, a sentiment
score everyone else already has) are the ones most exposed to exactly the five traps, and their
advertised win rates are almost never audited net-of-cost live returns.

Speculation, stated as such: the durable edge in this whole field is less "which model" and more
discipline about the traps: point-in-time data, out-of-sample and walk-forward testing, honest
accounting for how many things you tried, real transaction costs, and humility about regime
change. A tool that is transparent about those (Morningstar's uncertainty bands are a small good
example) deserves more trust than one that hands you a single confident number.

## See also

- **[Building your own basketball spread model](topics/games/basketball-ats-model/)** — the same
  machinery (features, walk-forward validation, calibration, and the honest math on why the
  market usually wins) applied to sports betting instead of stocks.
- **[AI agent engineering](topics/ai-engineering/)** — how the machine-learning and language-model
  building blocks referenced here actually work.

## Sources

Each source is marked **[peer-reviewed]**, **[vendor/blog]**, **[practitioner]**,
**[working paper]**, **[preprint]**, or **[book]**. Where a peer-reviewed paper is paywalled, a
free working-paper or author copy is listed alongside.

### Multi-factor scoring

- Fama & French, "Common risk factors in the returns on stocks and bonds," *Journal of Financial
  Economics* (1993) — **[peer-reviewed]** — https://doi.org/10.1016/0304-405X(93)90023-5
- Fama & French, "A five-factor asset pricing model," *Journal of Financial Economics* (2015) —
  **[peer-reviewed]** — https://doi.org/10.1016/j.jfineco.2014.10.010 (free PDF:
  https://tevgeniou.github.io/EquityRiskFactors/bibliography/FiveFactor.pdf)
- Carhart, "On Persistence in Mutual Fund Performance," *Journal of Finance* (1997), momentum
  4-factor — **[peer-reviewed]** — https://onlinelibrary.wiley.com/doi/10.1111/j.1540-6261.1997.tb03808.x
- Jegadeesh & Titman, "Returns to Buying Winners and Selling Losers," *Journal of Finance*
  (1993) — **[peer-reviewed]** — https://onlinelibrary.wiley.com/doi/abs/10.1111/j.1540-6261.1993.tb04702.x
  (free PDF: https://www.bauer.uh.edu/rsusmel/phd/jegadeesh-titman93.pdf)
- Asness, Frazzini & Pedersen, "Quality Minus Junk," *Review of Accounting Studies* (2019) —
  **[peer-reviewed]** — https://doi.org/10.1007/s11142-018-9470-2 (free full text:
  https://www.aqr.com/Insights/Research/Working-Paper/Quality-Minus-Junk)
- Frazzini & Pedersen, "Betting Against Beta," *Journal of Financial Economics* (2014) —
  **[peer-reviewed]** — https://doi.org/10.1016/j.jfineco.2013.10.005 (author PDF:
  https://pages.stern.nyu.edu/~lpederse/papers/BettingAgainstBeta.pdf)
- Ang, Hodrick, Xing & Zhang, "The Cross-Section of Volatility and Expected Returns," *Journal of
  Finance* (2006) — **[peer-reviewed]** — https://onlinelibrary.wiley.com/doi/10.1111/j.1540-6261.2006.00836.x
- McLean & Pontiff, "Does Academic Research Destroy Stock Return Predictability?," *Journal of
  Finance* (2016), factor decay — **[peer-reviewed]** — https://onlinelibrary.wiley.com/doi/abs/10.1111/jofi.12365
- Harvey, Liu & Zhu, "... and the Cross-Section of Expected Returns," *Review of Financial
  Studies* (2016), the factor zoo — **[peer-reviewed]** — https://academic.oup.com/rfs/article-abstract/29/1/5/1843824
- Zacks Rank methodology — **[vendor/blog]** — https://www.zacks.com/education/rank-guide/zacks-rank-guide

### Fair value / intrinsic valuation

- InvestingPro, "Fair Value in Investing" (Investing.com Academy) — **[vendor/blog]** —
  https://www.investing.com/academy/analysis/fair-value-investing-stocks/
- Morningstar Equity Research Methodology (three-stage DCF, moat, uncertainty rating) —
  **[vendor/blog]** — https://www.morningstar.com/content/dam/marketing/shared/research/methodology/705988Morningstar_Equity_Research_Methodology.pdf
- Morningstar, "An Introduction to the Morningstar Uncertainty Rating" — **[vendor/blog]** —
  https://www.morningstar.com/stocks/an-introduction-morningstar-uncertainty-rating
- Damodaran, "Ten Myths About Discounted Cash Flow Valuation" (NYU Stern) — **[practitioner]** —
  https://pages.stern.nyu.edu/~adamodar/pdfiles/country/DCFmythsTemasek.pdf
- Damodaran, "DCF Myth 3: too much uncertainty" (Musings on Markets) — **[practitioner]** —
  https://aswathdamodaran.blogspot.com/2016/05/dcf-myth-3-you-cannot-do-valuation-when.html
- "A multi-dimensional assessment of the accuracy of analyst target prices," *International Review
  of Financial Analysis* — **[peer-reviewed]** — https://www.sciencedirect.com/science/article/abs/pii/S1059056024000960

### Machine-learning return models

- Gu, Kelly & Xiu, "Empirical Asset Pricing via Machine Learning," *Review of Financial Studies*
  (2020), the canonical study — **[peer-reviewed]** — https://academic.oup.com/rfs/article/33/5/2223/5758276
  (NBER working paper: https://www.nber.org/papers/w25398 ; author PDF:
  https://dachxiu.chicagobooth.edu/download/ML.pdf)
- Gu, Kelly & Xiu, "Autoencoder Asset Pricing Models," *Journal of Econometrics* (2021) —
  **[peer-reviewed]** — https://www.sciencedirect.com/science/article/abs/pii/S0304407620301998
- Kelly & Xiu, "Financial Machine Learning," *Foundations and Trends in Finance* (2023), survey —
  **[peer-reviewed]** — https://www.nowpublishers.com/article/Details/FIN-064 (open PDF:
  https://bfi.uchicago.edu/wp-content/uploads/2023/07/BFI_WP_2023-100.pdf)
- López de Prado, *Advances in Financial Machine Learning* (Wiley, 2018), methodology —
  **[book]** — https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3104847
- Bailey, Borwein, López de Prado & Zhu, "Pseudo-Mathematics and Financial Charlatanism,"
  *Notices of the AMS* (2014), backtest overfitting — **[peer-reviewed]** —
  https://www.ams.org/notices/201405/rnoti-p458.pdf
- Bailey, Borwein, López de Prado & Zhu, "The Probability of Backtest Overfitting," *Journal of
  Computational Finance* (2017) — **[peer-reviewed]** — https://escholarship.org/uc/item/4w1110bb

### NLP / sentiment

- Loughran & McDonald, "When Is a Liability Not a Liability? Textual Analysis, Dictionaries, and
  10-Ks," *Journal of Finance* (2011) — **[peer-reviewed]** —
  https://papers.ssrn.com/sol3/papers.cfm?abstract_id=1331573
- Loughran-McDonald Master Dictionary & sentiment word lists (Notre Dame SRAF) —
  **[data repository]** — https://sraf.nd.edu/loughranmcdonald-master-dictionary/
- Tetlock, "Giving Content to Investor Sentiment: The Role of Media in the Stock Market,"
  *Journal of Finance* (2007) — **[peer-reviewed]** —
  https://papers.ssrn.com/sol3/papers.cfm?abstract_id=685145
- Mayew & Venkatachalam, "The Power of Voice," *Journal of Finance* (2012), vocal cues —
  **[peer-reviewed]** — https://onlinelibrary.wiley.com/doi/full/10.1111/j.1540-6261.2011.01705.x
- Araci, "FinBERT: Financial Sentiment Analysis with Pre-trained Language Models," arXiv (2019) —
  **[preprint]** — https://arxiv.org/abs/1908.10063
- RavenPack / Bigdata.com news analytics — **[vendor/blog]** —
  https://www.ravenpack.com/products/edge/data/news-analytics
- LSEG/Refinitiv MarketPsych Analytics — **[vendor/blog]** — https://www.marketpsych.com/ma4/intro

### Technical-pattern recognition

- Lo, Mamaysky & Wang, "Foundations of Technical Analysis," *Journal of Finance* (2000), the
  canonical paper — **[peer-reviewed]** — https://onlinelibrary.wiley.com/doi/abs/10.1111/0022-1082.00265
  (free PDF: https://www.cis.upenn.edu/~mkearns/teaching/cis700/lo.pdf)
- Brock, Lakonishok & LeBaron, "Simple Technical Trading Rules...," *Journal of Finance* (1992) —
  **[peer-reviewed]** — https://onlinelibrary.wiley.com/doi/10.1111/j.1540-6261.1992.tb04681.x
- Sullivan, Timmermann & White, "Data-Snooping, Technical Trading Rule Performance, and the
  Bootstrap," *Journal of Finance* (1999) — **[peer-reviewed]** —
  https://onlinelibrary.wiley.com/doi/abs/10.1111/0022-1082.00163
- White, "A Reality Check for Data Snooping," *Econometrica* (2000) — **[peer-reviewed]** —
  https://onlinelibrary.wiley.com/doi/abs/10.1111/1468-0262.00152
- Trade Ideas, "What Holly Does" — **[vendor/blog]** —
  https://www.trade-ideas.com/hollyguide/What_Holly_Does.html
- Tickeron, Real-Time Pattern Scanner — **[vendor/blog]** — https://tickeron.com/stock-pattern-scanner/
