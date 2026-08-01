---
type: note
title: "How to emulate it without fooling yourself (and spot the chaff)"
status: curated
tags: [finance, investing, quant, machine-learning, verification]
created: 2026-07-22
---

# How to emulate it without fooling yourself (and spot the chaff)

Two things are true at once: rigorous quantitative stock selection is a real discipline, and most
"AI stock pick" marketing is built on the exact mistakes that discipline was invented to prevent.
The core problem is the same on both sides. A backtest is trivially easy to make look good and
very hard to make trustworthy. This note is the practical companion to
[how the methods work](topics/finance/ai-stock-picking/) and [whether they beat the market](does-it-actually-work):
Part 1 is the checklist a careful quant uses to avoid fooling themselves, and Part 2 is the same
checklist turned outward to catch someone trying to fool you.

Everything below is Assessment (a synthesis of standard quant practice and the cited literature),
except specific figures or rules attributed to a named source, which are reported at that source's
strength. If you have read the [basketball spread model](topics/games/basketball-ats-model/) notes,
this is the same machinery: the discipline that stops a sports-betting backtest from lying to you
is the discipline that stops a stock backtest from lying to you.

## The one organizing idea

Assessment, following Marcos López de Prado's *Advances in Financial Machine Learning* (2018): a
backtest is not an experiment, it is a sanity check. It cannot prove a strategy works; it can only
fail to reject it. His rule is that every backtest must be reported alongside *every* other trial
that went into producing it, because the number of attempts is what decides whether a good result
is skill or luck. Hold that thought through everything below.

## Part 1: building or simulating a strategy honestly

### Out-of-sample and walk-forward testing

Fit and tune the strategy on one slice of history, then measure it on a slice the model never saw.
Walk-forward testing does this repeatedly and in time order: train on a window, test on the next
untouched window, roll forward, repeat. It respects the arrow of time in a way a random train-test
split does not. The danger is leakage, where information from the test period sneaks into training,
most often by tuning settings against the hold-out until the hold-out is effectively in-sample. In
machine-learning setups with overlapping outcome windows, López de Prado's fixes are purging (drop
training rows whose outcome windows overlap the test set) and embargoing (leave a time gap after
the test set).

### Point-in-time data: look-ahead and survivorship bias

Point-in-time data reconstructs what was actually knowable on each historical date. Two biases
attack it. Look-ahead bias uses information that did not yet exist at decision time, such as
trading on earnings before they were filed, using restated financials instead of as-first-reported
numbers, or picking past stocks from an index's current membership. Survivorship bias builds the
historical universe only from companies that still exist, silently deleting the bankruptcies and
delistings, so the backtest never buys the stocks that went to zero. Both push returns upward, often
sharply: the survivorship effect in mutual-fund returns has been estimated at roughly 0.9 percent a
year (Elton, Gruber, and Blake 1996, reported at source strength), and higher for hedge funds. The
fix is a database that keeps dead securities and their delisting returns, and a universe that
reflects the actual investable set on each date.

### Realistic transaction costs, slippage, and turnover

Gross return is before trading frictions; net return is what lands in the account. The frictions are
commissions, the bid-ask spread (you buy at the ask and sell at the bid), slippage (the price moves
between decision and fill), market impact (your own order pushes the price against you, growing
roughly with the square root of order size relative to volume), short-borrow costs, and taxes.
Turnover is how often you trade, and every unit of turnover pays these costs again. Assessment: a
strategy's gross Sharpe can be excellent and its net Sharpe negative; high-turnover signals (short-
term reversal is the classic) look spectacular gross and often die net. Report net of conservative
cost estimates, with turnover disclosed.

### Multiple-testing correction: the most-skipped step

