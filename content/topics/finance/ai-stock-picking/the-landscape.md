---
type: note
title: "The AI stock-pick landscape: what these services actually sell"
status: curated
tags: [finance, investing, quant, machine-learning, verification]
created: 2026-07-22
---

# The AI stock-pick landscape: what these services actually sell

This note surveys the consumer services that advertise "AI stock picks" (InvestingPro,
Danelfin, Zacks, Seeking Alpha, Trade Ideas, Tickeron, Kavout, TipRanks, and others) and
separates what each product mechanically does from the marketing wrapped around it. Read it
alongside [how the methods work](topics/finance/ai-stock-picking/) and [whether any of it actually beats
the market](does-it-actually-work).

One honest rule governs everything here: product mechanics and pricing are drawn from vendor
and independent-review pages and drift over time, so verify at purchase; every performance and
"AI works" statement is Assessment, not verified FACT, because almost none of these vendors
publishes an independently audited, live, risk-adjusted track record. Figures in quotes are the
vendor's own framing, repeated so they can be scrutinized, not endorsed.

## The one thing to remember

Assessment: the single most important finding across the whole category is what is *missing*.
Essentially none of these vendors publishes an independently audited, live, risk-adjusted
track record against a cheap index fund. Headline performance is overwhelmingly self-computed,
backtested or hypothetical, equal-weighted, frictionless (zero trading costs assumed),
survivorship-filtered (winners shown, losers quietly retired), and reported as "percent
profitable" or "alpha" rather than net achievable return. Where an independent check of real
money exists (a Zacks managed account audited by CXO Advisory), the advertised edge largely
disappeared and roughly matched the S&P 500.

## Three buckets (and a fourth that is not AI at all)

Assessment: the products sort into three honest categories, plus a fourth that borrows the
label without the substance.

- **Analyst-rating aggregators wearing an AI label.** MarketBeat and much of TipRanks average
  Wall Street analysts' ratings and price targets into a score. Useful as a data convenience,
  but the "AI" is a thin veneer on aggregation.
- **Large-language-model chatbot wrappers.** Magnifi, Public's "Alpha," Incite, and Boosted's
  newer "Alfa" answer questions in plain English over filings and data. They summarize and
  research; they do no predictive modeling and make no real track-record claim.
- **Genuine quantitative or machine-learning modeling.** Danelfin and Kavout run real machine
  learning; Zacks, Seeking Alpha, and TipRanks' Smart Score are honest rules-based quant that
  the market often mislabels "AI." This is where an actual statistical signal might live.
- **Deterministic rule automation.** Composer lets you build and auto-run rule-based strategies.
  It is neither AI nor aggregation; the main risk is you fooling yourself with a curve-fit
  backtest, not the vendor fooling you.

## Quick comparison

The table is a map, not an endorsement. "Real ML?" asks whether a genuine machine-learning
model drives the picks. "Performance basis" is how the headline number is generated.

| Service | What it really is | Real ML? | Headline performance basis | Independently audited? |
|---|---|---|---|---|
| InvestingPro / ProPicks | Quant valuation blend + prebuilt portfolios | Thin | Backtest (to 2013) blended with ~2.5yr "since launch" | No (self-published) |
| Danelfin | ML probability score (1–10) | Yes | Backtest from a fixed 2017 start | No live audit |
| Zacks Rank | Rules-based earnings-revision quant (1–5) | No (not claimed) | Hypothetical, equal-weight, zero-cost, since 1988 | Real-money check ≈ matched S&P |
| Seeking Alpha Quant | Rules-based multi-factor quant (A+–F) | No | "Since 2010" is backtest (predates the product) | Signal has partial academic support |
| Trade Ideas / Holly | Overnight backtest-and-select engine | No ("self-learning" oversells it) | Simulated; winners-only showcase | No |
| Tickeron | Technical pattern recognition + trend filter | "FLM" is invented branding | Backtested "with hindsight" (its own words) | No |
| Kavout (K/Kai Score) | ML equity ranking (1–9), ~1-month horizon | Claimed (method opaque) | Backtest 2012–2025 | No |
| TipRanks Smart Score | Quant composite of analysts + insiders + more | Smart Score = quant; SPARK = LLM | Self-published "percent profitable," backtested | No |
| MarketBeat | Analyst-rating aggregator | Mostly no | None rigorous | Nothing to verify |
| Boosted.ai | Institutional ML platform + LLM analyst | Yes, but B2B not retail | No returns claim; a preference survey | Not applicable |

