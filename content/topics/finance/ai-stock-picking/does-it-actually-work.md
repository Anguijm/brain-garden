---
type: note
title: "Does any of it actually beat the market?"
status: curated
tags: [finance, investing, quant, machine-learning, verification]
created: 2026-07-22
---

# Does any of it actually beat the market?

This note answers the hardest and most important question about AI stock picking: setting aside
what the services claim, what does the rigorous, peer-reviewed evidence say about whether anyone
can reliably beat the market by picking stocks? It is the reality check behind the
[landscape](the-landscape) and the [methods](../ai-stock-picking).

The short version, stated up front as Assessment: over a decade or more, roughly 85 to 90
percent of professional active US stock funds fail to beat their benchmark after fees, almost
none stay at the top by skill rather than luck, and the academic literature finds that after
costs only a fraction of one percent of funds show genuine skill. Machine learning does find a
small, real edge in the data, but it is measured before trading costs, it lives in the stocks
that are hardest to trade, and it shrinks once you subtract costs and account for everyone else
chasing the same signal. Any AI stock-picking service is implicitly claiming membership in a very
thin, very unstable tail. That is the bar.

## The base rate: what active stock-pickers actually achieve

FACT: the S&P Dow Jones Indices SPIVA scorecard measures active funds against the correct
benchmark, net of fees, and corrects for funds that die mid-period (survivorship). Its year-end
2024 US scorecard reports the share of active large-cap US equity funds that **underperformed**
the S&P 500: about 65 percent over one year, about 85 percent over three years, about 76 percent
over five years, about 84 percent over ten years, and about 90 percent over fifteen years. The
failure rate rises with the horizon: over fifteen years, roughly one active large-cap fund in ten
beat a plain index fund.

FACT: the companion S&P Persistence Scorecard tests whether past winners keep winning, which is
the decisive question for any "we can pick winners" claim. In the year-end 2024 report, of the
funds in the top quartile of US equity performance, essentially none (reported at 0.0 percent)
stayed in the top quartile across the following five years. Only a few percent of top-half funds
stayed in the top half over the next four years, which is *worse* than the roughly 6 percent you
would get from coin flips. The one trait that reliably persists is bad performance (high-fee
funds tend to stay bad). S&P's own summary: "Skill is likely to persist, but luck is ephemeral,"
and the data shows almost no persistence.

Assessment: SPIVA is produced by an index provider, so it is an interested party, but it is
transparent and methodologically careful, and it is the authoritative public benchmark. Its
message is not that active management never wins, but that winning is uncommon, inconsistent, and
does not repeat, which is exactly what you would expect if short-run winners are mostly lucky.

## The peer-reviewed verdict on skill versus luck

The academic literature reaches the same place through cleaner methods.

FACT: Fama and French (2010), "Luck versus Skill in the Cross-Section of Mutual Fund Returns"
(Journal of Finance), conclude that the aggregate active fund portfolio is close to the market
before costs, so the high costs of active management "show up intact as lower returns to
investors," and their simulations find "few funds produce benchmark-adjusted expected returns
sufficient to cover their costs." Genuine skill, good or bad, shows up only in the extreme tails.

FACT: Carhart (1997), "On Persistence in Mutual Fund Performance" (Journal of Finance), finds the
apparent persistence of winning funds is explained almost entirely by momentum, expense ratios,
and transaction costs, not manager skill. The only robust persistence is strong underperformance
by the worst, highest-cost funds.

FACT: Barras, Scaillet, and Wermers (2010), "False Discoveries in Mutual Fund Performance"
(Journal of Finance), separate luck from skill and find that, net of costs, about 75 percent of
funds have zero true alpha, about 24 percent are genuinely unskilled (negative alpha), and only
about 0.6 percent are genuinely skilled. The skilled share fell from about 14 percent in 1990 to
0.6 percent by 2006. Assessment: fees convert most of the raw skill that does exist into net
losses for investors. (A later paper, Andrikogiannopoulou and Papakonstantinou 2019, argues the
method understates the true count of non-zero-alpha funds; the direction, that skill is scarce
and costs dominate, is not overturned.)

## What machine learning genuinely can do

The honest picture is not pure nihilism. Machine learning does find real predictability.

FACT: Gu, Kelly, and Xiu (2020), "Empirical Asset Pricing via Machine Learning" (Review of
Financial Studies), ran about 30,000 US stocks from 1957 to 2016 through many methods and found
that trees and neural networks genuinely beat linear models out-of-sample, mainly by capturing
nonlinear interactions among a few dominant signals (momentum, liquidity, volatility). The
monthly out-of-sample predictive power is tiny but real (an R-squared around 0.3 to 0.4 percent),
and a long-short strategy from the neural-net forecasts earned an annualized Sharpe ratio (return
per unit of risk) around 1.35 value-weighted.

The two load-bearing caveats, per the authors and the follow-up literature:

