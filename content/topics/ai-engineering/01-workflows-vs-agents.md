---
type: lesson
series: ai-engineering
chapter: 1
title: Workflows vs agents
status: curated
tags: [ai, ai-agents, llm, agent-engineering, workflows]
created: 2026-06-28
---

# Chapter 1 — Workflows vs agents

The first decision in any LLM system is how much freedom to give the model. This
chapter draws the line between a **workflow** (the model runs on rails you laid down)
and an **agent** (the model lays its own rails), introduces the building block both
are made of, and shows the handful of patterns that cover most real systems.

## The umbrella term and the two categories

FACT: Anthropic uses "agentic systems" as the umbrella term and splits it into two
architectural categories. Workflows are "systems where LLMs and tools are
orchestrated through predefined code paths." Agents are "systems where LLMs
dynamically direct their own processes and tool usage, maintaining control over how
they accomplish tasks." (Anthropic, *Building Effective Agents*; the same definitions
are echoed in the LangGraph documentation.)

The practical difference is who decides the order of steps. In a workflow, you do, in
code, ahead of time. In an agent, the model does, at runtime, based on what it finds.

![Diagram: a spectrum from a single LLM call, to a workflow (LLM plus tools on predefined paths), to an agent (LLM directs its own steps), with an arrow from predictable-and-cheap to flexible-and-costly](img/spectrum.png)
*From a single call to an agent: more autonomy means more cost. Diagram.*

## Start simple

FACT: Anthropic's headline advice is to "find the simplest solution possible, and
only increase complexity when needed." Workflows give "predictability and consistency
for well-defined tasks," while agents are "the better option when flexibility and
model-driven decision-making are needed at scale," at the price of higher latency and
cost. (Anthropic.)

Assessment: the right default is a single LLM call with good retrieval and a few
in-context examples. Move to a workflow only when the task cleanly decomposes into
fixed steps. Move to an agent only when the steps genuinely cannot be known in
advance. Each step up the ladder buys flexibility and costs you tokens, latency, and
the ability to test the thing.

FACT: frameworks (LangGraph, Amazon Bedrock AgentCore, Rivet, Vellum) make it easy
to start but "add layers of abstraction that can obscure the underlying prompts and
responses, making them harder to debug." Anthropic recommends starting with the LLM
APIs directly and adding a framework only once you understand the code underneath.
(Anthropic.)

## The building block: the augmented LLM

FACT: the base unit of every agentic system is the "augmented LLM," a model enhanced
with three things: **retrieval** (it generates its own search queries), **tools** (it
selects and calls them), and **memory** (it decides what to retain). The model uses
these actively rather than having them bolted on passively. (Anthropic.) LangGraph
realizes the same idea concretely as structured outputs, tool calling, and short-term
memory.

Everything else in this wing is an elaboration of those three augmentations.

## The five workflow patterns

FACT: most workflows are one of five patterns (Anthropic, corroborated by LangGraph
and OpenAI, so treat them as cross-vendor consensus rather than one vendor's house
style):

1. **Prompt chaining.** "Decomposes a task into a sequence of steps, where each LLM
   call processes the output of the previous one." You can add a programmatic check
   (a "gate") between steps. Use it when a task splits cleanly into fixed subtasks; it
   trades latency for accuracy.
2. **Routing.** "Classifies an input and directs it to a specialized followup task."
   This lets each category get its own specialized prompt. Use it when inputs fall
   into distinct kinds best handled separately (for example, refund requests versus
   technical questions).
3. **Parallelization.** Several LLM calls run at once and their outputs are combined
   in code. Two flavors: *sectioning* breaks a task into independent subtasks run in
   parallel, and *voting* runs the same task several times for higher confidence. Use
   it for speed, or when multiple perspectives raise confidence.
4. **Orchestrator-workers.** "A central LLM dynamically breaks down tasks, delegates
   them to worker LLMs, and synthesizes their results." The key difference from
   parallelization: the subtasks are not pre-defined, the orchestrator decides them
   from the input. Use it for complex tasks where you cannot predict the subtasks in
   advance, such as edits spread across many files.
5. **Evaluator-optimizer.** "One LLM call generates a response while another provides
   evaluation and feedback in a loop." Use it when you have clear evaluation criteria
   and iterative refinement measurably helps, the machine equivalent of draft and
   revise.

Assessment: routing and prompt chaining cover a surprising amount of production work
on their own. Orchestrator-workers is the pattern that shades into multi-agent
systems (see [chapter 7](07-multi-agent-systems)); the only real difference is whether
the workers are full agents.

## The agent loop

When you do hand control to the model, what it does is run a loop.

FACT: Anthropic's one-line definition of an agent is an LLM "autonomously using tools
in a loop." (Anthropic, *Effective Context Engineering*.) The loop begins from a
command or discussion with a user, then the agent "plans and operates independently,
potentially returning to the human for further information or judgement." Crucially,
at each step it "must gain 'ground truth' from the environment (such as tool call
results or code execution) to assess its progress." (Anthropic, *Building Effective
Agents*.)

![Diagram: a three-node cycle, reason/plan to call tool(s) to observe results and back, repeating until done or stopped; stops on task done, max steps, or a human checkpoint](img/agent-loop.png)
*The agent loop. Diagram.*

FACT: the loop needs **stopping conditions**. Agents "can pause for human feedback at
checkpoints or when encountering blockers," and "you can typically use stopping
conditions, such as a maximum number of iterations, to maintain control." Termination
is usually completion of the task or a checkpoint. (Anthropic.)

FACT: because errors and costs can compound across iterations, agents need "extensive
testing in sandboxed environments, along with the appropriate guardrails." (Anthropic.)
That discipline is the subject of [chapter 6](06-evaluation-and-testing) and
[chapter 8](08-safety-and-best-practices).

## Sources

- Anthropic, *Building Effective Agents* — https://www.anthropic.com/engineering/building-effective-agents
- Anthropic, *Effective Context Engineering for AI Agents* — https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
- LangChain / LangGraph, *Workflows and agents* — https://docs.langchain.com/oss/python/langgraph/workflows-agents
