---
type: lesson
series: ai-engineering
chapter: 23
title: "Loops, graphs, and the state between them"
status: curated
tags: [ai, ai-agents, llm, agent-engineering, loops, graphs, state, langgraph, harness-engineering]
created: 2026-10-15
---

# Loops, graphs, and the state between them

> Two words that keep coming up in talk about AI agents, "loop" and "graph", and the one
> problem both of them exist to solve, which is what survives from one step to the next.

## What I wanted to know

The question that started this chapter, asked on 2026-10-15: loops make sense (run an
agent on a timer, or until a condition is met, through a series of steps). A graph sounds
like a process flow, what a process engineer would draw. Why are both suddenly called
engineering, and what is actually different about them? Then, one level down: the graph
crowd claims most production failures are failures of state, and loops have to carry
state between passes too. Is that the same problem?

## Summary

A loop says "do it again until done." A graph says "here is the flowchart, follow the
arrows." Every agent is already a loop on the inside, so the loop people talk about is a
second loop wrapped around the agent, and a graph is a loop where somebody drew the allowed
moves in advance. The two camps differ most in where they keep state, meaning whatever has to
survive from one step, one crash, or one context window to the next. Graph frameworks save a
snapshot of a shared state object after every step and replay from it. Loop builders keep
the state in plain files and git and make every pass start by reading them. Both run into
the same five failures: state that is lost, doubled, misread, bloated, or written by two
hands at once.

## Two loops, not one

The [workflows chapter](06-workflows-vs-agents) covers the inner loop: a model plans, calls
a tool, reads the result, and goes round again until it decides it is done. Every coding
agent runs that loop. Nobody drew the path; the model picks it as it goes.