FACT: those returns are gross of trading costs, and the higher equal-weighted Sharpe leans on
small, illiquid stocks. FACT: Avramov, Cheng, and Metzker (2023), "Machine Learning vs. Economic
Restrictions" (Management Science), show the profitability of machine-learning signals is
concentrated in difficult-to-arbitrage stocks (microcaps, distressed, high-volatility) and
deteriorates further under realistic trading costs because of high turnover. Assessment: the edge
is real as *measurement*, but it lives disproportionately in the stocks a real investor can least
afford to trade at scale.

## What erodes the edge before it reaches your account

FACT: McLean and Pontiff (2016), "Does Academic Research Destroy Stock Return Predictability?"
(Journal of Finance), studied 97 published predictors and found their returns were about 26
percent lower out-of-sample and about 58 percent lower after publication. Roughly a third of a
typical signal is competed away once it is known.

FACT: Harvey, Liu, and Zhu (2016), "...and the Cross-Section of Expected Returns" (Review of
Financial Studies), argue that after decades of researchers testing hundreds of factors against
the same data, the usual "t-statistic above 2" significance bar is far too lenient; a genuinely
new factor should clear roughly 3.0. Assessment: many published (and sold) signals are probably
false positives from data mining.

FACT: Novy-Marx and Velikov (2016), "A Taxonomy of Anomalies and Their Trading Costs" (Review of
Financial Studies), find that low-turnover anomalies mostly survive real trading costs while
high-turnover ones generally do not. FACT: Bajgrowicz and Scaillet (2012) find that for technical
trading rules on the Dow, even the in-sample profits are fully offset by low transaction costs.

## The counterpoint: it is not all noise

Fairness requires the other side, because the "it's all data mining" view is itself overstated.

FACT: Chen and Zimmermann (2022), "Open Source Cross-Sectional Asset Pricing" (Critical Finance
Review), reproduced 319 published predictors and found that 98 percent of the clearly significant
ones replicate. FACT: Jensen, Kelly, and Pedersen (2023), "Is There a Replication Crisis in
Finance?" (Journal of Finance), test 153 factors across 93 countries and find most replicate,
cluster into about 13 economically meaningful themes, and work out-of-sample internationally.

Assessment: reconcile the two sides this way. The factor premia are largely real (they replicate)
and the difficulty is net-of-cost implementability and marketing hype, not that the signals are
fake. A real premium that decays after publication, lives in illiquid stocks, and costs more to
harvest than it pays is simultaneously "real research" and "not a product that beats a cheap index
fund for you."

## The direct evidence on stock-picking newsletters

There is no modern peer-reviewed evaluation of quant or "robo" signal newsletters specifically,
which is itself worth noting. The two canonical peer-reviewed studies of newsletter stock
selection both find no skill.

FACT: Metrick (1999), "Performance Evaluation with Transactions Data: The Stock Selections of
Investment Newsletters" (Journal of Finance), studied 153 newsletters and found no significant
evidence of superior stock-picking ability and no short-run persistence. FACT: Jaffe and Mahoney
(1999), "The Performance of Investment Newsletters" (Journal of Financial Economics), found
newsletter-recommended stocks did not outperform appropriate benchmarks as a group, and any
raw-return persistence vanished after risk adjustment. Assessment: these predate today's machine
learning, so they are the best available analogues rather than direct tests, and the absence of
any modern rigorous evaluation of the current products is a gap worth holding against them, not in
their favor.

## Why the paper edge evaporates: the theory

FACT: the efficient market hypothesis (Fama 1970, 1991) holds, in its semi-strong form, that
prices already reflect public information, so analysis of public data should not reliably beat the
market. It is not dogma: real anomalies exist (size, value, momentum), and Fama's own joint-
hypothesis point notes you can never test efficiency alone, only jointly with a model of risk.

FACT: Grossman and Stiglitz (1980) prove markets cannot be perfectly efficient, because if prices
reflected all information no one would be paid to gather it; there must be just enough inefficiency
to compensate information-gathering. Assessment: that is the theoretical opening every active
manager relies on, and it is small, competed over, and cost-eroded, not a free lunch. FACT:
Shleifer and Vishny (1997), "The Limits of Arbitrage" (Journal of Finance), explain why real
mispricings can persist and still be dangerous to trade: arbitrage uses other people's capital,
and when a mispricing widens into a paper loss, investors pull that capital at the worst moment,
forcing liquidation. The anomaly on paper is not the anomaly you can bank.

## What this means for an AI stock-picking service

Assessment: to be worth its fee net of costs, a service has to land its users in a group the
evidence shows is tiny and unstable: the roughly 10 to 15 percent of large-cap managers who beat
the index over ten to fifteen years, the roughly 0.6 percent the literature identifies as
genuinely skilled after costs, and, hardest of all, the roughly zero percent who stay on top from
one period to the next. Past outperformance is close to worthless as a predictor. The honest prior
is strongly against any given service, and the only claim that should move you is a long-horizon,
net-of-all-costs, survivorship-corrected, risk-adjusted live record versus a cheap index fund, not
a backtest and not one good year.

