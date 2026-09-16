---
title: Running frontier-class models at home
tags: [ai-engineering, local-llm, hardware, strix-halo, apple-silicon, home-lab]
created: 2026-09-16
---

# Running frontier-class models at home

Can a machine under your desk give you frontier-model answers without paying by the
token? As of September 2026 the honest answer is: not quite frontier, but for the first
time the gap is one model generation rather than an era, and the hardware that closes
most of it ships this month. This note records the state of play, what our own machine
can do today, and a purchase decision with a date on it. Everything here comes from a
three-way research pass run on 2026-09-16, with pages read rather than search snippets
trusted; the working record is in staging.

## How close the open models actually are

The models you can download and run at home are now about one generation behind the
paid frontier. FACT: On the LMArena community leaderboard as read 2026-09-16, the
top ten models are all closed, led by claude-fable-5.1 at a score of 1506; the best
open-weight model, GLM-5.3, ranks 19th at 1483 ([arena.ai/leaderboard](https://arena.ai/leaderboard)).
FACT: On the same date, Artificial Analysis's intelligence index scored the best
open model 45 against 53 for the frontier tier ([artificialanalysis.ai/models](https://artificialanalysis.ai/models)).
Unverified: in LMArena's coding-specific WebDev arena, the research pass read open
models qwen3.8-max and kimi-k3-max at 4th and 5th overall on 2026-09-16; that subpage of
[arena.ai/leaderboard](https://arena.ai/leaderboard) renders only in a browser, so no copy
could be staged for checking.

Assessment: Benchmarks flatter the open models slightly. Practitioners running
GLM-5.3-class models inside coding agents report the same pattern across many accounts:
the model does most of what the frontier does, but takes more turns to get there,
generates far more tokens (one measured 2.3 times the tokens for the same task), and
misses the edge cases the frontier model catches. The community shorthand is that the
best open models "feel like" the frontier of six to twelve months ago. For drafting,
research, summarization and well-defined coding, that is now genuinely useful. For the
hardest multi-step judgment work, the frontier still earns its fee.

Assessment: This is a real change from 2024, when open models were unusable for
serious work. Two things moved: the leading open labs (DeepSeek, Alibaba's Qwen, Zhipu's
GLM, Moonshot) now ship at or near frontier scale under genuinely open licenses, and the
mixture-of-experts design became standard. A mixture-of-experts model stores hundreds of
billions of parameters but activates only a small fraction per word generated, which is
what makes big-model quality reachable on home memory bandwidth.

## The number that decides everything: memory bandwidth

For one person chatting with one model, the speed you experience is set almost entirely
by how fast the machine can move the model's weights through the processor, which means
memory bandwidth, not processor speed. A useful rule: generated tokens per second is
roughly bandwidth divided by the bytes each token must touch. That is why a
mixture-of-experts model with few active parameters runs fast even on modest hardware,
and why a dense 70-billion-parameter model crawls everywhere outside a datacenter.

The current options, with measured speeds on the same test model (gpt-oss-120b, a
120-billion-parameter mixture-of-experts model) where measurements exist:

| Machine | Memory | Bandwidth | gpt-oss-120b, tokens/sec | Price today |
|---|---|---|---|---|
| Our Strix Halo (MS-S1 MAX) | 64 GB | 256 GB/s | ~34 (on 128 GB boxes, same chip) | owned |
| NVIDIA DGX Spark | 128 GB | 273 GB/s | 38.6 | \$4,699 |
| Mac Studio M5 Max, 40-core GPU | up to 128 GB | 614 GB/s | 87.9 | from \$2,499 (base) |
| Mac Studio M3 Ultra (2025) | up to 512 GB | 819 GB/s | not staged | used market dry |
| Mac Studio M5 Ultra | 96 to 512 GB | 1,229 GB/s | not yet benchmarked | from \$5,499, ships 2026-09-22 |

FACT: Apple introduced the M5 Max and M5 Ultra Mac Studios on 2026-08-25, with
availability 2026-09-22 and the 512 GB configuration following in late October; the
M5 Ultra starts at \$5,499 with 96 GB, and every M5 Ultra memory configuration runs at
1.2 TB/s ([Apple newsroom](https://www.apple.com/newsroom/2026/08/apple-introduces-new-mac-studio-with-m5-max-and-m5-ultra/),
[Apple's spec page](https://www.apple.com/mac-studio/specs/)). There was no M4 Ultra;
Apple skipped the generation. Unverified: Apple's configure-to-order prices for the
256 GB and 512 GB tiers did not render through our fetcher; MacRumors reports the 512 GB
model is "expected to start well above the \$10,000 mark"
([MacRumors, 2026-08-25](https://www.macrumors.com/2026/08/25/mac-studio-m5-ultra-512gb-ram-october/)).

FACT: An M5 Max measured 87.9 tokens/sec generating with gpt-oss-120b, and 1,325
tokens/sec reading the prompt
([hardware-corner.net, 2026-03-11](https://www.hardware-corner.net/m5-max-local-llm-benchmarks-20261233/)).
FACT: Apple builds a neural accelerator into every GPU core of the M5 generation
([Apple newsroom](https://www.apple.com/newsroom/2026/08/apple-introduces-new-mac-studio-with-m5-max-and-m5-ultra/)).
Assessment: those accelerators are why M5 speeds run ahead of what bandwidth alone
predicts, and why the Ultra's real speed needs measuring rather than extrapolating.

FACT: NVIDIA's DGX Spark, the \$4,699 128 GB desktop box, generates at 38.55
tokens/sec on the same model, barely above our existing machine, because its 273 GB/s
bandwidth is barely above ours; its strength is reading long prompts (1,723 tokens/sec),
not writing ([IntuitionLabs review, updated 2026-08-12](https://intuitionlabs.ai/articles/nvidia-dgx-spark-review)).

## What fits in what memory

Model weights have to sit in memory, so memory size decides which models you can run at
all, and the 4-bit compressed versions (quantizations) are the practical floor for
quality. **FACT** (sizes read from the community build repositories via the Hugging Face
API, 2026-09-16): the standout models by memory tier:

- **64 GB (our machine):** Qwen3.8-27B, the community's default local workhorse
  (Apache 2.0 licensed, about 16.5 GB at 4-bit), and gpt-oss-120b at roughly 60 GB,
  which fits but leaves little headroom for context.
- **128 GB:** [Qwen3.8-Flash-Next](https://huggingface.co/Qwen/Qwen3.8-Flash-Next)
  at 4-bit (93.7 to 111.3 GB), a 180-billion-parameter model that activates only
  6 billion per token, reported around 22 tokens/sec on 128 GB Strix Halo machines.
- **192 to 256 GB:** GLM-5.3-Flash at 4-bit (156.8 to 199.7 GB), the model practitioners
  describe as the credible daily driver one generation behind frontier.
- **512 GB:** [DeepSeek-V4.1-Flash](https://huggingface.co/deepseek-ai/DeepSeek-V4.1-Flash)
  (released 2026-09-10, MIT licensed, 1M-token context), which streams part of itself
  from disk and needs roughly 294 GB resident at 4-bit.

Assessment: The 256 GB tier is where "one generation behind frontier" starts being
something you run rather than rent. Below it you are running good-but-smaller models;
above it you are paying Apple's largest memory premium for models that are still
datacenter-first.

## What our existing machine can do today, corrected

Our MS-S1 MAX with 64 GB was undersold in this vault's earlier thinking, and also
oversold in one way: the right models make it useful, the wrong ones make it look
broken. FACT: Dense 70-billion-parameter models generate at roughly 5 tokens/sec on
Strix Halo, which is unusable for interactive work; mixture-of-experts models are the
entire game, with Qwen3-30B-class models at about 72 tokens/sec
([Level1Techs benchmark thread](https://forum.level1techs.com/t/strix-halo-ryzen-ai-max-395-llm-benchmark-results/233796)).

FACT: Run llama.cpp directly with the Vulkan backend, not Ollama: Ollama's bundled
copy of llama.cpp is months old and misses two 2026 AMD performance fixes, leaving about
34 tokens/sec where standalone llama.cpp reaches 52 to 56 on identical Strix Halo
hardware ([ollama/ollama issue #15601](https://github.com/ollama/ollama/issues/15601)).

FACT: The local tooling now speaks Anthropic's own API format, so Claude Code can
point at a local model by setting an environment variable: LM Studio documents the setup
([lmstudio.ai/blog/claudecode](https://lmstudio.ai/blog/claudecode)), and Ollama's blog
states it "is now compatible with the Anthropic Messages API, making it possible to use
tools like Claude Code with open models" ([ollama.com/blog](https://ollama.com/blog)). Assessment:
This means the trial costs nothing: we can run Qwen3.8-27B on the machine we already own,
point a coding session at it, and measure whether the quality serves before spending a
dollar on new hardware.

## Two machines, or one big one

Buying two machines and clustering them is now technically real and almost never the
right call. FACT: Since macOS 26.2 (December 2025) Macs can pool memory over
Thunderbolt 5 with remote direct memory access, and in the canonical test four M3 Ultra
Mac Studios ran the 671-billion-parameter DeepSeek V3.1 at 32.5 tokens/sec, two at 27.8,
one at 21.1 (tests by [Jeff Geerling, 2025-12-18](https://www.jeffgeerling.com/blog/2025/15-tb-vram-on-mac-studio-rdma-over-thunderbolt-5/);
figures as reported in [AppleInsider's writeup, 2025-12-20](https://appleinsider.com/articles/25/12/20/ai-calculations-on-mac-cluster-gets-a-big-boost-from-new-rdma-support-on-thunderbolt-5)).
Assessment: Doubling the hardware bought 32 percent more speed. Clustering earns its
complexity only when a model literally does not fit in the biggest single machine you
can buy, which today means the 600 GB-plus giants. For everything else, one
larger-memory machine is faster, simpler, and cheaper than two smaller ones. That
settles the "two Studios for \$10,000" question: no.

## The market is repricing under a memory shortage

Hardware with lots of fast memory is appreciating, not depreciating, which changes the
usual wait-for-the-sale instinct. FACT: Framework raised its 128 GB desktop from the
\$1,999 launch price to \$2,459 in January 2026, citing DRAM chip costs
([Notebookcheck, 2026-01-13](https://www.notebookcheck.net/Framework-Desktop-now-cost-up-to-460-more-due-to-RAM-shortage.1203235.0.html));
its configurator listed \$3,449 and out of stock when read on 2026-09-16 (the site
refuses our vault's fetcher, so that reading is from the research pass, not a staged
copy). FACT: RTX 5090 graphics cards, nominally \$1,999, sit at \$6,000 to \$9,000-plus
at third-party sellers with official retailers out of stock
([PCGamesN, September 2026](https://www.pcgamesn.com/nvidia/rtx-5090-pricing-september-2026)).
FACT: Apple's certified-refurbished store listed no Mac Studios at all when checked
2026-09-16. Assessment: Used M3 Ultras are holding at or above original retail, so
the used market offers no discount worth waiting for, and Apple is currently the only
vendor selling high-bandwidth memory at list price. Our own machine has appreciated
since purchase.

## The decision, with a date

Assessment: The purchase that changes what we can run is one Mac Studio M5 Ultra
with 256 GB, expected around \$9,000 to \$10,000 pending Apple's configurator pricing.
That is the smallest machine that runs GLM-5.3-Flash at 4-bit with context headroom,
which is the model tier practitioners describe as one generation behind frontier. At
1.2 TB/s it should generate several times faster than anything we own; no real number
exists until independent benchmarks land after the 2026-09-22 ship date.

Speculation: If the M5 Max's neural-accelerator gains carry to the Ultra, GLM-5.3-Flash
could plausibly run in the 40 to 60 tokens/sec range on that machine, which is
comfortable interactive speed. That is extrapolation, not measurement.

The decision gate is **2026-10-20**: by then independent M5 Ultra benchmarks and real
configurator prices will exist. Before spending anything, the free experiment runs
first: local models on the machine we already own, driven through Claude Code, judged
on our actual work. What would change the answer: benchmarks showing the M5 Ultra
underperforming its bandwidth, a configurator price well above \$10,000 for 256 GB, or
the local-model trial showing the quality tier does not serve our workloads anyway.

## Sources

Staged under `10_staging/local-frontier-home-lab/sources/`, read 2026-09-16: Apple
newsroom and spec pages, MacRumors, hardware-corner.net M5 Max benchmarks, IntuitionLabs
DGX Spark review, Level1Techs Strix Halo thread, ollama/ollama issue #15601, Jeff
Geerling's cluster writeup, Notebookcheck, PCGamesN, Hugging Face model cards for
DeepSeek-V4.1-Flash and Qwen3.8-Flash-Next, the exo repository, and LM Studio's Claude
Code guide. Leaderboard readings (LMArena, Artificial Analysis) are from pages that
render only in a browser and are dated in the text.
