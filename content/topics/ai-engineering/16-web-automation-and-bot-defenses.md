---
type: lesson
series: ai-engineering
chapter: 16
title: Web automation and bot defences
status: curated
aliases: ["topics/ai-engineering/16-web-automation-and-bot-defenses"]
tags: [ai, ai-agents, automation, web-scraping, browsers, ethics, verification, using-ai-well]
created: 2026-08-15
---

# Getting data off the web without being a problem

There is a boring, real job that nobody should be doing by hand: following twenty links,
reading a number off each page, and typing it into a spreadsheet. That is exactly the kind
of work a computer should do. And yet more and more of the web now answers an automated
request with a wall.

Both sides of that are reasonable, and this chapter is about how to live in the middle of
it. The short version is that the effective moves and the polite moves are almost entirely
the same moves, and the tricks people reach for first mostly do not work.

## Why sites block automated traffic

A site owner is not usually annoyed that a program is reading their page. They are annoyed
about volume and cost. One person clicking around costs nothing. A careless script can
issue thousands of requests a minute, which looks a lot like an attack, and every one of
those requests costs the owner money in bandwidth and server time. Add to that the newer
worry: content being harvested wholesale to train or feed AI systems that then answer
questions the site used to be visited for.

FACT: Cloudflare announced in 2025 that it had become "the first Internet infrastructure
provider to block AI crawlers accessing content without permission or compensation, by
default," letting site owners choose whether AI crawlers may access their content and how
AI companies may use it. FACT: the same company later described setting new defaults on
15 September 2026 in which the **Training** and **Agent** categories of AI traffic are
blocked by default for new domains.

Assessment: that is the important shift to understand. The default on a large slice of the
web moved from "allowed unless you say no" to "refused unless you ask." A wall in your way
is usually not a personal accusation; it is a policy that was switched on for everybody.

## What a bot blocker actually looks at

This is where most people's mental model is wrong, and being wrong here wastes hours.

**It cannot see your browsing history.** There is no way for a site to ask your browser
what other sites you have visited; that was closed off years ago as a privacy leak. So
browsing around to "look like a real person" transfers nothing. Credibility does not
generalise across the web. It is per-site, and it lives in that site's own cookies.

What the checks really read:

- **The TLS handshake.** Before any page loads, your client and the server negotiate
  encryption, and the exact shape of that negotiation differs between real Chrome, Python's
  `requests`, and `curl`. It is a fingerprint, and it is visible before a single byte of
  HTML moves.
- **HTTP/2 details**, such as the order in which the connection's setup frames are sent.
- **The JavaScript environment**: the fonts you have, how your graphics card draws a test
  image, which plugins exist, how the window is sized.
- **Automation instrumentation.** A browser being driven by a program is usually driven
  over a debugging channel, and that channel is detectable from inside the page.
- **IP reputation** — whether your address belongs to a home broadband line or a rented
  server in a data centre.
- **Cookies you already hold**, including the token a challenge hands out when you pass it.

Assessment: the popular idea that "headless" is the giveaway, and that running a visible
browser fixes it, addresses one item on that list. That is why a real window on a real
screen still fails: the checkbox challenge is not asking whether a human is present, it is
asking whether the browser is under remote control. A person can click it all day and it
will keep looping.

## The ladder: try these in order

Work top-down. Each step is more polite, more reliable and less work than the one below it.

1. **An official API, registered properly.** Slowest to start, best forever. It is designed
   for this, it is stable, and nobody has to guess your intent.
2. **A bulk export or data dump.** Many sites publish the whole dataset precisely so you do
   not have to crawl it page by page. One download beats a thousand requests.
3. **A published dataset or mirror.** If somebody has already lawfully republished what you
   need, take it from them and leave the origin alone entirely.
4. **Your own authenticated session.** If a site lets *you* see something when logged in,
   logging in as yourself and reading it is not a disguise. It is you, using your account.
5. **Careful direct fetching**, where the site's own rules permit it — slowly, cached, and
   identifying yourself honestly.

Below that line sits the tier people reach for first: faking fingerprints, rotating through
rented addresses, paying services to solve challenges. Assessment: skip it. It is an arms
race you lose on a schedule, it breaks whenever the other side ships a change, and it is
the behaviour that caused the walls to go up for everyone else. The polite ladder is also
the one that still works in six months.

## The etiquette that actually matters

FACT: the Robots Exclusion Protocol — the `robots.txt` convention from 1994 — was written
up as a standard, RFC 9309, in 2022, defining how automatic clients should read a site
owner's rules about what may be fetched. Assessment: it is a request, not a lock. Nothing
enforces it. Honouring it is the cheapest possible signal that you are not the problem.