## The bigger names, briefly

**InvestingPro / ProPicks (Investing.com).** A subscription layer over free Investing.com. "Fair
Value" is a per-stock estimate blending roughly 15 valuation models; "ProPicks AI" is a set of
prebuilt, monthly-refreshed portfolios, the count of which is a paywall lever. Pricing runs
about \$18/month (Pro) to \$45/month (Pro+), sold with a perpetual "50% off" countdown. Marketing
blends a short "since launch (Nov 2023)" live figure with backtested-to-2013 numbers ("Mid-Cap
Movers +571% since backtesting began"). Assessment: the sharpest conflict of interest in the
category, because Investing.com runs hype about its own paid product ("we let AI pick stocks and
it's up 111%") as editorial news. Only winning strategies are showcased.

**Danelfin.** The most genuinely machine-learning consumer product here. A daily 1–10 "AI Score"
is the modeled probability that a stock beats the market over about three months, with an
explainable-AI panel showing the drivers. Assessment: notably honest framing for the category,
it describes itself as "a statistical edge that works across a basket of stocks over time" that
"won't guarantee the performance of any individual stock." The marquee numbers ("+376% since
2017 vs S&P +166%," "70% win rate") are still in-sample backtests from a chosen start date.

**Zacks Rank.** A 1–5 rating driven entirely by trends in analysts' earnings-estimate revisions,
recomputed nightly, for a one-to-three-month horizon. Zacks honestly calls it "quantitatively
determined," not AI. The famous "~+24%/year since 1988, double the S&P" is, per Zacks' own
disclosure, an equal-weighted hypothetical portfolio assuming zero transaction costs. Assessment:
CXO Advisory found a representative Zacks real-money managed account returned about 6.5% a year
from 1995 to 2009, essentially identical to the S&P. The estimate-revision premise itself has
genuine academic support; the frictionless 36-year compounding does not survive contact with a
real account.

**Seeking Alpha Quant.** A daily, sector-relative letter grade (A+ to F) from five factor grades,
rules-based, not AI. The eye-catching "Strong Buy beat the S&P by nearly 8x since 2010" is a
reconstructed backtest, because the product launched years after 2010. The genuinely live number
belongs to its separate Alpha Picks product (live since July 2022, roughly 76% of picks
profitable in a mostly-bull sample), which is the most trustworthy figure in either Seeking
Alpha's or Zacks' stable and still only about four years long.

**Trade Ideas "Holly."** An overnight engine that backtests dozens of pre-built strategies and
streams the next day's signals from whichever recently tested best. Marketed as "self-learning
AI," which oversells nightly automated backtesting. Its records page shows individual winning
trades with no losers and no drawdown; independent reviewers estimate real-world returns around
14–20% after costs. AI features are gated to the roughly \$2,000/year tier.

**Tickeron.** Technical pattern-recognition bots branded with "Financial Learning Models," an
invented term with no industry meaning. Per-robot headlines run to "171% in 30 days" and a
"10,070% return," produced over hand-picked 7-to-64-day windows and annualized, with losing
robots retired. Its own disclaimer concedes the numbers are "achieved by the retroactive
application of a backtested model designed with the benefit of hindsight" and adjusted "until
desired or better performance results are achieved," which is an admission of curve-fitting.
Assessment: the highest-risk marketing profile in this review.

**Kavout, TipRanks, MarketBeat, Boosted.ai.** Kavout ranks stocks 1–9 with opaque "deep learning
and reinforcement learning" and self-published backtests; an independent test found high scores
beat low ones only modestly and the score lagged a major news event by a day. TipRanks' Smart
Score is legitimate rules-based quant (analyst ratings weighted by each analyst's measured
accuracy) marketed loosely as AI, reported as "percent profitable," a low bar in a bull market.
MarketBeat is fundamentally an analyst-rating aggregator with a thin AI "Idea Engine" and no
verifiable performance claim. Boosted.ai is an institutional (business-to-business) platform, not
a retail product, and mostly avoids falsifiable return claims.

## The cross-cutting red flags

Assessment: the same handful of tells recur across the category. They are worth memorizing.

1. **"AI" is doing marketing work, not technical work** in several products; genuine machine
   learning drives only a few (Danelfin, Kavout, institutional Boosted), while honest rules-based
   quant (Zacks, Seeking Alpha, TipRanks Smart Score) gets mislabeled "AI."
2. **The vendor is judge, jury, and beneficiary.** Nearly every performance number is computed by
   the company selling the subscription. Investing.com publishing hype about its own product as
   "news" is the clearest case.
3. **Backtest dressed as a track record.** The since-1988, since-2010, since-2017 figures all lead
   with backtests; the few honest live records that exist are short and fall in a mostly-bull
   window.
4. **Frictionless, equal-weighted, high-turnover math.** Zacks explicitly excludes commissions,
   spreads, and price impact and states the returns "are not achievable with actual portfolios."
5. **Survivorship in the storefront.** Winners-only showcases and per-robot stats with losers
   retired.
6. **Annualizing tiny windows** turns a good 30-day run into a "360% annualized" headline.
7. **The affiliate review layer.** Many "independent reviews" carry affiliate relationships, so
   even skeptical-looking write-ups tilt toward the sign-up link. Vendor methodology and
   disclaimer pages were weighted most heavily for the critical claims here.

## How much to trust this

Assessment: treat the product-and-pricing details as a snapshot that drifts, and treat every
performance claim as unaudited until proven otherwise. The useful move is not to rank these
services against each other but to ask each the same five questions from the
[build-it-yourself note](build-it-yourself): show me the live, third-party-audited, out-of-sample,
net-of-cost track record; the Sharpe and maximum drawdown against a benchmark; how many
strategies you tested; and your registration status. The reason so much of this note is Assessment
rather than FACT is that, for almost every service, those answers do not exist.

## See also

- **[How AI and quant stock-picking actually works](topics/finance/ai-stock-picking/)** — the mechanics behind
  the scores, fair values, and robots listed here.
- **[Does any of it actually beat the market?](does-it-actually-work)** — the peer-reviewed base
  rates these performance claims have to clear.
- **[How to emulate it without fooling yourself](build-it-yourself)** — the honest method and the
  red-flag checklist.

## Sources

Vendor and review pages describe drifting product details; treat them as a snapshot. The one
independent real-money check is marked.

- InvestingPro / ProPicks methodology and performance — **[vendor]** — https://www.investing-support.com/hc/en-us/articles/21860692550289-How-are-ProPicks-Strategies-created and https://www.investing-support.com/hc/en-us/articles/21861480414993-ProPicks-performance
- Danelfin, "How it works" and audit page — **[vendor]** — https://danelfin.com/how-it-works and https://audit.danelfin.com/
- Zacks Rank methodology and performance disclosure — **[vendor]** — https://www.zacks.com/stocks/zacks-rank and https://www.zacks.com/performance_disclosure/
- CXO Advisory, "Are Zacks Rankings Exploitable?" (independent real-money check) — **[independent]** — https://www.cxoadvisory.com/fundamental-valuation/are-zacks-rankings-exploitable/
- Seeking Alpha Quant Ratings FAQ — **[vendor]** — https://help.seekingalpha.com/premium/what-are-quant-ratings-and-how-do-i-use-them
- Seeking Alpha Alpha Picks review (live record) — **[review]** — https://stockanalysis.com/article/alpha-picks-review/
- Trade Ideas, Holly records and guide — **[vendor]** — https://www.trade-ideas.com/holly-records/ and https://www.trade-ideas.com/hollyguide/Holly_AI_Strategies.html
- Tickeron AI Robots and disclaimers — **[vendor]** — https://tickeron.com/trading-investing-101/ai-robots-instructions/
- Kavout K Score — **[vendor]** — https://www.kavout.com/k-score/
- TipRanks Smart Score review — **[review]** — https://www.wallstreetzen.com/blog/tipranks-review/
- MarketBeat review — **[review]** — https://www.wallstreetzen.com/blog/marketbeat-review/
- Boosted.ai (institutional context) — **[vendor]** — https://boosted.ai/
