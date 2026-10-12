---
type: note
title: "Index funds, and borrowing against a portfolio instead of real estate"
status: curated
tags: [finance, investing, index-funds, margin, leverage, backtest]
created: 2026-10-13
---

# Index funds, and borrowing against a portfolio instead of real estate

Plain English first. Borrowing against a stock portfolio is a real, legal, widely sold loan, and the
scary feeling about it is partly right. A house mortgage leaves you alone as long as you pay. A
portfolio loan doesn't: the lender checks your collateral every day, can shrink how much you're
allowed to borrow without warning, and can sell your shares to pay itself back. Kept small (a loan
of 10% to 20% of the portfolio), it survives even a 2008-sized crash. At 50%, the crash that drops
the market by half wipes you out. The YouTube video that prompted this comes from a salesman with a
Discord to sell. He is honest about the danger in places, but his headline number counts borrowed
money as if it were his.

This is education, not personal financial advice. It was written with an AI, not a licensed advisor.

Labels follow the vault rules: **FACT** (read in a primary or cross-checked source this session),
**Assessment** (reasoning or arithmetic), **Speculation**, **Unverified** (probably right, source not
read here).

## 1. The funds: VTI, VXUS, VT

- **VTI** (Vanguard Total Stock Market ETF, exchange-traded fund) holds the whole US stock market.
  **FACT** (totalrealreturns.com and alphacubator.com, cross-checked against Yahoo adjusted closes,
  fetched 2026-10-13): from 2003-09-26 to 2026-10-09, with dividends reinvested, it returned about
  **11.4% a year**, turning \$10,000 into about \$120,400. Its expense ratio was not checked against
  Vanguard this run (**Unverified**; commonly quoted as 0.03%).
- **VXUS** (Vanguard Total International Stock ETF) is everything outside the US. "VTX" is assumed
  to mean this fund. **FACT** (Motley Fool, Dec 2025; SEC N-PORT filing data via Chartrow, Apr 2026):
  about 8,500 to 8,700 stocks, 0.05% a year.
- **VT** (Vanguard Total World Stock ETF) is roughly VTI plus VXUS weighted by size: about 62% US
  and 37% international, about 9,700 to 10,000 stocks, 0.06% a year (**FACT**, same sources).