If you try enough strategies, the best one will look great by chance alone. FACT: Bailey, Borwein,
López de Prado, and Zhu (2014), "Pseudo-Mathematics and Financial Charlatanism" (Notices of the
American Mathematical Society), show the expected maximum in-sample Sharpe from worthless strategies
grows with the number of trials, and that overfit strategies can deliver *negative* out-of-sample
returns, not merely zero. Two purpose-built tools follow. The **Deflated Sharpe Ratio** (Bailey and
López de Prado 2014) shrinks an observed Sharpe for the number of trials, the length of the record,
and non-normal (fat-tailed) returns, and reports the probability the true Sharpe beats zero. The
**Probability of Backtest Overfitting** estimates how often the in-sample-best configuration
underperforms the median out-of-sample. FACT: the asset-pricing analogue is Harvey, Liu, and Zhu
(2016), who argue a genuinely new factor should clear a t-statistic near 3.0, not the usual 2.0,
precisely because so many factors have been mined.

### Factor-exposure attribution: is your "alpha" just known risk premia?

Regress your strategy's excess returns on the returns of known factors (market, size, value,
momentum, and in the five-factor extension profitability and investment) and read the intercept.
The intercept is alpha; the slopes are your factor exposures. Assessment: if the intercept is
statistically indistinguishable from zero, you have no genuine skill, you have merely repackaged
compensated exposure to well-known factors that anyone can buy cheaply through factor exchange-
traded funds. This one regression separates a manager who earns a fee from one selling beta as if
it were alpha. Kenneth French's public data library provides the factor returns to run it.

### Capacity, crowding, and regime change

Capacity is how much money a strategy can run before its own market impact eats the edge. Crowding
is many managers holding the same positions, so a shock forces correlated selling (the August 2007
"quant quake" is the textbook case). Regime change is the plain fact that market relationships are
not stationary; a pattern that held under one interest-rate, volatility, or liquidity environment
can break in the next. Assessment: a strategy that backtests beautifully at ten million dollars can
be uninvestable at one billion, and a backtest that pre-dates crowding and never lived through a
regime it will face overstates what you will get.

### Evaluate risk-adjusted, across regimes

Judge risk-adjusted, not raw. The core metrics are the Sharpe ratio (excess return per unit of
total volatility), the Sortino ratio (per unit of downside deviation only), maximum drawdown and
Calmar (return over max drawdown), and benchmark-relative measures (information ratio and tracking
error against the right index, not cash). Assessment: a high return with a 60 percent drawdown is a
margin call waiting to happen. Sharpe flatters skewed, fat-tailed strategies, which is exactly why
the Deflated Sharpe corrects for non-normality. Above all, evaluate across bull and bear, high and
low rates, calm and stressed volatility; a record measured only during a long bull market tells you
almost nothing about the next bear.

## Part 2: the red flags that separate chaff from wheat

Each red flag maps to a Part 1 safeguard the marketer skipped. That mapping is the fastest way to
judge a pitch.

1. **A gorgeous backtested equity curve sold as "performance."** Labeled "backtested," "simulated,"
   or "model," often starting on a suspiciously good date, for rules chosen after seeing the same
   history. This is backtest overfitting presented as achievement, and the number of configurations
   tried is never disclosed, so you cannot judge it.
2. **No live, audited, out-of-sample track record.** Instead, "hypothetical results" disclaimers.
   FACT: under the SEC Marketing Rule (Rule 206(4)-1, compliance date November 2022), a registered
   adviser may not present hypothetical performance unless it adopts policies letting the audience
   evaluate it, and such material may not be broadcast to a mass-market audience; in September 2023
   the SEC settled charges against nine advisers for advertising hypothetical performance publicly.
   A retail site touting "hypothetical" AI returns is showing you the thing the rule restricts.
3. **Survivorship in the backtest universe.** "Tested on today's top tech stocks" run backward
   guarantees a strong result, because the bankruptcies were deleted before the test began.
4. **"AI" or "deep learning" as an unfalsifiable buzzword.** Heavy jargon with no disclosed method:
   no features, no validation scheme, no baseline, no discussion of overfitting controls. An
   undisclosed method cannot be checked, which is the opposite of science.