The outer loop is newer and much simpler. FACT: Geoffrey Huntley's "Ralph Wiggum" post of
2025-07-14 describes the technique as, in its purest form, a Bash loop, `while :; do cat
PROMPT.md | claude-code ; done`, which re-runs the same prompt in a fresh context window
over and over ([Huntley, 2025](https://ghuntley.com/ralph/)). FACT: he insists on one item
per loop, because each loop has only about 170,000 tokens of context and the more it uses
the worse the outcomes get (Huntley, same post). FACT: he tells the loop to re-read the same
plan file and specifications at the start of every pass, which he calls deterministically
allocating the stack the same way every loop (Huntley, same post).

Two things in that post are the whole design. FACT: Huntley calls the checks that reject a
bad generation "back pressure": a type system, a build, a test suite, a security scanner,
anything that can be wired in to refuse invalid code (Huntley, same post). FACT: his one
condition on them is that the wheel has to turn fast (Huntley, same post). The second thing
is memory. FACT: because the next loop will not have this loop's reasoning in its context,
he has each loop write down why a test exists and what it is trying to do, "leaving little
notes for future iterations" (Huntley, same post). FACT: each loop commits to git when the
tests pass, and `git reset --hard` is his recovery when a loop leaves the code broken
(Huntley, same post).

Claude Code packages the outer loop. FACT: the `/loop` command re-runs a prompt while the
session stays open, either on a fixed interval or on one Claude chooses between a minute
and an hour based on what it saw last time
([Claude Code docs, "Run prompts on a schedule," read 2026-10-15](https://code.claude.com/docs/en/scheduled-tasks)).
FACT: the same page says scheduled tasks are session-scoped, recurring tasks expire seven
days after creation, and a self-paced `/loop` is not restored when a session is resumed
(Claude Code docs, same page). FACT: for scheduling that outlives a session the page points
to Routines, Desktop scheduled tasks or GitHub Actions (Claude Code docs, same page).

Assessment: the outer loop has very little design in it, which is the point. The work is in
three choices: what the stop condition is (something that cannot lie, never the model's
own "done"), what the cap is (iterations, tokens, or days), and what gets carried from one
pass to the next. That third choice is the subject of the rest of this chapter.

## What a graph actually is

A graph is a flowchart with a clipboard. FACT: in LangGraph, the framework that made the
picture standard, "nodes do the work, edges tell what to do next"
([LangGraph Graph API overview, read 2026-10-15](https://docs.langchain.com/oss/python/langgraph/graph-api);
API is application programming interface, the published way of talking to a piece of
software). FACT: the first thing you define is the `State`, a schema plus reducer
functions that say how each node's update is applied, where the default reducer replaces
a value and a custom one can append to a list (LangGraph Graph API overview). FACT: a
conditional edge calls a function to decide which node runs next, and the program advances
in discrete "super-steps," inspired by Google's Pregel system, ending when every node is
inactive and no messages are in transit (LangGraph Graph API overview). FACT: the same page
gives a graph a hard cap, the recursion limit on super-steps, set to 1,000 by default since
version 1.0.6 (LangGraph Graph API overview). FACT: it also provides a `Send` object for the
case where the number of edges is not known until runtime, such as fanning out over a list
a previous node produced (LangGraph Graph API overview).

Assessment: that is exactly the process-flow reading. Nodes are steps, edges are the
arrows, the state object is the traveler that carries the form from desk to desk, and a
conditional edge is the diamond on the flowchart. The one thing a process engineer would
not expect is that a node can be a model choosing among the outgoing edges. Two
consequences follow. A loop is a one-node graph with an edge back to itself, and the
recursion limit is its iteration cap. And a graph is a loop whose transitions were pinned
down before it ran, which is why the [workflows chapter](06-workflows-vs-agents) can call
the same two things "workflow" and "agent."

On the names. FACT: an Analytics Vidhya guide of 2026-07-28 defines graph engineering as
"the practice of representing an AI application as an executable graph containing agents,
tools, functions, policies, data systems, evaluators, and human decisions"
([Analytics Vidhya, 2026](https://www.analyticsvidhya.com/blog/2026/07/graph-engineering/)).
FACT: the same guide says graph engineering is not a replacement for prompt engineering,
context engineering, harness engineering or loop engineering (Analytics Vidhya, same
guide). FACT: a Shakudo explainer, undated on the page but written as a 2026 piece, defines
loop engineering as "the practice of writing outer control loops that drive AI agents
autonomously," borrowing the name from control systems engineering
([Shakudo, read 2026-10-15](https://shakudo.io/blog/loop-engineering)). **Unverified**
(2026-10-15): when the term was first used; the other explainers search turns up are dated
June and July 2026 and were not read
([Requesty](https://www.requesty.ai/blog/loop-engineering-how-to-build-ai-agent-loops-that-run-themselves)).
Assessment: both labels are 2026 blog coinages for two shapes of the same thing, the
harness around the model. The instinct that "everything can't be engineering" is fair. The
label with content under it is context engineering, covered in the
[context chapter](09-context-engineering); loop and graph are two answers to where the
context comes from each time.

## State: the thing both of them fight over

State is whatever has to survive a boundary. There are three boundaries that matter: from
one step to the next, across a crash or a restart, and across the end of a context window.
Graph builders and loop builders put the state in different places, and the failures that
follow are the same failures wearing different clothes.

**The graph answer: snapshot after every step.** FACT: LangGraph's checkpointers "persist
a thread's graph state as checkpoints," for conversation continuity, human-in-the-loop
workflows, time travel, and fault tolerance
([LangGraph persistence docs, read 2026-10-15](https://docs.langchain.com/oss/python/langgraph/persistence)).
FACT: a separate store holds data that must cross threads, such as user preferences
(LangGraph persistence docs). FACT: a thread is the accumulated state of a sequence of
runs, a checkpoint is the snapshot of that state saved at each super-step, and time travel
means replaying from a prior checkpoint or forking the state there to explore another path
([LangGraph checkpointers docs, read 2026-10-15](https://docs.langchain.com/oss/python/langgraph/checkpointers)).
FACT: replay skips the nodes before the chosen checkpoint and re-executes everything after
it, "including any LLM calls, API requests, or interrupts," which are always re-triggered
(LangGraph checkpointers docs).

FACT: checkpoints are saved at super-step boundaries, not mid-function inside a node, so
when execution stops and resumes, the affected node runs again from the start of its
function and "code and side effects before the pause run again" (LangGraph Graph API
overview). FACT: the docs therefore tell you to design node logic so re-execution does not
corrupt state, naming idempotency keys, upserts and read-before-write checks (LangGraph
Graph API overview). FACT: they also suggest wrapping the operations inside a node as
tasks, whose results are checkpointed so a resume can skip the ones already done (LangGraph
Graph API overview). FACT: if one node in a super-step fails, the writes of the nodes that
finished are kept, so they are not re-run on resume (LangGraph checkpointers docs). FACT:
the in-memory checkpointer loses everything on restart, checkpoints accumulate without
bound over a long conversation and need pruning, and a subgraph keeps its own checkpoint
namespace, so a parent graph may not see a subgraph's update (LangGraph persistence docs).

FACT: a guest post on Temporal's blog describes going one layer down: each agent thread is
mapped one-to-one to a Temporal Workflow and every LangGraph checkpoint becomes workflow
state ([Temporal blog, Manetu, undated, read 2026-10-15](https://temporal.io/blog/manetu-the-thread-is-the-workflow)).
FACT: when the worker running the agent crashes, Temporal re-hydrates the thread elsewhere
with its checkpoints, "without the agent process' involvement or knowledge" (Temporal blog,
same post). FACT: they keep only the latest LangGraph checkpoint, because Temporal's own
history is the record and has size limits (Temporal blog, same post). FACT: the post also
draws a line worth keeping: a human-in-the-loop resume and a crash-recovery resume look
identical from outside, both are "continue this thread," but a person's answer is new input
while a restarted worker has none (Temporal blog, same post). FACT: the graph must
therefore resume from its last checkpoint without a stale message forced into an
incomplete turn (Temporal blog, same post).

**The loop answer: the repository is the state.** FACT: Anthropic's engineering post on
long-running agents starts from the fact that each new session "begins with no memory of
what came before"
([Anthropic, "Effective harnesses for long-running agents"](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)).
FACT: it reports two failure modes: the agent tries to do too much at once and runs out of
context mid-feature, leaving the next session to guess, and a later session looks around,
sees progress, and declares the whole job done (Anthropic, same post). FACT: their fix is
an initializer agent that writes an `init.sh` to start the app, a `claude-progress.txt`
log, a feature list in JSON (JavaScript Object Notation, a plain structured text format)
with every feature marked as failing, and an initial git commit (Anthropic, same post).
FACT: every later session reads the progress file and the git log, runs a baseline
end-to-end test, works on one feature, and ends with a commit and a progress note
(Anthropic, same post). FACT: they chose JSON for the feature list because the model "is
less likely to inappropriately change or overwrite JSON files compared to Markdown files,"
and they tell it that removing or editing tests is unacceptable (Anthropic, same post).
FACT: the post also says plainly
that compaction was not enough on its own: it "doesn't always pass perfectly clear
instructions to the next agent," which is why the progress file exists (Anthropic, same
post).

That matters because compaction is lossy state. FACT: Anthropic's context engineering post
defines compaction as summarizing a conversation nearing the window limit and restarting
with the summary
([Anthropic, "Effective context engineering for AI agents"](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)).
FACT: it warns that "overly aggressive compaction can result in the loss of subtle but
critical context whose importance only becomes apparent later" (Anthropic, same post).
FACT: it pairs compaction with structured note-taking, notes written outside the window and
pulled back in later, with a `NOTES.md` file as the example (Anthropic, same post).

**What a production team learned.** FACT: Anthropic's account of its multi-agent research
system says agents "are stateful and errors compound," and that restarting from the
beginning after a failure is too expensive
([Anthropic, "How we built our multi-agent research system"](https://www.anthropic.com/engineering/multi-agent-research-system)).
FACT: so they built systems that resume from where the agent was, combining the model's
adaptability with "deterministic safeguards like retry logic and regular checkpoints"
(Anthropic, same post). FACT: because running agents may be anywhere in their process when
code is deployed, they use rainbow deployments that keep old and new versions running side
by side (Anthropic, same post). FACT: they evaluate the end state, broken into discrete
checkpoints "where specific state changes should have occurred," instead of judging every
intermediate step (Anthropic, same post).

**About that statistic.** Several 2026 explainers on graph engineering attribute a figure to
LangChain's State of Agent Engineering report: that over 60 percent of production incidents
trace back to state-management failures. FACT: the report's page, read on 2026-10-15,
reports 57.3 percent of respondents with agents in production, quality as the top barrier
for about a third of them, and observability in place at 89 percent of organizations
([LangChain, State of Agent Engineering](https://www.langchain.com/state-of-agent-engineering)).
FACT: that page contains no figure about incidents or about state management (LangChain,
same page).
**Unverified** (2026-10-15): the explainers that carry the number were not read in full,
only their search summaries, so where the figure came from is not known
([UD blog](https://ud.hk/en/blogs/insight/article/2026-08-21-graph-engineering-ai-agents)).
Assessment: treat the 60 percent as unsupported until somebody finds it in a primary
source. The claim behind it, that state is where agent systems break, is well enough
supported by the Anthropic and LangGraph material above without the number.

## The five ways state fails

Assessment, built only from the facts above. Every one of these has a graph version and a
loop version.

1. **Lost.** The snapshot was never durable: an in-memory checkpointer on a process that
   restarted, or a session that ended without writing its progress note. The next pass
   starts from nothing and guesses.
2. **Doubled.** A replay re-ran a side effect: a node that inserted a row runs again from
   its start after a resume, or a loop re-sends the email because the previous pass crashed
   after sending and before recording that it had. The cure in both camps is the same word,
   idempotent: make every step safe to run twice.
3. **Misread.** The resume fed the wrong thing in. Temporal's two meanings of "resume" is
   one form; a compaction summary that dropped the detail that turns out to matter is
   another; the later session that sees progress and declares victory is a third.
4. **Bloated.** The state grew until it was the problem: checkpoints that accumulate over a
   long thread, or a loop that burns most of its window re-reading specifications every
   pass. Pruning and one-item-per-loop are the same instinct.
5. **Split.** Two writers on one state: a subgraph writing to its own namespace while the
   parent reads the old value, or two sessions committing to one repository. Locks,
   namespaces and single-writer rules are the answer, and nobody finds this one until it
   happens.

All five get worse with length. The [many agents chapter](12-multi-agent-systems) carries
the arithmetic: when each step works 99 percent of the time, a hundred steps in a row work
about 37 percent of the time, and every one of the failures above is a step that did not
work.

## What this means for the vault's own loops

Assessment: the vault already runs one of each, and it is useful to say which is which.

The weekly newsletter is a graph. FACT: each issue week has one `state.json` with a fixed
schema, thirteen keys per article and five statuses, and a validator that prints one plain
sentence per fault (the vault's own state schema template, `90_meta/templates/state-schema.md`,
read 2026-10-15). FACT: a `.state.lock` file is held by the writer while it reads, changes
and writes the state, "so two drafting sessions finishing at the same moment under a
concurrency above 1 cannot lose one of the two entries" (state schema template, same
file). That is a reducer, a checkpoint and a single-writer lock, written in plain Python
before anybody here used the word graph.

The gauntlet is a loop around a graph. FACT: the standing rule here is that each critic
writes its verdict per round and each round ends with a digest file the next round reads
(`~/bin/session-digest … -o round-N-digest.md`, operator ruling of 2026-10-09, recorded in
the account-wide instructions file). That digest is the progress file: the only thing a
round carries to the next one, by design.

Assessment: five rules of thumb fall out, and none of them depends on which framework you
use. Decide where the state lives before you write the first step. Make every step safe to
run twice. Carry the reasoning, not only the result, because the next pass will not have
this pass's context. Put a cap on the loop that does not depend on the model's judgment.
Stop on something that cannot lie.

## Open questions

- Does the 60 percent figure appear anywhere in LangChain's full report, as opposed to its
  web page? Nobody here has read a PDF, if one exists.
- Date the term "loop engineering" properly. Shakudo's page carries no date in its
  metadata; find the first use, or drop the sentence that mentions the other explainers.
- Claude Code's Routines (the cloud scheduler) were not read; the chapter cites the
  scheduled-tasks page's pointer to them, nothing more.
- GenAI.mil, the Department's IL5 portal with Gemini, Grok and ChatGPT: can it run a loop or
  a graph at all, or only a chat? Being looked into separately (2026-10-15).

## Source ledger

| # | Source | Tier | AI-generated | Verified |
|---|--------|------|--------------|----------|
| 1 | Huntley, "Ralph Wiggum as a software engineer," 2025-07-14 | 4 | no | read 2026-10-15 |
| 2 | Claude Code docs, "Run prompts on a schedule," undated | 4 | no | read 2026-10-15 |
| 3 | LangGraph docs, Graph API overview, undated | 4 | no | read 2026-10-15 |
| 4 | LangGraph docs, Persistence, undated (the durable-execution address redirects here) | 4 | no | read 2026-10-15 |
| 5 | LangChain, State of Agent Engineering survey page, undated | 4 | no | read 2026-10-15 |
| 6 | Temporal blog (Manetu guest post), "The thread is the Workflow," undated | 4 | no | read 2026-10-15 |
| 7 | Analytics Vidhya, "Graph Engineering for AI Agents," 2026-07-28 | 4 | no | read 2026-10-15 |
| 8 | Anthropic, "Effective harnesses for long-running agents" (staged 2026-08-01) | 4 | no | read 2026-10-15 |
| 9 | Anthropic, "Effective context engineering for AI agents" (staged 2026-08-01) | 4 | no | read 2026-10-15 |
| 10 | Anthropic, "How we built our multi-agent research system" (staged 2026-08-01) | 4 | no | read 2026-10-15 |
| 11 | Anthropic, "Building effective agents" (staged 2026-08-01) | 4 | no | read 2026-10-15 |
| 12 | LangGraph docs, Checkpointers, undated | 4 | no | read 2026-10-15 |
| 13 | Shakudo, "What is Loop Engineering?", undated | 4 | likely | read 2026-10-15 |
| 14 | Requesty "loop engineering" post | 4 | likely | not read |
| 15 | UD blog, "graph engineering" (carries the 60 percent claim) | 4 | likely | not read |

## See also

- **In this series:** [← Running frontier-class models at home](22-running-frontier-models-at-home) · [Overview](topics/ai-engineering/)
- **[Workflows vs agents](06-workflows-vs-agents)**: the inner loop, and the workflow-versus-agent split a graph formalizes.
- **[Context](09-context-engineering)**: compaction and note-taking, the loop camp's state tools.
- **[Memory](08-memory-for-agents)**: the store side of the checkpointer-versus-store split.
- **[Many agents at once](12-multi-agent-systems)**: the compounding-error arithmetic.

## Sources

Staged under `10_staging/agent-loops-graphs-and-state/sources/` and
`10_staging/ai-engineering-sources/sources/`, read 2026-10-15.

- Geoffrey Huntley, *Ralph Wiggum as a "software engineer"*: https://ghuntley.com/ralph/
- Claude Code docs, *Run prompts on a schedule*: https://code.claude.com/docs/en/scheduled-tasks
- LangGraph, *Graph API overview*: https://docs.langchain.com/oss/python/langgraph/graph-api
- LangGraph, *Persistence*: https://docs.langchain.com/oss/python/langgraph/persistence
- LangGraph, *Checkpointers*: https://docs.langchain.com/oss/python/langgraph/checkpointers
- LangChain, *State of Agent Engineering*: https://www.langchain.com/state-of-agent-engineering
- Shakudo, *What is Loop Engineering?*: https://shakudo.io/blog/loop-engineering
- Temporal blog, *The thread is the Workflow*: https://temporal.io/blog/manetu-the-thread-is-the-workflow
- Analytics Vidhya, *Graph Engineering for AI Agents*: https://www.analyticsvidhya.com/blog/2026/07/graph-engineering/
- Anthropic, *Effective harnesses for long-running agents*: https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents
- Anthropic, *Effective context engineering for AI agents*: https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
- Anthropic, *How we built our multi-agent research system*: https://www.anthropic.com/engineering/multi-agent-research-system
- Anthropic, *Building effective agents*: https://www.anthropic.com/engineering/building-effective-agents
