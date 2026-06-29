---
type: lesson
series: ai-engineering
chapter: 9
title: "MRAgent: reconstructive memory"
status: curated
tags: [ai, ai-agents, llm, agent-engineering, memory, mragent, verification, constraint-solving]
created: 2026-06-28
---

# MRAgent: reconstructive memory

The [memory chapter](03-memory-for-agents) ended by pointing at a newer idea, and this is
it. MRAgent is a 2026 research system for giving an AI agent memory. Its whole pitch is in
the title of the paper behind it: memory is reconstructed, not retrieved. Most memory
systems look up a fixed set of saved notes and then think about them. MRAgent does it in
the other order: it thinks as it goes, stepping through a web of linked notes and deciding
where to look next based on what it has found so far. This chapter explains what the name
means, how it works, what the numbers really show, and an honest read on what holds up
versus what is just hype.

## What it is, and the name to get right

FACT: the system is called MRAgent, and "MR" stands for "Reconstructive Memory." The
paper's own section title spells it out: "Reconstructive Memory Agent." (arXiv:2606.06036,
checked against the full paper.)

Assessment: getting the name right matters, because the wrong one is everywhere. Many
AI-written summaries claim MR stands for "Memory Reasoning Architecture for LLM Agents."
That phrase is nowhere in the paper. It is a made-up name that got copied around, a live
example of the confidently-wrong problem from the [safety chapter](08-safety-and-best-practices).
The real name is Reconstructive Memory Agent.

FACT: the authors are Shuo Ji, Yibo Li, and Bryan Hooi at the National University of
Singapore, and the paper went up on 4 June 2026. The code is at github.com/Ji-shuo/MRAgent.
(Do not confuse it with an unrelated medical tool that shares the name.)

## Where it was actually published

I tracked down where this was published, because the sources disagree and it changes how
much to trust the results.

FACT: the most reliable record is the official listing on a review site called OpenReview,
which says the paper appeared at the "ICLR 2026 MemAgents workshop," a real workshop with
its own website. So it is a workshop paper, posted in March 2026. (OpenReview, forum
YPoHy6lgKP.)

FACT: the one conflicting clue is a note the authors typed onto the arXiv page that says
"Accepted at ICML 2026." Nothing else backs that up: there is no official ICML record of
it anywhere. Assessment: when an author's typed note clashes with the official listing,
the official listing wins. The ICML line is most likely an error or an out-of-date note.
Treat the venue as the ICLR workshop, and the ICML claim as unconfirmed.

FACT: this matters because the workshop listing shows no reviews at all, no expert comments
for or against. Assessment: a workshop is a lower bar than a main conference, and "peer
review," other experts checking the work before it is published, does not appear to have
happened here in public. The honest upshot runs through the rest of this chapter: every
number below comes from the authors themselves, and no outside group has checked it or
repeated it.

## The core idea: rebuild, do not just look up

FACT: most memory systems work in a fixed order. They look up a set of saved notes, then
reason about them. The paper argues this is too rigid, because the system has already
chosen what to fetch before it has thought about the question at all. (arXiv:2606.06036.)

FACT: MRAgent turns that into a back-and-forth. It reasons and looks things up at the same
time, in steps, and each thing it finds shapes where it looks next. That lets it reach
facts a one-shot lookup would miss. The name comes from how human memory works: we rebuild
a memory from pieces, rather than pulling a finished file off a shelf. (arXiv:2606.06036.)

## How the memory is stored: cues, tags, and content

FACT: the memory is a graph, which is just a web of points joined by links. It has three
kinds of point. A *cue* is a small keyword, such as a name or a detail. A *content* point
is an actual saved memory. A *tag* is the link in the middle that connects a cue to the
content it belongs to. Everything is stored as these cue-tag-content triples.
(arXiv:2606.06036.)

![Diagram: three columns of points, cues on the left, tags in the middle, content on the right, joined by links; one path is highlighted to show the system stepping from a cue through a tag to a memory](img/cue-tag-content.png)
*Memory as a web of cues, tags, and content, with one path through it highlighted. Diagram.*

