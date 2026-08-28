---
title: "PS4 custom firmware: how the exploit chain works"
type: topic-note
category: technology
tags: [security, hardware, gaming, hacking, exploit, ps4, webkit, kernel]
created: 2026-08-28
updated: 2026-08-28
sources_staged: true
draft: false
---

# PS4 custom firmware: how the exploit chain works

The PS4 runs a custom Sony OS called Orbis, based on FreeBSD 9.0. That one fact is the foundation
of everything that follows: the PS4's kernel is a Unix kernel, its syscall table is largely
FreeBSD's syscall table, and twenty years of FreeBSD kernel research applies directly to it. Sony
built on top of a well-understood system, which made their job building new features easier and the
hacker community's job tearing through it more tractable than it might otherwise have been.

## The architecture Sony built

The PS4 jailbreak problem has three distinct layers, and defeating any one of them does not defeat
the others. Assessment (from CTurt's 2015 writeups and fail0verflow's 2017 disclosure):

**Layer 1 — the browser process.** The PS4 ships with a built-in web browser built on WebKit, the
same open-source engine used by iOS, the Wii U, and the PS Vita. The browser runs in a
sandbox that limits what it can access. Getting code execution in the browser process gets you
inside the sandbox but not out of it.

**Layer 2 — the sandbox.** To reach the kernel the attacker must escape the browser sandbox. This
step is the least well-documented in the public writeups; the technical details of the sandbox
escape mechanism are largely absent from the sources gathered here.

**Layer 3 — the kernel.** The kernel runs Orbis/FreeBSD. Getting kernel-level code execution means
the attacker can patch kernel memory, add new syscalls, and control what the console will and won't
run. This is what enables unsigned code — homebrew and CFW payloads.

## How the WebKit entry works

Assessment: The PS4's WebKit sandbox is missing several mitigations iOS has added over the years,
including Gigacage (an allocator hardening scheme) and StructureID randomization. JIT compilation
is disabled on PS4, which removes one attack class but also removes a constraint the iOS team
designed against. The result is that iOS WebKit exploits often port to PS4 with modification.

The first public PS4 exploit (2014, by researchers "nas and Proxima") was CVE-2012-3748 — a
heap-based buffer overflow in `JSArray::sort()` originally written for Mac OS X Safari, adapted to
PS4 firmware 1.76. The vulnerability was two years old when it was ported; Sony had not patched
it.

Assessment: A WebKit exploit typically gives the attacker arbitrary read/write within the browser
process, which they then use to build a ROP chain — Return-Oriented Programming, where instead of
injecting new executable code (blocked by data execution prevention) the attacker chains together
small snippets of existing executable instructions to do what they want.

To get from ROP to actual native code execution, CTurt identified a Sony-specific mechanism:
`sys_jitshm_create` and `sys_jitshm_alias`, custom syscalls that create a shared memory region
mapped twice — once as writable, once as executable, pointing to the same physical memory. Write
shellcode through the writable mapping, execute it through the executable one. The catch: a
`syscall` instruction executed from within JIT memory triggers a fault, so system calls have to
be made by jumping to `syscall` instructions that already exist in `libkernel`.

## Kernel exploits, generation by generation

Assessment: Sony's kernel contains over 600 syscalls — roughly 500 from FreeBSD and the rest
Sony-specific. The Sony-specific ones have been the most productive attack surface, partly because
FreeBSD's native syscalls have more eyes on them and partly because Sony's additions were less
scrutinised.

**Firmware ≤1.76 / 2.xx (patched in 2.xx):** CTurt achieved kernel code execution via a heap
overflow in `sys_dynlib_prepare_dlclose`, a Sony-specific dynamic linking syscall. fail0verflow
obtained the 1.01 firmware kernel dump via a PCIe man-in-the-middle attack; Sony had accidentally
left full ELF symbol information in that build, which gave researchers a map of the entire kernel
— an oversight corrected in every subsequent firmware.

**Firmware 4.05:** fail0verflow's "namedobj" exploit, implemented by Cryptogenic. The
`sys_namedobj_create` syscall lets a userland process set a type field to a nearly fully
user-controlled value; `sys_mdbg_service` can then be tricked into treating the resulting object
as a debug struct it is not, leading to a controlled kernel write. The exploit patches kernel write
protection, enables RWX memory mappings, and installs a custom syscall #11 (`kexec()`) for
running arbitrary kernel code. Reported stability: approximately 95%.