5. **Unrealistic returns with no risk numbers.** Big headline returns, no Sharpe, no maximum
   drawdown, no benchmark comparison, over a short or cherry-picked window. Return without risk is
   meaningless, and a strategy that fails to beat a cheap index fund after costs has produced
   nothing worth paying for. Those numbers are omitted because they would puncture the headline.
6. **Testimonials, urgency, and affiliate conflicts over data.** Member "wins," countdown timers,
   "the AI just flashed a rare buy signal," and influencer links. Testimonials are survivorship bias
   as a sales technique; urgency exists to short-circuit due diligence; affiliate promotion pays the
   promoter to enroll you regardless of merit. FACT: the SEC Marketing Rule permits testimonials and
   endorsements only with clear disclosure of whether the speaker is a paid, current client.
7. **"For informational purposes only."** FACT: this fine print is a deliberate liability shield. A
   registered investment adviser files a Form ADV and owes clients a fiduciary duty; many stock-pick
   services instead claim the publisher's exclusion from the definition of "investment adviser"
   (rooted in *Lowe v. SEC*, 1985), which protects impersonal, disinterested commentary of general
   circulation. Assessment: by declaring themselves publishers, these services disclaim any
   fiduciary duty to you. The plain translation: they are not accountable for their picks, they owe
   you nothing, and they are telling you so in the fine print.

## The one-line test

Assessment: for any AI stock-picking pitch, ask for five things: the live, third-party-audited,
out-of-sample, net-of-cost track record; the Sharpe and maximum drawdown against a benchmark; the
number of strategies you tested; and your registration status. A legitimate operation can answer
all five. Marketing built on overfitting cannot answer any of them, which is exactly why the pitch
talks about testimonials and "AI" instead of those numbers.

## See also

- **[Does any of it actually beat the market?](does-it-actually-work)** — the base rates a homemade
  strategy is also up against.
- **[Building your own basketball spread model](topics/games/basketball-ats-model/)** — the same
  walk-forward, cost-aware, honest-backtest discipline applied to sports betting.
- **[Using AI well, without fooling yourself](connections/using-ai-well)** — the vault's
  running thread on the same discipline in other domains.

## Sources

- López de Prado (2018), *Advances in Financial Machine Learning* (Wiley) — **[book]** — https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3104847
- Bailey, Borwein, López de Prado & Zhu (2014), "Pseudo-Mathematics and Financial Charlatanism," *Notices of the AMS* — **[peer-reviewed]** — https://www.ams.org/notices/201405/rnoti-p458.pdf
- Bailey & López de Prado (2014), "The Deflated Sharpe Ratio," *Journal of Portfolio Management* — **[peer-reviewed]** — https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2460551
- Bailey, Borwein, López de Prado & Zhu (2017), "The Probability of Backtest Overfitting," *Journal of Computational Finance* — **[peer-reviewed]** — https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2326253
- Harvey, Liu & Zhu (2016), "...and the Cross-Section of Expected Returns," *Review of Financial Studies* — **[peer-reviewed]** — https://academic.oup.com/rfs/article/29/1/5/1843824
- Kenneth R. French Data Library (factor returns for attribution) — **[data repository]** — https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/data_library.html
- Palomar, "The Seven Sins of Quantitative Investing," *Portfolio Optimization* (open text) — **[practitioner]** — https://bookdown.org/palomar/portfoliooptimizationbook/8.2-seven-sins.html
- SEC, Investment Adviser Marketing Rule (Release IA-5653, 2020) — **[regulatory]** — https://www.sec.gov/files/rules/final/2020/ia-5653.pdf
- SEC Press Release 2023-173, marketing-rule sweep on hypothetical performance — **[regulatory]** — https://www.sec.gov/news/press-release/2023-173
- *Lowe v. SEC*, 472 U.S. 181 (1985), publisher's exclusion — **[legal]** — https://supreme.justia.com/cases/federal/us/472/181/
