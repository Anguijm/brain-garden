---
type: lesson
series: ai-engineering
chapter: 15
title: "Forward deployed engineers: the job the AI boom invented"
status: curated
tags: [ai, ai-engineering, careers, forward-deployed-engineer, fde, palantir, enterprise-ai, deployment]
created: 2026-08-03
---

# Forward deployed engineers: the job the AI boom invented

Every chapter before this one is about building the thing. This one is about the person who
goes to the customer's office and makes it actually work there, which turns out to be the
harder half. The job title is **forward deployed engineer**, or FDE, and in 2026 it is being
advertised as the highest-paid engineering role in the industry. This chapter covers what it
is, where it came from, what the people doing it actually have on their CVs, what separates a
good one from a bad one, what it really pays, and how to tell a genuine FDE job from a support
role wearing the name.

## What it is, in one sentence

An FDE is a software engineer who goes and sits with a customer, learns how that business
really works, and then writes production code inside the customer's own systems until the
product delivers a result there.

FACT: the clearest description of the difference comes from Palantir, the company that
invented the role. An ordinary product engineer works on "one capability, many customers." An
FDE works on "one customer, many capabilities" ([The Pragmatic Engineer, *What are Forward
Deployed Engineers, and why are they so in
demand?*](https://newsletter.pragmaticengineer.com/p/forward-deployed-engineers), reporting
Palantir's own framing).

FACT: they are not consultants, and the distinction is specific rather than snobbery. A
consultant makes a recommendation and leaves; an FDE builds the working thing and owns whether
it delivers (same source). A Palantir FDE putting it in his own words: what separates the job
from consulting is "how technically creative we can be while also delivering solutions"
([Palantir, *A Day in the Life of a Palantir Forward Deployed Software
Engineer*](https://blog.palantir.com/a-day-in-the-life-of-a-palantir-forward-deployed-software-engineer-45ef2de257b1)).

FACT: the job alternates. FDEs embed with customer teams and then rotate back to core product
engineering, and when a customer needs a capability the product lacks, the FDE is expected to
go build it into the product (Pragmatic Engineer, same article). Assessment: that rotation is
the whole design. It is what stops the role from decaying into a services arm, because the
work flows back into the thing everyone else sells.

## Where it came from

FACT: Palantir created the role in the early 2010s and called it "Delta." Until around 2016
Palantir had more forward deployed engineers than it had ordinary software engineers, and even
now no company employs more of them or has shaped the role more (Pragmatic Engineer, same
article).

Assessment: the reason is worth understanding, because it explains why AI companies copied it.
Palantir's early customers were intelligence and defence agencies who often could not describe
what they needed through normal product discovery, because the requirement itself was
classified or buried in a workflow nobody had written down. You cannot gather requirements
from a customer who is not allowed to tell you the requirement. So Palantir sent engineers
into the building instead.

## Why the role exploded in 2026

FACT: the venture firm a16z argues the AI industry has rediscovered an old trade-off it calls
trading margin for moat. Enterprise software companies that do heavy implementation work post
worse margins early: at their initial public offerings ServiceNow's gross margin was **63.2%**
and Workday's just **54.1%**, against roughly 80% considered ideal for software. By 2024 those
same companies were at **79%** and **75%** ([a16z, *Trading Margin for Moat: Why the Forward Deployed Engineer Is the Hottest Job in
Startups*](https://a16z.com/services-led-growth/)).

Assessment: that pair of numbers is the argument in miniature. The implementation work looks
like a bad business while you are doing it and looks like a moat afterwards, because by then
you own the customer's workflow and nobody can rip you out. FACT: a16z's own summary of why
enterprises need this at all is blunter than anything a vendor would say: "Enterprises buying
AI are like your grandma getting an iPhone: they want to use it, but they need you to set it
up" (same source).

Assessment: the connective tissue with the rest of this course is that the hard part of
enterprise AI turned out not to be the model. It is evaluation, data access, and the fifty
undocumented exceptions in how a real company does a task, which is
[chapter 11](11-evaluation-and-testing) and [chapter 9](09-context-engineering) as a job
description rather than a technique.

## What qualifications they actually have

The honest answer is less exotic than the salary headlines suggest, and it is worth reading
the postings rather than the commentary.

FACT: Palantir's own listing for **Forward Deployed Software Engineer** requires 1+ years of
relevant post-college work experience; a strong engineering background in a field such as
computer science, mathematics, software engineering, physics or data science; demonstrated
proficiency in a language such as Python, Java, C++ or TypeScript/JavaScript; and willingness
to travel up to 25% to client sites ([Palantir Technologies, Forward Deployed Software
Engineer](https://jobs.lever.co/palantir/dab396d4-2f14-4796-aac0-0d82883dccf0), read
2026-08-03).

FACT: the newer **Forward Deployed AI Engineer** posting asks for more specific ground: past
experience building solutions with large language models and an understanding of the wider
generative-AI landscape, plus a strong foundation in machine learning basics, which it lists as
evaluation, training, and problem decomposition ([Palantir Technologies, Forward Deployed AI Engineer](https://jobs.lever.co/palantir/636fc05c-d348-4a06-be51-597cb9e07488), read 2026-08-03).

FACT: a16z's advice to companies building these teams is that the work "doesn't require a PhD,
but it does require hustle," and that the best implementation leads are high-agency (a16z, same
article).

Assessment: put those together and the entry bar is a competent mid-level engineer who will get
on a plane, not a research scientist. What is actually scarce is the second half of the job,
and that is where the money is.

## Good versus bad

This is the part worth internalising, because it is not the usual seniority ladder.

FACT: the pitch in the video that prompted this chapter is that the valuable FDE is "truly the
best of both" (engineering and communication), explicitly not "the worst combination of both
where you're not the best communicator but you also can't code," and that the high-value hire
is the one who "can turn business understanding into working software end to end" ([*FDE: The
\$1M/Year AI Job Explained*](https://www.youtube.com/watch?v=zXysLUTLjw4), Greg Isenberg with
Vas of Varick Agents). The framing offered is that if you understand art and science and can
speak both, you have what the role needs. On the date: the operator saw the video on
2026-08-03 and reported it as released roughly 13 days earlier, which puts it in mid-July
2026, but YouTube did not return an upload date to the ingest and the page did not state one
on re-check, so this chapter does not assert a publication date for it.

Assessment: that is a real distinction but it is stated as a personality trait, so here is the
observable version. A good FDE is judged on whether the customer's problem went away. A bad one
is judged on whether something shipped. Those come apart constantly: you can deliver every
agreed feature and leave a workflow nobody uses, and on this job that counts as failure.

Assessment: three concrete separators, drawn from how the strong descriptions above differ from
the weak ones.

- **Owning the outcome versus owning the ticket.** The FDE who finds out the real blocker is a
  data-access rule nobody mentioned, and goes and gets that fixed, is doing the job. The one who
  reports being blocked is doing support.
- **Feeding the product versus accumulating custom work.** FACT: the rotation back into core
  product engineering is part of the role's definition (Pragmatic Engineer, same article).
  Assessment: an FDE whose work never generalises is generating bespoke code the company must
  maintain forever, which is the failure mode the whole model is supposed to avoid.
- **Saying no to the wrong scope.** a16z's guidance to companies is to "sell smart," pick the
  right customers and start small (a16z, same article). Assessment: the person in the room when
  the scope is set is usually the FDE, so an FDE who agrees to everything is manufacturing the
  low-margin trap rather than escaping it.

Assessment: the hardest part according to someone actually doing it is not any of the glamorous
parts. Asked what was most challenging, the Palantir FDE said "directing focus," because the
customer's needs and the product's capabilities are both moving (Palantir blog, same source).

## The money, honestly

This is where the headline and the evidence disagree, so both are here.

FACT: the video's claim is a range: "you could make anywhere from 150,000 base with
considerable equity to I've seen roles go up as high as a million dollars a year," described as
applying to people who are the best combination of consulting and engineering (video, same
source).

FACT: Palantir, the company that invented the role and employs more FDEs than anyone,
publishes an estimated salary range of **\$135,000 to \$200,000 a year** for both its Forward
Deployed Software Engineer and Forward Deployed AI Engineer postings, with total compensation
that may also include restricted stock units ([FDSE](https://jobs.lever.co/palantir/dab396d4-2f14-4796-aac0-0d82883dccf0) and [AI Engineer](https://jobs.lever.co/palantir/636fc05c-d348-4a06-be51-597cb9e07488) listings, read 2026-08-03).

Assessment: the million-dollar figure is the ceiling at a handful of frontier AI companies, is
mostly equity, and is not what the job pays. The floor is a normal senior engineering salary at
a company that will also send you to a customer site a quarter of the time. Treat "the \$1M AI
job" as a description of the top of a distribution, which is how the video actually frames it
in the body even though the title does not.

## Telling a real one from a fake one

Assessment: this is the practical filter, and it follows from the definition rather than from
anything anyone advertises. The role is new enough that the title has outrun the substance, and
FACT: the same job is also advertised as "agent engineer," "solutions architect," "sales
engineer" and "technical delivery," while some companies use the FDE title for what is
essentially technical consulting on a no-code product (Pragmatic Engineer, same article).

Four questions that separate them:

1. **Do you write code in the customer's systems, or slides about them?** Writing production
   code on customer infrastructure is the line the role is built on.
2. **Can you change the product?** If the answer is "file a feature request," the rotation back
   into core engineering does not exist and this is a services job.
3. **Who owns the outcome?** If success is measured in tickets closed or deployments completed
   rather than whether the customer's problem went away, the incentive is wrong.
4. **What happens to what you build?** If nothing generalises, you are the maintenance burden.

## How much to trust this

Assessment: the sourcing here is uneven and it is worth being explicit about which parts are
solid.

Strongest: the definition, the Palantir origin, the rotation structure, and the stated
qualifications. Those come from Palantir's own job listings and engineering blog, and from
Gergely Orosz's reporting, which is based on interviews with the Head of FDE at OpenAI, the
person running FDE at Ramp, and a seven-year Palantir FDE. Those are people doing the job at
the companies defining it.

Weaker: everything about compensation above the posted ranges, and the good-versus-bad
distinction. The million-dollar figure is one practitioner's report of roles he has seen, with
no dataset behind it, in a video whose title is built on that number. The a16z piece is a
venture firm arguing that a model it is invested in is the future, which does not make the
margin figures wrong (they are checkable) but does mean the framing is promotional. FACT: the
Pragmatic Engineer article's own compensation section sits behind its paywall and was not read
for this chapter, so nothing here rests on it.

Not established: whether the role is durable. Assessment: every argument for it assumes
enterprise AI stays hard to deploy. If the tooling gets good enough that a customer's own
engineers can do the integration, the economic case a16z makes gets weaker, and the role
compresses back toward ordinary solutions engineering. Nothing read for this chapter tests
that, and the honest position is that this is a role created by a specific and possibly
temporary gap between what models can do and what enterprises can absorb.

## See also

- **[Evaluation and testing](11-evaluation-and-testing)** — building evals against a customer's
  real data is the FDE's core technical task, not a side activity.
- **[Context engineering](09-context-engineering)** — the undocumented specifics of how a
  company works are exactly the context problem, met in person.
- **[Using Gen AI in BD without fooling yourself](topics/business-development/defense-bd-playbook/10-gen-ai-in-bd)**
  — the same deployment gap seen from the buyer's side.
