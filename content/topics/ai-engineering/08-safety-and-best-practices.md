---
type: lesson
series: ai-engineering
chapter: 8
title: Safety and best practices
status: curated
tags: [ai, ai-agents, llm, agent-engineering, safety, security]
created: 2026-06-28
---

# Chapter 8 — Safety and best practices

An agent that can send email, run code, and touch databases is software with real
blast radius, and a model that can be talked into doing the wrong thing. This chapter
covers the standard security checklist for LLM applications, the failure modes that
matter most once an agent has real permissions, the guardrails that contain them, and
a consolidated set of best practices pulled from across the wing.

## The OWASP Top 10 for LLM applications

FACT: the OWASP GenAI Security Project's 2025 Top 10 for LLM applications is:
`LLM01 Prompt Injection`, `LLM02 Sensitive Information Disclosure`, `LLM03 Supply
Chain`, `LLM04 Data and Model Poisoning`, `LLM05 Improper Output Handling`, `LLM06
Excessive Agency`, `LLM07 System Prompt Leakage`, `LLM08 Vector and Embedding
Weaknesses`, `LLM09 Misinformation`, and `LLM10 Unbounded Consumption`. (OWASP,
genai.owasp.org.)

The four that bite agent builders hardest:

**Prompt injection (LLM01).** FACT: attackers manipulate inputs to override the
original instructions. There are two forms, *direct* injection (in the user's prompt)
and *indirect* injection (hidden instructions inside external content the model
ingests: a document, web page, or email). The consequences are worse in agentic
systems because the model "may be able to send emails, access databases, modify files,
create tickets, call APIs, or trigger business workflows." (OWASP 2025.) Assessment:
there is no complete fix; defenses are layered (input and output filtering, privilege
separation, treating all retrieved content as untrusted, and human approval for
high-impact actions).

**Improper output handling (LLM05).** FACT: passing model output to downstream systems
without validation enables injection, server-side request forgery, or remote code
execution. The fix is the Zero Trust principle: "treat LLM outputs as untrusted user
input," encode before rendering, and parameterize queries instead of concatenating
model text into SQL. (OWASP 2025.)

**Excessive agency (LLM06).** FACT: "an LLM-based system is often granted a degree of
agency"; excessive functionality, permissions, or autonomy lets an influenced model
take harmful actions. (OWASP 2025.) Assessment: this is the risk that matters most as
agents move into production. Mitigate by minimizing tools and permissions (least
privilege), constraining each tool's scope, and requiring human approval for
consequential or irreversible actions.

**Unbounded consumption (LLM10).** FACT: unrestricted resource use causes excessive
cost and operational disruption, including "denial of wallet." (OWASP 2025.) The
concrete driver here is the roughly fifteen-fold token cost of multi-agent systems and
the "50 subagents" failure from [chapter 7](07-multi-agent-systems). Assessment:
mitigate with token and step budgets, rate limits, maximum-subagent caps, and timeouts.

## Hallucination and overconfidence (LLM09)

FACT: OWASP notes that "misinformation from LLMs poses a core vulnerability for
applications relying" on factual accuracy. (OWASP 2025.) Assessment: models express
high confidence on wrong answers. The mitigations are grounding and retrieval (see
[chapter 5](05-retrieval-and-rag)), a citation requirement with citation-accuracy
checks, verification steps, and schema-constrained decoding. This is exactly why the
LLM-as-judge rubric in [chapter 6](06-evaluation-and-testing) scores factual accuracy
and citation accuracy separately.

## Guardrails, observability, and humans in the loop

Assessment: the standard structure is a layer of *input guardrails* (block injection,
PII, off-topic requests) and *output guardrails* (filter, check relevance and
groundedness, validate against a schema) wrapped around the model. A strong pattern is
a self-correction loop, where a failed response is revised mid-execution rather than
returned. Reserve human approval for high-impact or irreversible tool actions, the
standard mitigation for excessive agency.

FACT: Anthropic's evaluator-optimizer workflow is the canonical guardrail-by-loop, one
LLM generates while another evaluates and feeds back "when we have clear evaluation
criteria." For observability, Anthropic monitors "agent decision patterns and
interaction structures, all without monitoring the contents of individual
conversations, to maintain user privacy," and built systems "that can resume from
where the agent was" rather than restarting. (Anthropic, *Building Effective Agents*
and *multi-agent research system*.) Assessment: tracing (capturing each step's
prompts, tool calls, latencies, and costs) is the prerequisite for debugging
non-deterministic agents at all.

## A consolidated checklist

Assessment: pulling the wing together, the practices that hold up across the sources:

1. **Start simple.** A single call beats a workflow you do not need; a workflow beats
   an agent you cannot evaluate ([chapter 1](01-workflows-vs-agents)).
2. **Invest in tool design.** Clear names, sharp "use this when" boundaries,
   mistake-proofed arguments, concise results ([chapter 2](02-tools-and-mcp)).
3. **Engineer the context.** Treat the window as finite; compact, take notes, and use
   sub-agents to keep it clean ([chapter 4](04-context-engineering)).
4. **Ground your facts.** Use retrieval for knowledge that changes or must be cited,
   and enforce citations ([chapter 5](05-retrieval-and-rag)).
5. **Evaluate continuously.** Keep a golden set, validate your judge, score both path
   and end-state ([chapter 6](06-evaluation-and-testing)).
6. **Keep chains short.** Compounding error is real; add checkpoints and verify the
   end-state ([chapter 7](07-multi-agent-systems)).
7. **Apply least privilege.** Minimize tools and permissions, treat all model output
   and retrieved content as untrusted, and gate consequential actions on a human.
8. **Bound consumption.** Token, step, and subagent budgets, plus timeouts.

## Sources

- OWASP GenAI Security Project, *Top 10 for LLM Applications (2025)* — https://genai.owasp.org/llm-top-10/
- Anthropic, *Building Effective Agents* — https://www.anthropic.com/engineering/building-effective-agents
- Anthropic, *How we built our multi-agent research system* — https://www.anthropic.com/engineering/multi-agent-research-system
- Datadog, *LLM guardrails best practices* — https://www.datadoghq.com/blog/llm-guardrails-best-practices/
