---
type: lesson
series: ai-engineering
chapter: 18
title: The plan that reads right and the plan that runs
status: curated
tags: [ai, verification, tooling, shell, linux, using-ai-well, provisioning]
created: 2026-08-19
---

# Why a correct-looking plan still falls over on contact

On 18 August 2026 a plan arrived for building a local pipeline that turns a text prompt
into a printable 3D miniature. It was well organised, it had a comparison table, it named
real tools, and most of what it said was true. It was also wrong in precisely the places
that would have ended the afternoon, and it left out the one tool that made the whole
thing easy.

Then we built the thing. Six more failures turned up over the next day. Not one of them
was visible by reading anything.

That gap is the subject here, because it is the practical difference between work that
looks finished and work that is finished. A plan is a claim about what will happen. The
only test of it is running it.

## The document that read correctly

Give it its due first. Its central argument was right, and it matched what
[chapter 17](topics/ai-engineering/17-generating-3d-versus-scripting-it) worked out
independently: a model that generates a mesh directly beats a model asked to write code
that constructs one, because organic shapes are not what construction-by-primitive is good
at. Its pipeline shape was right too. Its command line for one of the tools was correct
down to the flag names.

Four things were wrong, and they have the same shape.

**A hardware number understated by half.** The document put Microsoft's TRELLIS at "8–12 GB
VRAM." FACT: the TRELLIS README states "An NVIDIA GPU with at least 16GB of memory is
necessary. The code has been verified on NVIDIA A100 and A6000 GPUs"
([microsoft/TRELLIS](https://github.com/microsoft/TRELLIS)). On an 8 GB laptop card that is
the difference between a tool you can run and one you cannot. Worse, the document
contradicted itself: its own hardware section put TRELLIS in the 12 GB-and-up tier.

**"Open source" doing heavy lifting.** The table was headed "Top Open-Source AI Mesh
Tools." FACT: Hunyuan3D 2.1 ships under the Tencent Hunyuan Community License, whose
"Territory" is defined as worldwide *excluding the European Union, United Kingdom and South
Korea*
([licence text](https://github.com/Tencent-Hunyuan/Hunyuan3D-2.1/blob/main/LICENSE)).
FACT: Stable Fast 3D is under the Stability AI Community License, which permits free
commercial use only below one million US dollars of annual revenue
([Stability AI](https://stability.ai/license)). Both are usable by a hobbyist. Neither is
open source, and if you had picked one for something commercial on the strength of that
heading you would have had a problem later rather than sooner.

**Two blockers omitted entirely.** The tool the document recommended, TripoSR, is genuinely
MIT licensed and genuinely fits in 6 GB ([VAST-AI-Research](https://github.com/VAST-AI-Research/TripoSR)).
But FACT: its `requirements.txt` pulls `torchmcubes` straight from a git repository rather
than a wheel, and that package compiles native CUDA code at install time, so it needs the
CUDA development toolkit and not merely the driver
([TripoSR](https://github.com/VAST-AI-Research/TripoSR)). FACT: when that build finds no CUDA compiler it produces a CPU-only
version that installs cleanly and only announces itself at runtime, and TripoSR's own
troubleshooting notes carry the message verbatim: "torchmcubes was not compiled with CUDA
support, use CPU version instead ... This is because `torchmcubes` is compiled without CUDA
support" ([TripoSR](https://github.com/VAST-AI-Research/TripoSR)). Assessment: that it
warrants a troubleshooting entry, and a long-running issue thread
([#83](https://github.com/VAST-AI-Research/TripoSR/issues/83)), says this catches people
routinely rather than rarely. A clean install proves nothing. Separately, its dependency pins are from 2023 and will not build on
a current Python.

**And the tool that solved everything was absent.** FACT: `trellis.cpp` is a C++/GGML
reimplementation of TRELLIS.2 with Vulkan, CUDA, ROCm and CPU backends and no Python at
runtime ([pwilkin/trellis.cpp](https://github.com/pwilkin/trellis.cpp)). Vulkan removes the
CUDA toolkit requirement. No Python at runtime removes the dependency-pinning problem. It
made both of the omitted blockers irrelevant and it was not in the document at all.

Assessment: notice that every one of those four is *checkable in minutes*. The VRAM figure
is one line of a README. The licences are one file each. The build dependency is one line
of `requirements.txt`. The document was not guessing wildly. It was reporting plausible
things without opening the pages, which is a specific failure mode and not the same as
being uninformed.

## Then we ran it

The plan became about a dozen shell scripts to provision a new machine. Here is what
happened when a person typed them, in order.

**A command wrapped in the terminal.** A `sudo apt install ... && sudo systemctl enable ...`
one-liner broke across two lines at exactly the wrong place, so `sudo` ended line one and
`systemctl enable` ran as its own command without privileges. The reported error was
"Unit file earlyoom.service does not exist," which sends you looking for a missing package
rather than a missing word. The real blocker underneath was different again: `sudo` cannot
prompt for a password with no terminal attached.

**A placeholder taken literally.** Instructions said `sudo make-bios-usb.sh /dev/sdX`. The
`X` was standing in for a letter. It got typed as written, because it was presented as a
copy-pasteable line, and the script answered "`/dev/sdX` is not a block device," which is
true and useless.

**Two versions of the same tool disagreeing about whitespace.** This is the good one.
A safety guard checked that a target disk was removable before erasing it, by comparing the
output of `lsblk -dno RM` against `"1"`. FACT: on this machine, util-linux 2.42.2 installed
via Homebrew returns `1`, and the system's util-linux 2.39.3 returns ` 1` with a leading
space. Homebrew is first on the user's PATH; `sudo` uses `secure_path` and gets the system
one. So the guard passed when tested by hand and refused a perfectly good disk when run for
real. Every diagnostic run to investigate it ran as the user, and therefore kept reporting
that everything was fine.

**A command that succeeds and reports failure.** `sgdisk --zap-all` cannot parse the hybrid
ISO9660 layout left on a USB stick by a previously written installer image. FACT: it prints
"Invalid partition data!", destroys the old partition table anyway, and exits non-zero. With
`set -e` in the script, that killed the run in the gap between destroying the old table and
writing the new one, leaving a blank disk and an alarming pair of messages that appear to
contradict each other.

**A format that quietly does nothing.** The partition was then sized at the whole 476.7 GB
device, and `mkfs.vfat -F32` will not format a volume that large with default cluster
sizing. FACT: the run left a partition present with no filesystem on it. The script had
already moved on. Nothing said "this failed" in a way anyone noticed, and the next command
to look at the disk found something that looked partly right.

**And the cheap check done last.** The plan built a BIOS-update USB stick, walked it to the
new machine, and only then read the installed firmware version off the setup screen, which
turned out to already be the version on the stick. The whole exercise was unnecessary and
the check that would have shown it took thirty seconds, on a screen the operator was going
to be standing in front of anyway.

## What they have in common

Assessment: five of those six are the same mistake wearing different clothes. **The script
trusted a report instead of looking at the result.**

An exit code is a report. A tool's formatted output is a report. "pip install completed" is
a report. In every failure above, the report and the reality came apart: `sgdisk` said it
failed while succeeding, `mkfs.vfat` failed while leaving something plausible behind,
`torchmcubes` installs cleanly while being the wrong build, and `lsblk` said `1` or ` 1`
depending on which copy answered.

The repairs all took the same form, and it is worth stating as a rule. **Check the state,
not the return value.** After creating a partition, look for the partition. After
formatting, ask the kernel what filesystem is there. For the removable-disk guard, stop
parsing a tool's output and read `/sys/block/<dev>/removable`, which is the kernel's own
answer and cannot be formatted into ambiguity by a version bump.

The sixth is different and is about sequencing rather than verification. **Do the cheap
check that might make the expensive work unnecessary, first.** It is not a correctness bug;
every step worked. It is a plan that was in the wrong order, and reading it would never have
revealed that, because a plan in the wrong order reads exactly like a plan in the right one.

## The one that cost two evenings

Everything above was an hour here and an hour there. The expensive failure came later, and
it was a different shape again.

The plan called for installing Ubuntu 26.04 LTS on the new machine. That choice was made
from documentation about the distribution: FACT, Ubuntu 26.04 carries ROCm, AMD's GPU
compute stack, in its own package archive, and FACT, its tested-architecture list names
gfx1151, which is the graphics processor in this machine. Both true. An LTS release with
first-party support for exactly this chip is a defensible pick and it reads like diligence.

It does not boot. It reaches its own boot menu and then blanks the screen the moment the
kernel takes over the display, on two different display outputs, surviving three separate
workarounds. The machine stays alive underneath the whole time, which is how you can tell
it is the display and not a crash.

FACT: there is a documented regression in which this processor family black-screens on
Linux kernel 6.19.0 and works on 6.18.9
([CachyOS forum](https://discuss.cachyos.org/t/regression-black-screen-with-kernel-6-19-0-on-amd-ryzen-ai-max-395-strix-halo/23042)).
FACT: Ubuntu 26.04 ships kernel 7.0, which is 6.20 renumbered, the release after the one
that broke it.

Here is the part worth sitting with. Two public setup guides exist for this exact
processor. FACT: one specifies "Ubuntu 24.04 LTS or later (this guide tested on Ubuntu
25.10)" ([Framework Strix Halo guide](https://github.com/Gygeek/Framework-strix-halo-llm-setup)),
and FACT: the other specifies Ubuntu 24.04 LTS
([strix-halo-guide](https://github.com/hogeheer499-commits/strix-halo-guide)). Neither uses
26.04. Finding them took one search of about two minutes, run only after two evenings had
already gone.

Assessment: the error was not picking wrong. It was answering the wrong question. I asked
"which distribution supports this hardware," which the vendor's own documentation answers
confidently, and never asked "what do people running this hardware actually install," which
is a different question with a different and more reliable answer. A compatibility matrix
tells you what is meant to work. A stranger's setup guide tells you what did.

**The rule: for anything new enough to be interesting, find someone who has it working
before designing from specifications.** Support matrices are written by the party who
benefits from breadth. Setup guides are written by people who were annoyed enough to
document a fight they won.

## When the retry path is the main path

One more, from the same work. A provisioning script kept failing at a different stage each
time: an exit code that lied, a whitespace difference between two versions of the same
tool, a partition table copied from a disk image that believed the disk was smaller than it
was, a cleanup that deleted something it did not own, and finally a re-run that produced a
useless four-sector partition because the previous run had already consumed the free space
it wanted.

That last one is the general lesson. **A provisioning script is run again by definition**,
because the reason you are running it is that something went wrong last time. Its retry
path is not an edge case, it is the path most executions take. It had been written as the
exception.

## What this means for working with a model

Assessment, and it is the practical point. A fluent plan and a correct plan look identical
on the page. The properties that separate them are all things that can only be established
by contact: does the number match the README, does the licence say what the heading claims,
does the command run, does the disk actually have a filesystem on it now.

So the useful discipline is not "distrust the output." It is narrower and more actionable:

- **Any claim that could be checked in minutes should be, before it is relied on.** Four
  errors in that document were each one page-load away from being caught.
- **A hedge on something checkable is worse than either doing the check or leaving the claim
  out**, because it ships the error while looking careful.
- **Run it on the target, not on something that resembles the target.** The whitespace bug
  was invisible to every test not run under `sudo`.
- **Expect the first contact with real hardware to find things**, and treat that as the
  process working rather than as a surprise. Six is not an unusual number.

## The honest bottom line

- The plan was well written and mostly true, and was wrong in four checkable places, all of
  which were one page-load from being caught.
- It also omitted the tool that made the problem easy, which is the failure mode nobody
  notices, because absence leaves no error message.
- Six failures showed up on contact. Five were the same mistake: trusting a report rather
  than checking the resulting state.
- The fix is a rule, not vigilance: after an operation, ask the system what is now true.
- The sixth was sequencing. Do the thirty-second check before the twenty-minute one.
- The most expensive error was none of those. It was choosing a component from a vendor's
  compatibility matrix instead of from what people running the hardware actually use. Two
  guides, one search, two minutes, found only after two evenings.
- And a provisioning script's retry path is the path most runs take, because you only run
  it again when something broke.

## See also

- **In this series:** [← Generating 3D](topics/ai-engineering/17-generating-3d-versus-scripting-it) · [Overview](topics/ai-engineering/)
- **[Safety and good habits](topics/ai-engineering/13-safety-and-best-practices)** — the same
  instinct applied to agents that act: least power, verify before trusting.
- **[Web automation and bot defences](topics/ai-engineering/16-web-automation-and-bot-defenses)**
  — where a status code answers a different question from the one you asked.
- **[Mini forge](projects/mini-forge/)** — the project this came out of, and where it stands.

## Sources

- Microsoft, *TRELLIS* — https://github.com/microsoft/TRELLIS
- VAST AI Research, *TripoSR* — https://github.com/VAST-AI-Research/TripoSR
- TripoSR issue #83, *torchmcubes was not compiled with CUDA support* — https://github.com/VAST-AI-Research/TripoSR/issues/83
- pwilkin, *trellis.cpp* — https://github.com/pwilkin/trellis.cpp
- Tencent, *Hunyuan3D 2.1 Community License* — https://github.com/Tencent-Hunyuan/Hunyuan3D-2.1/blob/main/LICENSE
- Stability AI, *Community License* — https://stability.ai/license
- CachyOS forum, *Black Screen with Kernel 6.19.0 on AMD Ryzen AI Max 395* — https://discuss.cachyos.org/t/regression-black-screen-with-kernel-6-19-0-on-amd-ryzen-ai-max-395-strix-halo/23042
- Gygeek, *Framework Strix Halo LLM setup* — https://github.com/Gygeek/Framework-strix-halo-llm-setup
- hogeheer499-commits, *strix-halo-guide* — https://github.com/hogeheer499-commits/strix-halo-guide
