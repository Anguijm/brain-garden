---
type: lesson
series: ai-engineering
chapter: 4
title: Context engineering
status: curated
tags: [ai, ai-agents, llm, agent-engineering, context-engineering]
created: 2026-06-28
---

# Chapter 4 — Context engineering

Context engineering is the practice of deciding what goes into the model's window on
each call, and what to leave out. It is the successor skill to prompt engineering: not
just writing a good system prompt once, but managing the whole context state, turn
after turn, as a system runs. This chapter explains why more context is not better,
the failure mode called context rot, and the techniques for keeping the window clean.

## What it is, and why it is finite

FACT: Anthropic defines context engineering as "the set of strategies for curating and
maintaining the optimal set of tokens (information) during LLM inference." It
supersedes prompt engineering, which is just writing the prompt; context engineering
manages "the entire context state (system instructions, tools, MCP, external data,
message history, etc.)" across many turns. (Anthropic, *Effective Context Engineering
for AI Agents*.)

FACT: context "must be treated as a finite resource with diminishing marginal
returns." LLMs have a finite "attention budget" that depletes with every token,
analogous to human working memory. (Anthropic.)

## Context rot

The central reason to manage context carefully is that long contexts quietly degrade
the model.

FACT: context rot is the effect where "as the number of tokens in the context window
increases, the model's ability to accurately recall information from that context
decreases." The mechanism: transformers produce "n-squared pairwise relationships for
n tokens," so as context grows the ability to capture those relationships "gets
stretched thin," and models have "fewer specialized parameters for context-wide
dependencies" because their training sequences were mostly shorter. (Anthropic.)

![Diagram: a curve of recall versus tokens in context, high and sharp early, declining and unreliable late, with the cause noted as n-squared attention stretched thin and a depleting attention budget](img/context-rot.png)
*Context rot: recall falls as the window fills. Diagram.*

FACT: Chroma's *Context Rot* study tested 18 models (including Claude 4, GPT-4.1,
Gemini 2.5, and Qwen3) and found that "model performance varies significantly as input
length changes, even on simple tasks," and that models "do not use their context
uniformly." It also showed that the popular "needle in a haystack" test only measures
simple lexical retrieval and overstates real long-context ability; on realistic tasks,
"even a single distractor reduces performance," and "adding four distractors compounds
this degradation." (Chroma.)

Assessment: the widely repeated "lost in the middle" claim, that models recall the
start and end of a long context far better than the middle, is real in the
long-context literature, but the specific figures that circulate (such as "30% lower
in the middle") are secondary-source. Chroma's own position test actually found no
notable variation for that particular task, so position effects are task-dependent.
The safe takeaway is the general one: do not assume the model reliably uses everything
you put in the window.

## The techniques

FACT: Anthropic describes a standard toolkit for keeping context clean (each is theirs
unless noted):

- **Compaction.** "Taking a conversation nearing the context window limit, summarizing
  its contents, and reinitiating a new context window with the summary." The goal is a
  high-fidelity distillation so the agent continues with minimal degradation.
- **Structured note-taking** (scratchpads). "The agent regularly writes notes
  persisted to memory outside of the context window," then pulls them back later. This
  gives "persistent memory with minimal overhead."
- **Sub-agent architectures.** Specialized sub-agents "handle focused tasks with clean
  context windows" and return "only a condensed, distilled summary" (Anthropic cites
  roughly 1,000 to 2,000 tokens), giving a clear separation of concerns.
- **Just-in-time retrieval.** Agents hold "lightweight identifiers (file paths, stored
  queries, web links)" and "dynamically load data into context at runtime using
  tools," rather than pre-loading everything. The trade-off: "runtime exploration is
  slower than retrieving pre-computed data."

FACT: Anthropic also describes operational features for this: context editing
(rule-based pruning of stale tokens), context awareness (telling the model how much
capacity remains), persistent memory tools, and, for long-running agents, keeping
"context management files alongside version control history" so a fresh window can
quickly recover the work state. (Anthropic engineering posts.)

## The rule of thumb

Assessment: more tokens is not better. The job is to maximize signal-to-noise in the
window; a clean context beats a large one. This matters most for coding and research
agents, where accumulated search results and backtracking are the main source of
noise, and where compaction plus note-taking plus sub-agents are the standard fix.
The same insight motivates both [retrieval](05-retrieval-and-rag) (pull in only the
relevant facts) and [multi-agent systems](07-multi-agent-systems) (give each agent its
own clean window).

## Sources

- Anthropic, *Effective Context Engineering for AI Agents* — https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
- Anthropic, *Effective Harnesses for Long-Running Agents* — https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents
- Chroma, *Context Rot: How Increasing Input Tokens Impacts LLM Performance* — https://www.trychroma.com/research/context-rot