The habits that keep you welcome:

- **Ask for less.** If an endpoint takes a list, send one request for twenty things rather
  than twenty requests. This is usually the single biggest reduction available.
- **Cache everything.** Never fetch the same thing twice because your script restarted.
- **Go slowly on purpose.** A few seconds between requests costs you nothing overnight and
  keeps you far below the level anyone notices.
- **Say who you are** in the User-Agent, with a way to contact you.
- **Prefer off-peak hours** for anything long-running.
- **Stop on errors.** Repeated failures mean back off, not retry harder.

## Where the law sits, roughly

Assessment first, because the shape matters more than the detail: "not a crime" and
"allowed" are different things, and people routinely collapse them.

FACT: the Ninth Circuit's 2022 opinion in *hiQ Labs, Inc. v. LinkedIn Corp.* records that,
on remand from the Supreme Court, the panel affirmed an order "preliminarily enjoining
LinkedIn Corp. from denying hiQ Labs, Inc., a data analytics company, access to publicly
available member profiles." FACT: the same opinion states that "The CFAA prohibits
accessing a" protected computer "without authorization," and reads that provision in light
of *Van Buren v. United States*.

FACT: per the case history, that same dispute later saw a ruling that hiQ had breached
LinkedIn's "User Agreement and a settlement agreement was reached between the two parties."

Assessment: so reading a genuinely public page is a poor fit for a computer-crime statute,
and it can still breach the contract you accepted by using the site. Those are two separate
questions with two separate answers, and winning the first does not win the second. Note
also that this is one country's courts; none of it is legal advice.

## A worked example

FACT: BoardGameGeek's published policy states that "Registration and authorization is
required for use of the XML API," directs applications to `boardgamegeek.com/applications`,
warns it "may be a week or more" before a reply, and lists narrow exceptions — downloading
your own collection, or "the CSV dump of all games" — both "while logged in."

What happened when this vault wanted twenty-one game ratings, in order:

- A plain request to the API returned 401. Not a bot check — a policy.
- A stealthed browser failed. A **real** browser, on a **real** screen, with a saved
  profile, also failed, and never received so much as a cookie.
- Checking the network showed the block was specific to that site: other sites behind the
  same protection provider answered normally from the same connection.
- A published mirror of the site's own daily export supplied most of the ratings, costing
  the site nothing, and carrying a date so every number could say when it was true.
- The four newest games were missing from that export, and the owner read them off four
  pages by hand in about a minute.
- The durable fix was to register for an API key, as the policy asks.

Assessment: the fastest route was never the cleverest one. Reading the site's own rules
took ten minutes and pointed at three legitimate doors, two of which were open immediately.

## The honest bottom line

- Automating dull data collection is a fair thing to want. Say so plainly and act like it.
- The blockers are aimed at volume and at wholesale harvesting, not at you personally.
- Almost every wall has a sanctioned door beside it. Read the site's own page about
  automated access before writing any code.
- Anything you would be embarrassed to describe to the site owner is the wrong approach,
  and it is usually also the one that breaks first.

## See also

- **In this series:** [← Forward deployed engineers](15-forward-deployed-engineers) · [Overview](topics/ai-engineering/)
- **[Safety and good habits](13-safety-and-best-practices)** — the same instinct applied to
  agents that can act: least power, clear limits, verify before trusting.
- **[Tools and MCP](07-tools-and-mcp)** — how an agent reaches the outside world in the
  first place.
- **[Using AI well](connections/using-ai-well)** — a fluent answer is not a checked one.

## Sources

- Cloudflare, *Cloudflare Just Changed How AI Crawlers Scrape the Internet-at-Large* (2025) — https://www.cloudflare.com/press/press-releases/2025/cloudflare-just-changed-how-ai-crawlers-scrape-the-internet-at-large/
- Cloudflare, *Your site, your rules: new AI traffic options for all customers* — https://blog.cloudflare.com/content-independence-day-ai-options/
- IETF, *RFC 9309: Robots Exclusion Protocol* (2022) — https://www.rfc-editor.org/rfc/rfc9309.html
- United States Court of Appeals for the Ninth Circuit, *hiQ Labs, Inc. v. LinkedIn Corp.*, No. 17-16783 (18 April 2022) — https://cdn.ca9.uscourts.gov/datastore/opinions/2022/04/18/17-16783.pdf
- *hiQ Labs v. LinkedIn* — case history and final disposition — https://en.wikipedia.org/wiki/HiQ_Labs_v._LinkedIn
- BoardGameGeek, *Using the XML API* (policy page, read via the Internet Archive because the live page is behind a bot check) — https://web.archive.org/web/20260715032744/https://boardgamegeek.com/using_the_xml_api