**Firmware 5.05:** A BPF (Berkeley Packet Filter) double-free and race condition. The BPF driver
in PS4's FreeBSD kernel lacked proper locking; qwertyoruiopz had previously exploited a BPF write
vulnerability on ≤4.55 firmware, and Sony's fix (removing write functionality from BPF) left the
underlying race condition intact through 5.05. Sony's eventual response was to block unprivileged
access to `/dev/bpf` entirely — closing the attack class rather than fixing the bug.

**Firmware ≤6.72 / 7.02:** The WebKit "bad-hoist" bug (CVE-2018-4386) covers firmware through
6.72; a kernel exploit by theflow0 provides kernel R/W primitives reachable from the WebKit
sandbox and covers up to 7.02. Synacktiv documented a novel WebKit use-after-free
(`WebCore::ValidationMessage::buildBubbleTree`) discovered by their internal fuzzer, combined with
an ASLR weakness on PS4 to build a full read/write primitive.

## Firmware 9.00 and GoldHEN

Assessment: The sources gathered for this note stop at approximately firmware 7.02. The 9.00
jailbreak — which became the most widely used entry point for end users — and GoldHEN, the
homebrew-enabler payload that runs on top of it, are absent from the source set.

What is publicly known but not sourced here: firmware 9.00 is the highest version with a
confirmed public jailbreak as of mid-2024. GoldHEN is a payload developed by GoldHEN_Team that
provides a homebrew launcher, FTP server, and game patching support on top of a kernel exploit;
it is hosted and updated on GitHub. Because it operates by patching kernel memory at runtime,
rebooting the console returns it to stock — there is no permanent flash modification as there
was with PS3 CFW, which means re-exploiting is required after every power cycle.

The gap between what the researcher writeups cover (through ~7.02) and what end users actually use
(9.00 + GoldHEN) represents roughly two firmware generations of exploit development that has not
yet been written up in the same detail as the earlier work.

## Why PS4 is structurally different from PS3

The PS3 break was permanent because the signing key was burned into read-only hardware and the
private key was recovered mathematically, with no patch ever possible. The PS4 learned from that.

Assessment: The PS4 uses per-console keys and a more layered trust chain. There is no single
global private key to recover. Each firmware generation is a separate fight: Sony patches the
known vulnerability, researchers find the next one. The exploits documented above do not persist
across firmware updates, and updating the firmware closes the hole they used. The result is a
moving boundary: a console on firmware 9.00 that never updated is permanently jailbreakable;
the same hardware on a later firmware may or may not be, depending on whether a public exploit
exists for that version.

This is why "what firmware are you on" is the first question in any PS4 jailbreak discussion.
The PS3 answer was always "it doesn't matter" — once the key leaked, every unit ever made was
open. The PS4 answer is: it matters entirely.

---

**How much to trust this:** Assessment-labeled throughout. The primary sources are the researchers'
own writeups (CTurt, fail0verflow, Cryptogenic, Synacktiv) — these are authoritative for the
specific firmware versions they address. The GoldHEN and 9.00 material is noted as publicly known
but unsourced in this note. Trust the exploit-chain architecture description; treat the 9.00 and
GoldHEN section as an honest statement of the gap.

*Sources: [CTurt — Hacking the PS4 part 1](https://cturt.github.io/ps4.html);
[CTurt — Hacking the PS4 part 3 (kernel)](https://cturt.github.io/ps4-3.html);
[fail0verflow — The First PS4 Kernel Exploit: Adieu](https://fail0verflow.com/blog/2017/ps4-namedobj-exploit/);
[Cryptogenic — PS4 4.05 Kernel Exploit](https://github.com/Cryptogenic/PS4-4.05-Kernel-Exploit);
[Cryptogenic — PS4 5.05 BPF Double Free writeup](https://github.com/Cryptogenic/Exploit-Writeups/blob/master/FreeBSD/PS4%205.05%20BPF%20Double%20Free%20Kernel%20Exploit%20Writeup.md);
[Synacktiv — This is for the Pwners](https://www.synacktiv.com/en/publications/this-is-for-the-pwners-exploiting-a-webkit-0-day-in-playstation-4);
staged in 10_staging/ps4-custom-firmware-jailbreak-exploit-webkit-kernel-goldhen-9-00-history/*
