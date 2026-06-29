---
title: "Thread: using AI well, without fooling yourself"
status: curated
tags: [connections, moc, verification, using-ai-well]
created: 2026-06-28
---

# Using AI well, without fooling yourself

This is the strongest and most surprising thread in the garden. Independently, in three
unrelated corners, the same thesis appears: a fluent, confident model will quietly
degrade your judgment unless you force it to expose its own downside. Treat it as a
drafting assistant, never as an oracle, and verify before you rely.

## The thread

- **[AI engineering: evaluation and testing](../topics/ai-engineering/11-evaluation-and-testing)**
  — how you actually know a model's output is good: golden datasets, LLM-as-judge (and
  its biases), and why you must validate the judge before trusting it.
- **[AI engineering: safety and best practices](../topics/ai-engineering/13-safety-and-best-practices)**
  — overconfidence and hallucination as named failure modes (treat model output as
  untrusted input), and the evaluator-optimizer loop that makes a model critique itself.
- **[Defense BD: using Gen AI without fooling yourself](../topics/business-development/defense-bd-playbook/10-gen-ai-in-bd)**
  — the same argument from a practitioner's seat: a powerful drafting assistant and a
  dangerous oracle, with experimental evidence that AI can degrade exactly the
  high-stakes judgment calls business development runs on.
- **[White's tree frogs: a bioactive build](../topics/pets/whites-tree-frog-bioactive-japan)**
  — the surprise. The terrarium note is a *worked example* of the discipline: it takes
  an AI's confident, detailed build plan and runs an adversarial pass over it, catching
  the contradictions and treating "very confident model numbers" as leads to verify, not
  gospel.

## Why it connects

One is written by an AI engineer worried about agent behavior, one by a defense
business-development practitioner worried about win-probability estimates, and one by a
hobbyist building a frog tank. None references the others, yet all three arrive at the
same move: make the AI argue against itself, and never let an unverified, confident claim
sit in your working state where it can quietly corrupt everything downstream. It is the
same instinct as this vault's own FACT / Assessment / Speculation labeling rule, applied
in the wild.

The deeper version of the thread, present in nearly every note here, is the **honest
catch**: the section where the author flags what is *not* known. The aircraft note's
"the covering is genuinely experimental," the MRAgent note's "promising but unproven
outside the authors' own runs," and the frog note's second look at the AI plan are all
the same habit. Browse the `verification` tag to find them.
