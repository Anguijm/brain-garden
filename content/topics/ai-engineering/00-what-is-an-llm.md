---
type: lesson
series: ai-engineering
chapter: 0
title: "Start here: what is an AI model?"
status: curated
tags: [ai, ai-agents, llm, agent-engineering, beginner-friendly]
created: 2026-06-29
---

# Start here: what is an AI model?

This is the ground floor. If words like "model," "token," or "prompt" are fuzzy to
you, read this first. The rest of this section will make a lot more sense after it.
You do not need any computer background.

## The one big idea: it guesses the next word

A large language model (LLM) is a computer program that works with words. "Large"
just means it is big. "Language model" means it deals with language.

Here is the whole trick. The model guesses the next word. Then it guesses the word
after that. Then the next. One word at a time, over and over, until it has written a
full answer.

That sounds too simple to be useful. But it turns out that if a program gets really,
really good at guessing the next word, it can answer questions, write emails, explain
ideas, and more. Think of someone who has read a huge amount of text and has gotten
very good at finishing your sentences.

![Diagram: a prompt goes into the model; the model guesses the next word, adds it, and repeats, building up an answer one word at a time](img/how-llm-works.png)
*A model builds an answer one guessed word at a time. Diagram.*

## How it learned: training

The model got good at guessing by reading. A lot.

FACT: before you ever use it, the model is "trained." Training means it is shown an
enormous amount of text and slowly adjusts itself to predict text better. (This is the
standard description of how these models are built.)

Two things matter for you:
- The model learned all of this **ahead of time**. It is not reading the live internet
  while it talks to you (unless it is given a search tool, which we cover later).
- By default it does **not** remember your past chats. Each new chat starts fresh. We
  come back to this in the [memory chapter](03-memory-for-agents).

## A few words you will keep seeing

You only need a handful of terms to follow everything else.

- **Model.** The program itself. The "brain" that does the guessing. Newer models are
  better at it. (Claude is one family of models.)
- **Prompt.** What you type to the model. Your question plus any background you give
  it. Better prompts get better answers, which is its own small skill.
- **Token.** Models do not read whole words. They read small chunks called tokens.
  FACT: one token is about three-quarters of a word, very roughly four characters of
  English. So "tokenize" might be two tokens: "token" and "ize." You do not need to
  count them, but two facts matter: you usually **pay per token**, and there is a
  **limit** on how many a model can handle at once.
- **Context window.** This is how much the model can read at one time: your prompt plus
  everything in the chat so far. Picture a desk that only fits so many papers. Once the
  desk is full, something has to come off. The size of that desk is the context window.

## Three limits to always keep in mind

Almost everything else in this section exists to work around these three limits. If
you remember nothing else, remember these.

1. **It can be wrong and still sound sure.** The model guesses. Most of the time the
   guess is good. Sometimes it is wrong but the writing sounds just as confident.
   FACT: making up something false but believable is called a **hallucination**. This
   is why you check important answers against a real source. (More in the
   [safety chapter](08-safety-and-best-practices).)
2. **It forgets between chats.** Start a new chat and it does not know what you said
   yesterday, unless you tell it again or the app stores it for you.
3. **It can only read so much at once.** That is the context window from above. Hand it
   a giant pile of text and it starts to lose track of the middle. (More in the
   [context chapter](04-context-engineering).)

## Why this matters for the rest of this section

Once you see those limits, the rest of this section is easy to place. Each part is a
way to give the model what it cannot do on its own:

- **[Tools](02-tools-and-mcp)** let it *do things* (search the web, run code, look
  something up), instead of only writing.
- **[Memory](03-memory-for-agents)** lets it *remember* across chats.
- **[Context engineering](04-context-engineering)** keeps its "desk" tidy so it does
  not lose track.
- **[Retrieval](05-retrieval-and-rag)** feeds it the right facts so it does not have to
  guess from memory.

Assessment: a useful way to read this whole section is "the model, plus help." The
model writes and reasons. Everything else helps it act, remember, and stay accurate.

## Sources

- Anthropic, *Introduction to Claude / models overview* — https://docs.anthropic.com/en/docs/about-claude/models
- Anthropic, *Token counting and context windows* — https://docs.anthropic.com/en/docs/build-with-claude/context-windows
