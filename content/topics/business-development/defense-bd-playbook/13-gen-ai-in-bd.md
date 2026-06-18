---
type: playbook
series: defense-bd-playbook
chapter: 13
title: Using Gen AI in BD without fooling yourself
status: curated
tags: [business-development, defense, capture-management, govcon, proposals, playbook, ai]
created: 2026-06-18
---

# Chapter 13 — Using Gen AI in BD without fooling yourself

Generative AI is now in every BD shop, and it genuinely speeds the rote work. But
BD lives on a handful of high-stakes judgment calls made under uncertainty (PWin,
price-to-win, bid/no-bid, what the competition will do), and there is now
experimental evidence that Gen AI can quietly degrade exactly that kind of
judgment. This chapter is a cross-cutting discipline: it applies to the qualifying
decisions in [chapter 2](02-qualification-pwin-gates.md), the competitive and price
calls in [chapter 6](06-win-strategy-competitive.md), and the reviews in
[chapter 7](07-proposal-management.md).

## The evidence

FACT: In a controlled experiment with roughly 300 managers and executives,
researchers had participants forecast a stock (Nvidia) one month out, then revise
after either a peer discussion or a ChatGPT consultation. The group that consulted
ChatGPT became more optimistic (raising estimates by about $5.11 on average), while
the peer-discussion group became more conservative (lowering estimates by about
$2.20). When checked against what actually happened, both groups were too
optimistic, but the ChatGPT users made worse predictions, while the peer-discussion
users made better ones than before. The AI users also became more overconfident,
offering more falsely precise "pinpoint" estimates. Source: José Parra-Moyano,
Patrick Reinmoeller, and Karl Schmedders, "Research: Executives Who Used Gen AI
Made Worse Predictions," Harvard Business Review (HBR.org), July 1, 2025, Reprint
H08SQS.

FACT: the authors offer five explanations for why AI pushed people toward
optimism and overconfidence:

1. **Extrapolation / trend-riding.** The model extends recent historical trends,
   effectively assuming what has been going up keeps going up.
2. **Authority bias and detail overload.** The AI's confident, thorough, expert
   tone led users to over-weight it relative to their own judgment.
3. **No emotional gut-check.** Humans feel wariness at a peak ("this could crash");
   the AI has no such instinct and does not second-guess an optimistic trend.
4. **Peer calibration.** Group discussion surfaces diverse views, moderates
   extremes, and adds a "don't be the naive optimist" social brake. This is the
   opposite of groupthink: peers reined each other in.
5. **Illusion of knowledge.** Access to a vast, fluent information tool makes people
   feel they know more than they do.

The authors also note the study's limits (a single stock, a one-month horizon, an
executive-education setting, and a model without live data), so treat it as a
strong signal rather than the last word. Assessment: even discounted, the direction
of the finding maps directly onto BD's failure modes.

## Why this hits BD specifically

Assessment: the playbook's most important numbers are all forecasts under
uncertainty, and they are exactly the kind the study shows AI inflates.

- **PWin** ([chapter 2](02-qualification-pwin-gates.md)): an AI that rides trends
  and sounds authoritative will tell you a pleasing story about your chances. An
  inflated PWin is the single most common way BD organizations talk themselves into
  bidding deals they should pass.
- **Price-to-win** ([chapter 6](06-win-strategy-competitive.md)): a confident AI
  pinpoint price can anchor your whole pricing decision on a number that is
  precisely wrong. PTW demands ranges and competitor logic, not false precision.
- **Black Hat and competitor reads** ([chapter 6](06-win-strategy-competitive.md)):
  the AI lacks the adversarial, "fear of being wrong" instinct that makes a Black
  Hat useful, and it will extrapolate rather than imagine a competitor's surprise.
- **Pipeline honesty** ([chapter 9](09-pipeline-metrics-cadence.md)): optimism bias
  compounds across a pipeline, turning a weighted forecast into wishful thinking.

## Where Gen AI helps (the safe lane)

Assessment, consistent with the article's framing that AI boosts simple and rote
tasks and aids communication: use it freely for work where a human still owns the
judgment and verifies the output.

- First drafts of proposal boilerplate and non-discriminating sections.
- Summarizing long RFPs, RFIs, and reference documents (then verify against the
  source).
- Building a compliance-matrix skeleton from Sections L and M
  ([chapter 7](07-proposal-management.md)), for a human to check line by line.
- Generating question lists for industry days and call plans, research starting
  points, and reformatting or tightening prose.

## Where to keep humans in charge (the danger lane)

Assessment: do not let AI own any decision the award hinges on. Keep human judgment
primary for PWin scoring, bid/no-bid, price-to-win, Black Hat and competitor
assessment, win-strategy bets, and final proposal evaluation. Use AI as an input to
these, never as the decider.

## Four working rules (translated from the research)

1. **Make the AI argue against itself.** Whenever you use AI for an estimate, name
   the exact data it should use, demand a range or confidence interval instead of a
   pinpoint, and explicitly ask "how could this be wrong?" and, in BD terms, "what
   would make us lose this?" This turns the tool's fluency toward finding your
   exposure rather than flattering you.
2. **Keep the room.** Do not let an AI take replace the gate review or the color
   team. Sequence it: get the AI's take, then convene the human debate. The study's
   clearest result is that structured peer discussion improved judgment, it is a
   debiasing mechanism, not overhead ([chapters 2](02-qualification-pwin-gates.md)
   and [7](07-proposal-management.md)).
3. **Apply critical thinking regardless of source.** Treat AI output as a starting
   point for inquiry, not the answer; probe its basis and what recent factors it may
   be missing. Apply the same skepticism to the human group, ask whether you are
   being too conservative simply because everyone is uncertain.
4. **Set team guidelines.** Make it explicit to the BD team that AI tends toward
   optimism and overconfidence, and institutionalize a counterweight, for example a
   worst-case or peer-discussion step before any AI-influenced PWin or price gets
   locked. The article's image is Ulysses needing wax and rope to resist the
   Sirens: the guardrails go in before you listen.

## The non-negotiable BD guardrail: never feed sensitive data to a public tool

Assessment, and this one is specific to defense: BD routinely handles competition-
sensitive, proprietary, source-selection, Controlled Unclassified Information
(CUI), and sometimes classified material. Never paste any of that into a public or
commercial AI tool. Doing so can breach proprietary and teaming obligations, create
or worsen an organizational conflict of interest, violate the Procurement Integrity
Act, and (for CUI or classified data) constitute a serious security incident. Use
only approved, accredited tools for sensitive content, and when in doubt treat the
data as off-limits. This ties directly to the integrity rules in
[chapter 8](08-relationships-ethics-oci.md).

## Bottom line

Assessment: Gen AI is a powerful drafting and research assistant and a dangerous
oracle. In BD, let it accelerate the rote work and serve as one input that you
force to expose downside, but keep the judgment calls, the peer challenge, and the
sensitive data firmly in human, accredited hands. Used that way, it sharpens the
playbook; used as an authority, it will confidently help you lose.

Next: [Further reading (HBR)](14-further-reading-hbr.md).