- **The drops (FACT, same sources as VTI's return):** VTI fell about **55%** from October 2007 to
  March 2009 and didn't fully recover until March 2012. It fell **35%** in February to March 2020
  and **25%** in 2022. Any leverage plan has to survive these.

## 2. How borrowing against a portfolio works

There are two products.

**Margin loan.** **FACT** (Schwab, M1, and the Federal Reserve's Regulation T): you can borrow up
to about 50% of eligible holdings in a taxable account. The rate is variable and interest is charged
daily. There's no repayment schedule as long as you keep enough equity, and the money can buy more
stock. Example rates: M1 about 5.90% (September 2026) and Schwab's base rate 10.00%. Retirement
accounts generally can't use margin.

**Securities-based line of credit** (SBLOC, a "portfolio line of credit"; Schwab calls theirs a
Pledged Asset Line). **FACT** (SEC/FINRA investor alert; Schwab): you can typically borrow 50% to 65%
against stocks (Schwab says up to 70%) and 95% against US Treasuries. **Minimums are often
\$100,000.** The money can be spent on almost anything, but you can't use it to buy securities.

**What makes it scary (FACT, from the required margin disclosure, FINRA Rule 2264, and the SEC and
FINRA alerts):**
- The broker can sell your holdings **without contacting you**.
- It can raise its maintenance requirement **at any time without notice**.
- You aren't entitled to extra time on a margin call.
- Your losses **can exceed** the collateral.
- An SBLOC is a "demand loan" the lender can call at any time. A call usually gives two or three
  days to add collateral, but the agreement often allows selling without notice.
- **Interactive Brokers** liquidates automatically, in real time, the moment an account dips
  below maintenance. There's generally no window to deposit cash.
- **A forced sale triggers capital gains tax**, which defeats the whole point of borrowing instead
  of selling.

**Rates move.** **FACT, single source** (Bernstein, Nov 2023): when the Federal Reserve raised rates
in 2022, benchmark margin rates roughly tripled, from about 2% to over 6%. Retail rates at big
brokers ran higher, roughly 8% to 13% on small balances (**Assessment** from the verifier). The
video's creator moved his money to M1 for a 2% rate in early 2022, right before that jump.

### The one table to remember (Assessment, arithmetic)

This assumes a typical 30% maintenance requirement, meaning your equity must stay at least 30% of
the account. Brokers can raise that requirement, which makes the call come sooner.

| Loan as % of portfolio | Margin call when the market falls by | Wiped out at a fall of | Survives 2008 (−55%)? |
|---|---|---|---|
| 10% | 86% | 90% | Yes, easily |
| 20% | 71% | 80% | Yes |
| 30% | 57% | 70% | Barely misses a call |
| 40% | 43% | 60% | No: forced sale |
| 50% (the maximum) | 29% | 50% | No: 2020's 35% drop alone triggers a call |

What's actually risky is borrowing near the limit, at a variable rate, in a concentrated fund,
with no cash to cover a call. A small loan against a broad index fund, held through a crash with
spare cash on hand, is a very different thing (**Assessment**).

## 3. Compared with a mortgage on a rental (Assessment; this run verified no sources for this section)

- **No margin calls on a house.** The bank doesn't sell your house because house prices fell 30%.
  As long as you make the payment, you keep it. This is the biggest difference.
- **Fixed rate.** US mortgages are usually fixed for 30 years. Margin and SBLOC rates float.
- **Higher leverage, more stable asset.** A rental mortgage is typically 75% to 80% of the value, far
  more than safe portfolio leverage. House prices are quoted rarely, so they feel calmer, but rentals
  bring tenants, repairs, vacancy and selling costs. The video's creator made only about \$15,000 on
  a house bought for \$173,000 after counting those costs (his own account).
- **Recourse.** Some US states don't let a lender come after you for more than the house is worth
  ("non-recourse"). Margin debt can always follow you past the collateral (FACT, from the disclosure
  above). State mortgage rules weren't checked (**Unverified**).
- **Liquidity cuts both ways.** Stocks can be sold in seconds, which is also what lets a lender sell
  them in seconds.

## 4. "Buy, borrow, die" (Assessment and Unverified; no claims on this survived the research run)

The idea: never sell, so you never owe capital gains tax. Live on loans against the portfolio. At
death, US heirs get a "step-up in basis": the cost basis resets to the value at death, so the
lifetime gain is never taxed. The loan is repaid from the estate. The step-up is real US law
(Internal Revenue Code §1014; **Unverified** this session, source not reopened).

Where it works: very large portfolios where the loan stays a small share of the total, and borrowing
costs less than the growth. Where it breaks: interest compounds against you if growth stalls, a
crash can force a sale (taxed), and the plan needs decades. The critics the research run found
(Advisor Perspectives, White Coat Investor) argue that for ordinary investors the interest cost
usually beats the tax saved; their specific claims weren't verified (**Unverified**).

## 5. The video: Mike Wilson, "RecycleYourMoney"

**FACT** (YouTube metadata and a full transcript made with Whisper):
the video is "How a \$64,960 Salary turned into \$762,447 in 5 Years" (uploaded 2026-10-07, about
8,800 views, 20 minutes) by Mike Wilson, https://www.youtube.com/@RecycleYourMoney. The description
links a paid Discord and a Robinhood referral link, and says it is "for educational and entertainment
purposes only."

**What he did (FACT, his own account):**
- A sales job, \$50,000 to \$87,000 a year.
- In 2019 he bought VOO (Vanguard's S&P 500 fund).
- In late 2021 he saw a Shark Tank clip about borrowing against stocks and moved to M1 for its 2%
  margin rate.
- He used margin for house down payments, a \$35,000 truck, a Glacier National Park trip, a sauna
  and red-light therapy gear.
- He had about \$100,000 of margin borrowed going into the 2025 tariff crash (portfolio down about
  30%) and bought \$100,000 more stock during it. His biggest position is VGT (Vanguard's technology
  fund), not a total-market fund.
- He has spent 2026 paying the margin down.

**Where he's right (Assessment):** he says plainly that "flash crashes are what will margin call
you," that being "levered up all of the time" is "an egregious error," and that you shouldn't be
levered at market highs. He is now cutting his own loan to prepare for a downturn.

**Where it doesn't hold up (Assessment):**
- **The \$762,447 is not his net worth.** He says the chart shows "total portfolio value…
  including the margin that I was dumping in." With \$100,000 or more borrowed, his own money is
  smaller than the headline. The title ties it to his salary, but it's really salary plus borrowed
  money plus a strong tech market.
- **"All of this growth is tax free"** is wrong as stated. Gains you haven't sold are untaxed for
  now, and borrowed money isn't income, but dividends are taxed every year, the gains are taxed
  whenever you sell (including a forced sale), and the step-up only happens at death.
- **"With external leverage, you can't get margin called"** is half true. A home equity line of
  credit (HELOC) or a 0% credit card won't sell your stocks, but banks froze and cut HELOCs in
  2008 (**Unverified**), card rates jump when the promotion ends, and borrowing against your house
  to buy stocks puts the house at risk.
- **"Wait for a downturn, then deploy leverage"** is market timing, which he says he isn't doing.
  It worked for him in 2025, the one V-shaped recovery he lived through. 2008 took four years to
  recover.
- **Survivorship and concentration.** His result is one person, one strong tech run, five years.
- His story about a hedge fund manager "Leopold" forced to sell is **Unverified** and garbled as
  told. Don't rely on it.

## Applied to the \$5,000, 10-year, \$1 million goal (Assessment, arithmetic)

- With no additions, reaching \$1 million from \$5,000 in 10 years takes about **70% a year**.
  No index fund, and no sane leverage, does that.
- With monthly additions, the deposit needed is about **\$5,800** a month at 7%, **\$4,900** at 10%,
  and **\$4,000** at 14%.
- \$5,000 plus \$500 a month at 10% reaches about **\$113,000**.
- **Leverage isn't available at this size anyway.** SBLOCs usually start at \$100,000. Margin on
  \$5,000 means borrowing at most a few thousand at 6% to 13%: real risk for little extra money.
- What moves the outcome is the monthly deposit, a low-cost broad fund, and not selling in a crash.

## Worked example: \$5,000, then \$500 a month, loan capped at 20% (Assessment, backtest)

Method: VTI's real monthly prices with dividends reinvested (Yahoo Finance, July 2001 to September
2026), every 10-year window that fits (183 of them, starting August 2001 through October 2016).
Each month: add \$500, then borrow back up to 20% of the portfolio if the loan has fallen below that,
and buy more VTI. Interest is the actual federal funds rate (FRED, the Federal Reserve's data
service) plus a broker markup, added to the loan rather than paid out of pocket. The markup is
2 points (roughly Interactive Brokers or M1) or 6 points (roughly Schwab's posted rates). Taxes are
ignored. The backtest scripts are kept in the vault, not on this page.

Total put in over 10 years: **\$65,000**.

| | No loan | 20% loan, cheap broker (+2 points) | 20% loan, expensive broker (+6 points) |
|---|---|---|---|
| Typical (median) ending value, after repaying the loan | ~\$124,000 | ~\$141,000 | ~\$132,000 |
| Windows where the loan helped | | 178 of 183 | 158 of 183 |
| Typical gain from the loan | | +13% | +6% |
| Worst case vs. no loan | | −4% | −9% |
| Best case vs. no loan | | +22% | +15% |
| Margin calls | | None | None |

Worked windows (ending value with the cheap broker):
- **Start August 2001**, the worst decade: dot-com tail, then 2008. Plain ~\$87,000, with loan ~\$87,000. The loan earned nothing.
- **Start October 2007**, buying at the peak right before the crash: ~\$128,000 → ~\$147,000.
- **Start January 2012**: ~\$163,000 → ~\$198,000.

What the backtest shows:
1. **The loan adds roughly a tenth to a fifth**, not multiples. \$500 a month makes about \$125,000
   to \$140,000 in 10 years, not \$1 million.
2. **"20% max" means 20% when you borrow, not 20% forever.** If you top the loan up to 20% at a
   market peak, a 2008-sized crash pushes it to about **38% to 40%**. That's still well short of a
   margin call (about 70% with a 30% maintenance requirement), but it only stays safe if you
   **never borrow more while the loan is above 20%**. That rule is built into the backtest.
3. **The broker's interest rate decides whether it's worth it.** At Schwab-like rates, the loan lost
   money in about one window in seven.
4. **Interest compounds onto the loan.** The backtest assumes you never pay it from income. Paying
   the interest out of pocket is safer and lowers the loan.
5. Margin needs at least \$2,000 in the account (FINRA's minimum; **Unverified** this session), so
   the loan can't start until the account is past that.

### More per month, and paying the interest yourself (Assessment, same backtest)

Same method, same 183 windows. The backtest scripts are kept in the vault, not on this page.

| Monthly deposit | You put in | No loan (typical) | 20% loan, cheap broker (typical) | No loan, worst decade |
|---|---|---|---|---|
| \$500 | \$65,000 | ~\$124,000 | ~\$141,000 to \$148,000 | ~\$76,000 |
| \$1,000 | \$125,000 | ~\$236,000 | ~\$266,000 to \$279,000 | ~\$145,000 |
| \$1,500 | \$185,000 | ~\$349,000 | ~\$390,000 to \$410,000 | ~\$214,000 |

The loan column's lower figure adds the interest to the loan. The higher figure pays the interest
from income, so more of the portfolio stays yours. That interest costs about \$40, \$70 or \$100 a month
on average at the cheap rate, and about \$90, \$170 or \$250 at the expensive rate.

**Paying the interest yourself does not make the loan more profitable.** Compared fairly, against
someone with no loan who invests that same interest money instead, the gain from the loan is the
same either way: about +12% typical at the cheap rate and about +5% at the expensive rate, with
the worst case about −4% and −9%. What paying it yourself buys is **safety**:
- The loan never grows on its own.
- The 2008-style peak loan share drops from about 38% to 40%, to about 36%.
- If the market goes sideways for years, you aren't watching the interest eat your equity.

**The deposit matters far more than the loan.** Going from \$500 to \$1,000 a month nearly doubles
the result; the loan adds about a tenth. On these numbers, \$1 million in 10 years from VTI needs
roughly \$4,000 to \$5,000 a month, loan or not.

### One broker: Webull (FACT, https://www.webull.com/margin, fetched 2026-10-13, rates "as of March 31, 2026")

- **Standard account:** 8.74% at every balance. With the federal funds rate around 3.75% (FRED,
  September 2026), that's about 5 points over, close to the backtest's "expensive broker" case.
- **Webull Premium** (\$3.99 a month or \$40 a year, per Webull's blog): 5.20% under \$25,000 and
  5.00% from \$25,000 to \$100,000. That's about 1.5 points over, slightly better than the
  backtest's "cheap broker" case.
- **Margin needs \$2,000 of equity** in the account (FACT, same page).
- **Assessment:** Premium saves about 3.5 points of interest, so it pays for its \$40 once the
  loan passes roughly \$1,150. A 20% loan reaches that once the account is near \$6,000. Without
  Premium, the backtest's expensive-broker column is the one that applies: still ahead in most
  decades, but by about 5% rather than 12%, and behind in about one decade in seven.

### \$750 or \$1,000 a month on Webull (Assessment, backtest)

Same 183 windows. Borrowing starts only once the account holds \$6,000, the loan is kept at or
below 20%, interest is paid from income each month, and the rate is the federal funds rate plus
1.5 points (Premium) or plus 5 points (Standard). The comparison is fair: the no-loan investor also
invests the money the borrower spent on interest. The backtest scripts are kept in the vault, not on this page.

| | \$750 a month | \$1,000 a month |
|---|---|---|
| You put in | \$95,000 | \$125,000 |
| No loan, typical | ~\$180,000 | ~\$236,000 |
| No loan, bad decade (1 in 10 worse) | ~\$143,000 | ~\$186,000 |
| No loan, worst decade | ~\$111,000 | ~\$145,000 |
| 20% loan, typical ending value | ~\$212,000 | ~\$279,000 |
| Interest paid, average per month | ~\$45 Premium / ~\$112 Standard | ~\$58 Premium / ~\$145 Standard |
| Loan helped, Premium | 179 of 183 decades, typically +12.5%, worst −3% | same |
| Loan helped, Standard | 163 of 183 decades, typically +6.5%, worst −8% | same |
| Highest loan share in a crash | 36% (margin call at about 70%) | 36% |

## If the market drops: a playbook (Assessment, for the worked example)

The example plan: VTI, \$750 to \$1,000 a month, a loan of at most 20% on Webull Premium, with interest
paid from income. Decide this now, while calm, because in a crash every instinct says sell. The
numbers below use a worked example: about five years in, an \$80,000 account with a \$16,000 loan.

**What a crash does to that account (arithmetic):**

| Market falls | Account becomes | Loan share | What happens |
|---|---|---|---|
| 30% (like 2020) | \$56,000 | 29% | Nothing. Keep going. |
| 50% (like 2008) | \$40,000 | 40% | Nothing forced, but you're in the danger zone. |
| 60% | \$32,000 | 50% | A margin call **if** Webull has raised its requirement to 50%. |
| 71% | \$23,000 | 70% | A margin call at a normal 30% requirement. |

Brokers can and do raise maintenance requirements during crashes, without notice (FACT, the
margin disclosure above). Webull's current requirement for VTI wasn't checked (**Unverified**), so
plan as if a call could come at a 60% fall instead of 71%.

**The rules:**
1. **Never sell to "get out."** A broad index fund has always come back; 2008 took about four
   years (FACT, VTI's drawdown above). Selling locks in the loss and triggers tax.
2. **Keep the deposits going.** The backtests assume you did. Crash-month deposits buy cheap
   shares and drive most of the long-run edge.
3. **Stop borrowing while the loan is above 20%.** Don't top it up in a crash, however tempting
   "buy the dip with margin" sounds. That's how the video's creator ended up with \$100,000
   borrowed in a 30% fall.
4. **Above 30%, send deposits to the loan instead of buying.** Each \$1,000 paid down lowers the
   loan share directly and buys safety when it's worth the most. Go back to buying once the loan
   is under 25%.
5. **Keep cash outside the account roughly equal to half the loan** (about \$8,000 in the example).
   It's the answer to a margin call that doesn't involve selling at the bottom. It's also the
   protection the video calls "external leverage," without borrowing against a house or a truck.
6. **Keep paying the interest.** If income drops (lost job, emergency), stop the deposits first
   and pay down the loan second. Never let unpaid interest pile onto the loan during a crash.
7. **Turn on Webull's margin alerts, then stop checking daily.** Know your loan share. Don't
   watch the price.
8. **Write the numbers down now:** today's loan, account value and loan share, and the falls that
   would take you to 30%, 40% and 50%. A crash is the wrong time to do this math.

**When it's over:** once the loan share is back under 20%, borrowing can resume. Recovery is when
the loan does its best work, buying while prices are still low.

## Open questions

1. Webull's maintenance requirement for VTI, and how often it has raised requirements in past crashes.
2. VTI's current expense ratio and holdings count, from Vanguard directly.
3. A fixed-rate mortgage versus a 20% portfolio loan, compared on cost and risk with real numbers.

## Sources

- SEC/FINRA investor alert on SBLOCs: https://www.sec.gov/oiea/investor-alerts-and-bulletins/sbloc
- FINRA, securities-backed lines of credit: https://www.finra.org/investors/insights/securities-backed-lines-credit
- Schwab, borrowing strategies: https://www.schwab.com/learn/story/borrowing-strategies-big-expenses
- Interactive Brokers margin: https://www.interactivebrokers.com/en/index.php?f=846
- M1 margin rates: https://help.m1.com/en/articles/9332230-what-are-the-m1-margin-loan-interest-rates
- Bernstein on margin and rates: https://bernstein.com/our-insights/insights/2023/articles/does-margin-still-work-amid-higher-rates.html
- VTI returns: https://totalrealreturns.com/n/VTI,AGG and https://www.alphacubator.com/analysis/VTI
- VXUS vs VT: https://www.fool.com/coverage/etfs/2025/12/27/vxus-vs-vt-go-international-only-or-include-u-s-stocks
- The video: https://youtu.be/76PBycxHvac
- Price data: Yahoo Finance monthly adjusted closes for VTI; rates: FRED FEDFUNDS, https://fred.stlouisfed.org/series/FEDFUNDS
