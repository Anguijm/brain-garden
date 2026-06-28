---
type: lesson
series: ai-engineering
chapter: 2
title: Tools and MCP
status: curated
tags: [ai, ai-agents, llm, agent-engineering, tools, mcp]
created: 2026-06-28
---

# Chapter 2 — Tools and MCP

A model on its own can only produce text. Tools are how it acts on the world:
searching, reading files, querying databases, calling APIs. This chapter covers the
mechanics of tool use (often called function calling), how to design tools the model
can actually use well, and the Model Context Protocol (MCP) that standardizes the
wiring between apps and tools.

## How tool use works

FACT: "Tool use lets Claude call functions that you define." You define a tool with a
`name`, a `description`, and an `input_schema` (a JSON Schema object). The cycle for a
client-side tool is: the model returns `stop_reason: "tool_use"` with one or more
`tool_use` blocks (each carrying an `id`, the tool `name`, and the `input`); your code
executes the function and sends back a `tool_result` block referencing the
`tool_use_id`; and the exchange repeats until the model answers without calling a tool.
(Claude Developer docs, *Tool use with Claude*.)

![Diagram: the model emits a tool_use block, your code runs the function and returns a tool_result, and the exchange repeats until the model answers](img/tool-cycle.png)
*The tool-use request and result cycle. Diagram.*

FACT: you control whether the model uses tools with `tool_choice`: `auto` (the
default, model decides), `any` (must call some tool), `tool` (must call a named one),
or `none`. The `auto` and `none` options cost fewer prompt tokens than `any` and
`tool`. (Claude docs.)

FACT: tools run in one of two places. **Client tools** run in your application, you
execute them and return the result. **Server tools** (such as web search, web fetch,
and code execution) run on the provider's infrastructure and return results directly.
A model can also emit several `tool_use` blocks in one response (parallel tool use),
and "strict" modes can guarantee the call conforms to the schema. (Claude docs;
OpenAI's `strict: true` enforces the same, requiring `additionalProperties: false`
and all properties listed in `required`.)

## Designing tools the model can use

The mechanics are easy. The hard, high-leverage part is the tool design.

FACT: Anthropic calls this the "Agent-Computer Interface" (ACI) and says to invest in
it "just as much effort as you would normally devote to" human-computer interfaces.
The principles: give the model enough tokens to think; keep input and output formats
"close to what the model has seen naturally occurring in text on the internet"; avoid
formatting overhead like manual line-counting or heavy string-escaping; document each
tool like "a great docstring for a junior developer" with example usage, edge cases,
and clear boundaries against other tools; and apply *poka-yoke* (mistake-proofing),
changing arguments so errors are hard to make, for example by requiring absolute file
paths. (Anthropic, *Building Effective Agents*.)

FACT: tools should be "self-contained, robust to error, and extremely clear with
respect to their intended use." The recurring failure mode is "bloated tool sets"
that create "ambiguous decision points about which tool to use." (Anthropic, *Context
Engineering*.)

Assessment: across vendors, the single biggest driver of reliable tool-calling is the
*description*, a clear name, a sharp "use this when..." boundary, and well-documented
parameters matter more than the schema mechanics. Return concise, structured results
rather than raw dumps. When a tool fails, pass the error back to the model as a
`tool_result` (often flagged as an error) and let it recover; OpenAI's guidance is the
same: "provide the error reported by the tool to the model and it will figure out what
to do." For very large tool inventories, group tools into namespaces or expose them
through an MCP server rather than dumping hundreds of flat function definitions.

## The Model Context Protocol (MCP)

Once you have tools, you face an integration problem: every app needs to connect to
every tool. MCP is the standard that collapses that.

FACT: MCP is "an open-source standard for connecting AI applications to external
systems" (data sources, tools, workflows). The documentation's analogy is "a USB-C
port for AI applications." Its scope is deliberately narrow: "MCP focuses solely on
the protocol for context exchange, it does not dictate how AI applications use LLMs or
manage the provided context." (modelcontextprotocol.io.)

![Diagram: an MCP host (the AI app) holding several MCP clients, each with a 1:1 connection to an MCP server (file system, database, web/API); M apps times N tools becomes M plus N](img/mcp.png)
*MCP's client-server architecture. Diagram.*

FACT: the architecture has three participants. The **MCP Host** is the AI application
that coordinates one or more clients (Claude Code, Claude Desktop, VS Code). An **MCP
Client** maintains a dedicated one-to-one connection to a single server. An **MCP
Server** is a program that provides context, running locally or remotely. The host
creates one client per server. (modelcontextprotocol.io.)

FACT: servers expose three kinds of primitive. **Tools** are "executable functions
that AI applications can invoke to perform actions" (discovered via `tools/list`,
called via `tools/call`). **Resources** are "data sources that provide contextual
information" (file contents, database records). **Prompts** are "reusable templates
that help structure interactions with language models." Clients can in turn expose
*sampling* (the server asks the host for an LLM completion), *elicitation* (the server
asks the user for input), and *logging*. (modelcontextprotocol.io.)

FACT: under the hood MCP uses JSON-RPC 2.0 over two transports: `stdio` for local
processes and "Streamable HTTP" for remote servers (with bearer tokens or API keys;
the docs recommend OAuth). It is a stateful protocol: an `initialize` request
negotiates the protocol version and capabilities. (modelcontextprotocol.io.)

Assessment: MCP's value is turning an M-times-N integration problem (M apps, N tools)
into M-plus-N, each tool ships one server, each app ships one client. To the model,
calling a tool defined inline and calling one exposed through an MCP server look the
same (a name, a description, a JSON Schema); MCP adds discovery, dynamic updates, and
reuse across hosts. It has become the de facto interoperability layer for agent
tooling in 2025 and 2026.

## Sources

- Claude Developer docs, *Tool use with Claude* — https://platform.claude.com/docs/en/docs/build-with-claude/tool-use/overview
- Anthropic, *Building Effective Agents* — https://www.anthropic.com/engineering/building-effective-agents
- Anthropic, *Effective Context Engineering for AI Agents* — https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
- Model Context Protocol, *Introduction* and *Architecture* — https://modelcontextprotocol.io/introduction and https://modelcontextprotocol.io/docs/learn/architecture
- OpenAI, *Function calling* — https://developers.openai.com/api/docs/guides/function-calling