FACT: the memory also sorts itself into layers, from specific events up to broader topics,
and it is built by having a model rewrite each piece of a conversation into a clean,
standalone sentence and pull out its keywords. (arXiv:2606.06036; project notes.)

## How it answers a question

FACT: answering is a loop. The model looks at the question and what it has found so far,
picks a promising direction, steps along the links to it, and then trims away the branches
that turned out not to matter, so its workspace stays small. The goal is to follow a few
useful trails instead of blindly opening every link, which would balloon out of control.
(arXiv:2606.06036.) The released code does this with a small set of search tools.

Assessment: the clever part is the feedback. Because the model thinks between steps, what
it learns on step two changes which link it follows on step three. That is the "rebuilding"
a fixed lookup cannot do, and it is how MRAgent reaches facts a plain search would miss.

## The numbers, read carefully

The headline results are real, but they mean something narrower than the hype suggests.
Here is the paper's own efficiency table, measured per question on a standard test set
called LongMemEval. The lowest value in each row is in bold.

| Per question (LongMemEval) | MRAgent | Mem0 | MemoryOS | A-Mem | LangMem |
|---|---|---|---|---|---|
| Tokens used | **118k** | 245k | 273k | 632k | 3,268k |
| Time (seconds) | 586 | **533** | 3,136 | 1,122 | 1,210 |

A couple of terms first. A "test set" is a standard batch of questions everyone runs
against. The other systems in the table, like Mem0 and LangMem, are the "rivals" MRAgent
was compared with. "Tokens" are the text chunks you pay for, from the
[cost chapter](12-cost-and-speed).

FACT: MRAgent used about 118,000 tokens per question, against LangMem's 3.26 million. That
is the famous "27 times fewer tokens" result. (arXiv:2606.06036.)

Assessment: two things keep that honest. First, it counts only the tokens spent answering a
question, not the cost of building the memory in the first place, which is a separate step.
So read it as "27 times cheaper per question," not "27 times cheaper overall." Second,
fewest tokens is not the same as fastest. Mem0 actually finished quicker, 533 seconds
against MRAgent's 586. And 586 seconds is about ten minutes per question, a lab-test
number, not something you would put behind a live chat.

FACT: the accuracy gains are real too. On one test, run on top of Google's Gemini model,
MRAgent scored 84 out of 100 against the best rival's 68, the "up to 23% better" headline.
On other tests the gap was sometimes smaller and sometimes larger. (The system was tried on
two underlying models, Gemini and Claude.) (arXiv:2606.06036, Tables 1 and 3.)

Assessment: notably, two strong memory systems, Zep and MemGPT, were left out of the
comparison. Zep is widely seen as the accuracy leader on this exact test, so leaving it out
is a real gap.

## The limits the authors admit

FACT: the paper is honest about two weaknesses. First, it can be slow: because it thinks
its way through the search, questions that need many steps take longer than a simple
lookup. Second, the memory only grows. It never updates, merges, or forgets, so it keeps
getting bigger and needs more storage the longer it runs. (arXiv:2606.06036, Section 7.)

Assessment: this lines MRAgent up against the other tools from the
[memory chapter](03-memory-for-agents). Mem0 and LangMem are smart about *writing* memory,
cleaning it up as they go. Zep is smart about *time*, marking old facts as expired.
MRAgent is smart about *reading*, thinking hard at lookup time. Spending its effort at read
time is why it uses so few tokens, but the trade is a store that never forgets.

## Trues and falses

A claim-by-claim check, since this is a topic where confident, wrong statements get
repeated a lot.

| Claim you will see | Verdict |
|---|---|
| "MR stands for Reconstructive Memory" | **True.** The paper's own section is titled "Reconstructive Memory Agent." |
| "MR stands for Memory Reasoning Architecture for LLM Agents" | **False.** That phrase is nowhere in the paper; an AI summary made it up. |
| "About 27x fewer tokens than LangMem" | **True, with a catch.** 118k vs 3.26M per question, but that counts only answering, not building the memory. |
| "MRAgent is the fastest memory system" | **False.** It uses the fewest tokens; Mem0 is faster on the clock (533s vs 586s). |
| "Up to 23% more accurate" | **True.** On one test it scored 84 vs the best rival's 68. On another the gap was even bigger. |
| "Accepted at ICML 2026" | **Unconfirmed.** Only an author note says so; the official listing says the ICLR 2026 workshop. |
| "A peer-reviewed result" | **Misleading.** It is a workshop paper with no public reviews; the numbers are author-reported. |
| "Independently reproduced" | **False / none found.** No outside group has repeated it yet. |

