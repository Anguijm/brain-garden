---
title: "How the PlayStation 3 got cracked permanently, and why Sony couldn't fix it"
type: topic-note
category: technology
tags: [security, hardware, cryptography, gaming, hacking, history]
created: 2026-08-28
updated: 2026-08-28
sources_staged: true
draft: false
---

# How the PlayStation 3 got cracked permanently, and why Sony couldn't fix it

Sony designed the PlayStation 3 to be unhackable. They were wrong, and the reason they were wrong is a
lesson in how a single cryptographic mistake can collapse an otherwise careful security architecture
permanently, with no patch possible.

## The security model

Assessment: The PS3's security rests on a hypervisor — a thin layer of privileged software that runs
beneath the operating system and controls what any code running above it can touch. Even the Linux
environment Sony originally shipped ("OtherOS") could not directly access GPU hardware or the disc
drive; the hypervisor enforced that boundary. Game decryption keys live in a read-only area on each
disc, and the drive firmware passes them upward only through hypervisor calls. Breaking the system
required breaking the hypervisor, and breaking the hypervisor required breaking the signing keys that
told the hardware what code was authorised to run.

Those signing keys were protected by ECDSA — Elliptic Curve Digital Signature Algorithm — a standard
cryptographic scheme where each signature requires a random number that must never be reused. Sony
shipped the PS3 with the public half of the ECDSA key burned permanently into the bootloader, which is
read-only hardware. That meant: if the private key ever leaked, Sony could not revoke it. Every PS3
ever manufactured would be compromised for life.

## The first cracks

Assessment: The first homebrew code ran on PS3 in 2009, exploiting a vulnerability in the game
*Resistance: Fall of Man* — a narrow, game-specific hole, not a break of the underlying security model.

In early 2010, George Hotz (known as geohot, who had previously jailbroken the iPhone) achieved
something more fundamental. Assessment: He connected an FPGA to the PS3's memory bus and sent a 40
nanosecond voltage pulse at a precise moment during hypervisor operation — a technique borrowed from
smart card attacks — exploiting a race condition in how the hypervisor managed its own page table. The
result was arbitrary read/write access to RAM with hypervisor privileges: level 1 access. This was not
yet enough to run pirated games, but it proved the hypervisor could be reached.

That same year, Sony made a decision that changed the trajectory of everything. In April 2010 they
pushed firmware update 3.21, which removed OtherOS — the Linux capability they had shipped with the
console. Assessment: The stated reason was security; the actual effect was to convert a community of
researchers who had been working within Sony's own framework into adversaries with a grievance.

## The cryptographic failure

Assessment: The group fail0verflow, motivated specifically by the OtherOS removal, set out to restore
Linux access. In doing so they examined Sony's ECDSA signing implementation and found something
remarkable: Sony was using the same random number for every signature.

This is catastrophic. ECDSA security depends entirely on that number — called *k* — being different
for every signature. If *k* is constant, then the value *r* (which appears in the signature and is
derived from *k*) is also constant. Two signatures with the same *r* and different messages allow an
attacker to algebraically solve for the private signing key. The math is straightforward once you have
two examples. Sony had produced thousands of signed firmware packages, all with the same *k*.

Assessment: Fail0verflow announced the flaw at the 2010 Chaos Communication Congress in Berlin. They
chose not to publish the private key itself — they had what they needed to restore OtherOS, and
releasing the key would enable game piracy they did not want to enable.

Geohot had fewer reservations. On January 3, 2011, he published the private key.

## Why there was no fix

The public component of the ECDSA key pair is burned into the PS3 bootloader — read-only hardware,
set at the factory. Sony could publish new firmware signed with a new key, but any PS3 already shipped
would still trust the old key, because the trust anchor is in silicon and cannot be updated. Patching
the bootloader would have required recalling every PS3 ever sold.

Assessment: The consequence was permanent. Custom firmware based on official version 3.55 — the last
version before Sony's partial patches — circulated quickly. The CFW "Rebug," released March 2011,
gave retail units most of the capabilities of Sony's developer hardware. Firmware 3.56 and later
versions blocked some attack paths on consoles that accepted the update, but any PS3 with a
compatible firmware version and flash write access remains permanently exploitable. As of 2024,
tools exist to convert even newer official firmware to CFW on most hardware versions.

## The legal aftermath

Assessment: Sony filed suit in January 2011 against both fail0verflow and geohot, alleging DMCA
violations (circumvention and trafficking in circumvention devices), Computer Fraud and Abuse Act
claims, and contributory copyright infringement. Sony also obtained court approval to collect IP
addresses of visitors to geohot's website — a move the Electronic Frontier Foundation characterised
as disproportionate and chilling to security research.

The jurisdictional basis was contested: Hotz had done his work in New Jersey, but had used
California-based platforms (YouTube, Twitter, PayPal) to distribute his findings. The court applied
a "personal direction" test and asserted California jurisdiction.

The case settled in late March or April 2011. Geohot agreed to a permanent injunction barring
him from further reverse engineering of Sony products; no judicial ruling on the underlying legal
questions — consumer rights to modify owned hardware, the scope of DMCA circumvention exemptions —
was ever issued.

Assessment: The earlier case *Sony v. Connectix* (2000) had established that reverse engineering a
console BIOS for emulator development constitutes fair use under the 9th Circuit. Whether that
reasoning extends to jailbreaking physical hardware remained unresolved by the Hotz settlement, and
remains unresolved today.

## The principle

The PS3 break is a case study in how root trust works and what happens when it is compromised. The
signing key was the foundation everything else rested on. Sony's cryptographic implementation was
correct in design and catastrophically wrong in execution — not because ECDSA is broken, but because
the implementation reused *k*. One constant in the wrong place collapsed a hardware security model
that had otherwise been carefully built.

The lesson is not "Sony was careless." It is that a correct algorithm, incorrectly implemented, is
indistinguishable from no algorithm at all, and that any security model whose root of trust is in
patchable software can be fixed, while one whose root of trust is in read-only hardware cannot.

---

**How much to trust this:** Assessment-level throughout. The technical claims about the hypervisor,
ECDSA flaw, and glitching attack come from a 2010 rdist blog post by Nate Lawson that is well-regarded
in the security community but is itself analysis of Hotz's disclosure, not primary documentation.
The legal claims come from the Berkeley Technology Law Journal and contemporary reporting. The
fail0verflow CCC 2010 presentation — which would be the most authoritative primary source on the
cryptographic attack — was not accessible during research. Wikipedia's PS3 homebrew article is the
most comprehensive single source but is unverified and editable. Treat the technical narrative as
well-supported analysis, not verified fact.

*Sources: [rdist.root.org — How the PS3 hypervisor was hacked (2010)](https://rdist.root.org/2010/01/27/how-the-ps3-hypervisor-was-hacked/); [Berkeley Technology Law Journal — Sony v. Hotz (2011)](https://btlj.org/2011/03/sony-v-hotz-controversies-regarding-dmca-jurisdiction-search-warrant-and-subpoenas/); [Wikipedia — PlayStation 3 homebrew](https://en.wikipedia.org/wiki/PlayStation_3_homebrew); [Wikipedia — PlayStation 3 Jailbreak](https://en.wikipedia.org/wiki/PlayStation_3_Jailbreak); staged in 10_staging/playstation-hacking-jailbreak-history-ps3-ps4-ps5-exploit-homebrew/*