## See also

- **[The AI stock-pick landscape](the-landscape)** — the services whose claims this note weighs.
- **[How AI and quant stock-picking actually works](../ai-stock-picking)** — the mechanics behind
  the edge and its decay.
- **[How to emulate it without fooling yourself](build-it-yourself)** — if you want to test a
  strategy anyway, how to do it honestly.

## Sources

All peer-reviewed unless marked. Several were verified by research agents against publisher, NBER,
or SSRN listings; the SPIVA figures come from S&P Dow Jones Indices (industry, authoritative).

- SPIVA U.S. Scorecard, Year-End 2024 — **[industry, authoritative]** — https://www.spglobal.com/spdji/en/documents/spiva/spiva-us-year-end-2024.pdf
- S&P U.S. Persistence Scorecard, Year-End 2024 — **[industry, authoritative]** — https://www.spglobal.com/spdji/en/documents/spiva/persistence-scorecard-year-end-2024.pdf
- Fama & French (2010), "Luck versus Skill in the Cross-Section of Mutual Fund Returns," *Journal of Finance* — **[peer-reviewed]** — https://onlinelibrary.wiley.com/doi/abs/10.1111/j.1540-6261.2010.01598.x
- Carhart (1997), "On Persistence in Mutual Fund Performance," *Journal of Finance* — **[peer-reviewed]** — https://onlinelibrary.wiley.com/doi/10.1111/j.1540-6261.1997.tb03808.x
- Barras, Scaillet & Wermers (2010), "False Discoveries in Mutual Fund Performance," *Journal of Finance* — **[peer-reviewed]** — https://onlinelibrary.wiley.com/doi/abs/10.1111/j.1540-6261.2009.01527.x
- Gu, Kelly & Xiu (2020), "Empirical Asset Pricing via Machine Learning," *Review of Financial Studies* — **[peer-reviewed]** — https://academic.oup.com/rfs/article/33/5/2223/5758276
- Avramov, Cheng & Metzker (2023), "Machine Learning vs. Economic Restrictions," *Management Science* — **[peer-reviewed]** — https://pubsonline.informs.org/doi/abs/10.1287/mnsc.2022.4449
- McLean & Pontiff (2016), "Does Academic Research Destroy Stock Return Predictability?," *Journal of Finance* — **[peer-reviewed]** — https://onlinelibrary.wiley.com/doi/abs/10.1111/jofi.12365
- Harvey, Liu & Zhu (2016), "...and the Cross-Section of Expected Returns," *Review of Financial Studies* — **[peer-reviewed]** — https://academic.oup.com/rfs/article-abstract/29/1/5/1843824
- Novy-Marx & Velikov (2016), "A Taxonomy of Anomalies and Their Trading Costs," *Review of Financial Studies* — **[peer-reviewed]** — https://academic.oup.com/rfs/article-abstract/29/1/104/1844518
- Bajgrowicz & Scaillet (2012), "Technical Trading Revisited," *Journal of Financial Economics* — **[peer-reviewed]** — https://econpapers.repec.org/article/eeejfinec/v_3a106_3ay_3a2012_3ai_3a3_3ap_3a473-491.htm
- Chen & Zimmermann (2022), "Open Source Cross-Sectional Asset Pricing," *Critical Finance Review* — **[peer-reviewed]** — https://www.nowpublishers.com/article/Details/CFR-0112
- Jensen, Kelly & Pedersen (2023), "Is There a Replication Crisis in Finance?," *Journal of Finance* — **[peer-reviewed]** — https://onlinelibrary.wiley.com/doi/abs/10.1111/jofi.13249
- Metrick (1999), "Performance Evaluation with Transactions Data," *Journal of Finance* — **[peer-reviewed]** — https://onlinelibrary.wiley.com/doi/10.1111/0022-1082.00165
- Jaffe & Mahoney (1999), "The Performance of Investment Newsletters," *Journal of Financial Economics* — **[peer-reviewed]** — https://www.sciencedirect.com/science/article/pii/S0304405X99000239
- Fama (1970, 1991), "Efficient Capital Markets" I and II, *Journal of Finance* — **[peer-reviewed]** — DOI 10.2307/2325486 and 10.1111/j.1540-6261.1991.tb04636.x
- Grossman & Stiglitz (1980), "On the Impossibility of Informationally Efficient Markets," *American Economic Review* — **[peer-reviewed]**
- Shleifer & Vishny (1997), "The Limits of Arbitrage," *Journal of Finance* — **[peer-reviewed]** — https://onlinelibrary.wiley.com/doi/full/10.1111/j.1540-6261.1997.tb03807.x