## Pros and cons

Assessment: pulling it together.

**Pros.** The core idea, think while you search instead of search then think, is
well-motivated, and it matches where the gains show up: the paper reports that hard
questions get notably better the more steps the system takes. The cue-tag-content web is a
genuine design idea, and the authors' own tests, where they switch off one part to see how
much it mattered, show that both the tags and the step-by-step thinking pull their weight.
The token savings are striking and a little surprising: doing *more* thinking at lookup
time can cost *fewer* tokens, because it avoids dragging a big pile of notes into the
window. The released code looks complete, even if no one outside has confirmed it matches
the paper.

**Cons.** Every number comes from the authors, from a workshop paper with no public review
and no outside repeat, so the *size* of the headline (27 times) deserves more doubt than
its *direction* (more efficient). A team testing its own system tends to tune its own
system, not its rivals, and LangMem's 3.26-million-token figure is extreme enough to hint
its settings may not have been fair. The slowness grows with the number of steps, so the
hard questions it is built for are exactly the slow ones. The memory never forgets or
updates, which makes it a poor fit for long-running agents whose facts change. And the
testing is narrow, two question-and-answer sets, leaving out the strongest accuracy rival,
Zep.

## How it compares, and when to use it

Assessment: MRAgent is one of four shapes of agent memory from the
[memory chapter](03-memory-for-agents), and the right pick depends on your main constraint.

![Diagram: three cards comparing MRAgent (rebuild: best for hard multi-step questions over a fixed history at low token cost; weak on speed and changing facts), Mem0 (consolidate: best for speed and changing facts; weaker on multi-step reasoning), and Zep (track time: best for facts that change over time; costs more to set up)](img/mem-compare.png)
*Which memory approach fits which job. Diagram.*

Assessment: reach for MRAgent's approach when answering hard, multi-step questions over a
fixed set of history matters more than speed, and you want to keep the per-question token
cost low. Reach for Mem0 when you need speed, facts that change, and a maintained library;
it is the one rival that beats MRAgent on the clock. Reach for Zep when your facts change
over time and you care about what was true when, since it can record "this was true, now it
is not," which MRAgent's ever-growing store cannot. MemGPT is a different kind of thing
again, a full agent setup that manages memory the way a computer manages its own, rather
than a memory layer you bolt on.

## How solid is the evidence

Assessment: this is the part to be clear-eyed about. The idea is interesting and well
argued, and the authors' own tests are reasonable, but the whole evidence base is
author-reported, workshop-level, and not checked by anyone outside. There is no truly
independent look at it: the trade-press coverage just repeats the paper's claims without
testing them, and the louder social-media and search-engine posts are where the made-up
"Memory Reasoning Architecture" name and the false "fastest" claim came from. A useful
counterweight from the wider field is another 2026 paper, *Does Memory Need Graphs?*, which
argues that gains from this kind of graph memory often come from ordinary setup choices
(how text is split, the prompts, the search model) rather than the clever structure itself.
None of this says MRAgent is wrong. It says the honest status is "promising and well
argued, but unproven outside the authors' own runs."

## Sources

- *Memory is Reconstructed, Not Retrieved: Graph Memory for LLM Agents* (arXiv:2606.06036) — https://arxiv.org/abs/2606.06036 (full text: https://arxiv.org/html/2606.06036v1)
- OpenReview record (venue: ICLR 2026 Workshop MemAgents; no public reviews) — https://openreview.net/forum?id=YPoHy6lgKP
- ICLR 2026 MemAgents workshop — https://sites.google.com/view/memagent-iclr26
- MRAgent code repository — https://github.com/Ji-shuo/MRAgent
- *Does Memory Need Graphs?* (a field counterpoint, arXiv:2601.01280) — https://arxiv.org/abs/2601.01280
