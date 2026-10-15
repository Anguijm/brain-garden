---
type: lesson
series: ai-engineering
chapter: 24
title: "GenAI.mil: what it can automate, and what it cannot"
status: curated
tags: [ai, ai-agents, agent-engineering, genai-mil, cui, government, loops, state]
created: 2026-10-15
---

# GenAI.mil: what it can automate, and what it cannot

> The Department's generative AI portal is no longer only a chat box. Inside each vendor's
> app you can now save standing instructions and keep files in a project. What you still
> cannot do is reach it from outside, so nothing runs unless a person presses go.

## What I wanted to know

The question, asked on 2026-10-15 while discussing agent loops and graphs: GenAI.mil
carries Gemini, Grok and ChatGPT inside an Impact Level 5 environment, the one place a
contractor can legally put Controlled Unclassified Information (CUI) in front of a frontier
model. Could it run analysis on work documents automatically, the way a loop or a graph
would, rather than one chat at a time?

## Summary

Partly. As of October 2026 all three apps on the portal let a user save standing
instructions (a custom GPT, an Agent Designer agent, a Grok playbook or skill) and keep
files and conversations together in a persistent project. So the same analysis can be run
the same way on each new document. ChatGPT Mil's own limits, read inside the app, are
roomy enough for work documents: 100 MB per document, 20 MB per image, 1,000 files per
project, and projects that can be shared. The number to plan around is the other one: files
are kept for 60 days after their last use, and the clock runs per chat, so a conversation
nobody reopens for two months loses its files. But there
is no application programming interface (API, the published way for one program to call
another), and no documented scheduler. The site
is reachable only with a Common Access Card (CAC) from the Department's unclassified network
on government equipment. In the language of the loops chapter: the portal gives you the
node and the state, but not the outer loop. A person supplies the loop, and a person reviews
the output.

## What is on the portal, and when it arrived

The portal opened with one model and gained two more nine months later. FACT: the War
Department's release of 2025-12-09 announced Google Cloud's Gemini for Government as "the
first of several frontier AI capabilities to be housed on GenAI.mil"
([War Department release, dated 2025-12-09 in the Department's release feed](https://www.war.gov/News/Releases/Release/Article/4354916/the-war-department-unleashes-ai-on-new-genaimil-platform/)).
FACT: the same release said all tools on the portal are certified for CUI and Impact Level
5, and that further models would be available to all civilians, contractors and military
personnel (same release). FACT: the Department's release on the xAI agreement, dated
2025-12-22 in the same feed, said the Grok integration was "targeted for initial deployment
in early 2026" at Impact Level 5, "enabling the secure handling of Controlled Unclassified
Information (CUI) in daily workflows"
([War Department release, 2025-12-22](https://www.war.gov/News/Releases/Release/Article/4366573/the-war-department-to-expand-ai-arsenal-on-genaimil-with-xai/)).
FACT: a release dated 2026-02-09 announced "a partnership with OpenAI to integrate ChatGPT
into GenAI.mil" and said the platform had passed one million unique users in two months
([War Department release, 2026-02-09](https://www.war.gov/News/Releases/Release/Article/4401775/genaimils-rapid-expansion-continues-with-openai-partnership/)).

The other two models arrived at the end of August. FACT: two releases dated 2026-08-31
announced that ChatGPT Mil and Starshield AI's Grok for Government had launched on the
portal, each accredited for CUI at Impact Level 5
([ChatGPT Mil release](https://www.war.gov/News/Releases/Release/Article/4586352/department-of-war-launches-openais-chatgpt-mil-on-genaimil/);
[Grok for Government release](https://www.war.gov/News/Releases/Release/Article/4586482/department-of-war-launches-starshield-ais-grok-for-government-on-genaimil/)).
FACT: the Grok release put the user count at over 1.7 million of more than 3 million
personnel (Grok for Government release). FACT: DefenseScoop reported the same day that
officials said insiders "can now chat with approved versions" of both models, and that the
dispute between the Pentagon and Anthropic had derailed plans to add Claude
([DefenseScoop, 2026-08-31](https://defensescoop.com/?p=131114)). FACT: at a Pentagon event
on 2026-09-30 the platform's authorizing official said the three apps "soft-launched" so
they could be scaled and monitored
([War Department story, 2026-09-30](https://www.war.gov/News/News-Stories/Article/Article/4615232/government-ai-models-advancing-increasing-productivity/)).
FACT: the program manager told the same event that everyone with a Common Access Card can
access all three (same story).

Assessment: that settles the date question the to-do raised. The trade-press claim that
ChatGPT was "integrated" in March 2026 is best read as the February partnership and a soft
launch. The Department's own releases date general availability of ChatGPT Mil and Grok to
2026-08-31.

## What each app can automate

**Gemini: agents built in plain language.** FACT: Google Public Sector announced Agent
Designer as "a no-/low-code platform that allows GenAI.mil users to easily build AI agents
using natural language," for automating "repetitive, multi-step administrative tasks"
([Google Public Sector blog, undated in the captured text](https://cloud.google.com/blog/topics/public-sector/gemini-for-government-build-custom-ai-agents-for-unclassified-work-on-genaimil/)).
FACT: its examples are drafting meeting read-aheads and breaking a goal into a checklist
(same post). FACT: Nextgov reported on 2026-04-27 that users had built more than 100,000
agents on the portal and that those agents are authorized at Impact Level 5
([Nextgov, 2026-04-27](https://www.nextgov.com/defense/2026/04/pentagon-adds-googles-latest-model-genaimil-usage-soars/413127/)).
FACT: the same report said Gemini 3.1 Pro reached portal users eight weeks after commercial
customers, and quoted the Pentagon's chief data officer on "moving from the old concept of
the large language models being just a chat interface to being an actual platform where it
can run tasks on its own" (same report).

**ChatGPT Mil: files, projects and custom GPTs.** FACT: the Department's release says the
core experience "centers on chat, files, projects, and custom GPTs, with additional
features sequenced over time" (ChatGPT Mil release, above). FACT: it is aimed at
document-heavy unclassified work in planning, policy, logistics and administration (same
release). A custom GPT is OpenAI's name for a saved assistant with its own standing
instructions. FACT: a person familiar with OpenAI's work told DefenseScoop that employees
can use it "to generate reliable models for repeatable workflows," such as filling out
travel forms or responding to contracting opportunities (DefenseScoop, 2026-08-31, above).
FACT: the same person said it keeps the files and conversations of months-long projects in
one place, and that GPT-5.4 Terra was the model offered at launch (same report).

**Grok: projects, playbooks, automations and skills.** FACT: the Department's release lists
"customizable workspaces, persistent projects and reusable 'playbooks' that capture and
scale institutional knowledge" (Grok for Government release, above). FACT: a Grok for
Government technician told the 2026-09-30 event that "automations in Grok have been trained
to do certain tasks and provide timely updates" (War Department story, 2026-09-30, above).
FACT: he said projects mode lets teams upload files and collaborate in a workspace, and
skills mode generates recurring administrative documents such as executive summaries (same
story).

**What all three share.** The Marine Corps made GenAI.mil the service's enterprise platform
by message on 2026-01-20 (a MARADMIN is a Marine administrative message). FACT: MARADMIN
018/26 lists the platform's current capabilities as a chat interface, file upload of PDF,
Word (DOCX) and plain text (TXT) files, and retrieval-augmented generation "for sourcing
answers from provided documents"
([MARADMIN 018/26](https://www.marines.mil/News/Messages/Messages-Display/Article/4383324/enterprise-generative-artificial-intelligence-availability-and-governance/)).
FACT: the same message lists secure web grounding, deep research, and persistent chat
histories (same message).
FACT: the Department of the Navy memorandum of 2026-01-28 repeats that list and widens the
upload types to PDF, DOCX, TXT, PowerPoint (PPTX) "and all image files"
([DON CIO, joint memo of 2026-01-28](https://www.doncio.navy.mil/ContentView.aspx?ID=20455)).

## What it cannot do

**No way in from outside.** FACT: MARADMIN 018/26 lists "agentic AI development solutions,
and API access for integration with other enterprise systems" among planned future
enhancements, and says "the timeline for future features is under development" (MARADMIN
018/26, above). FACT: two Marine Corps education staff wrote in DefenseScoop on 2026-01-12
that "the absence of an application programming interface (API) means GenAI.mil cannot
currently be integrated with other essential digital tools"
([DefenseScoop commentary, 2026-01-12](https://defensescoop.com/?p=123238)). FACT: they
also wrote that the platform then lacked persistent file management and could not generate
images (same commentary). Assessment: the persistent-files complaint was answered by the
August projects features; the API complaint was not. No official page read for this note
announces an API, a scheduled job, or a trigger that fires when a new file lands. Grok's
"automations" that "provide timely updates" are the one phrase that could mean a timer, and
nothing read says what starts them. **Unverified** (2026-10-15): whether any of the three
apps can run an agent on a schedule or on a new upload without a person starting it. The
portal's own FAQ at https://genai.mil/resources/faq would say, and it is behind the CAC wall
(HTTP 503 from the open internet when checked the same day).

**Limits, known for ChatGPT Mil only.** FACT: the ChatGPT Mil app on GenAI.mil states its
own limits, and I read them there on a government machine on 2026-10-15 (shown inside the
app; no public page, so no link). FACT: a document may be up to 100 MB and an image up to
20 MB, the app states. FACT: the same app states a limit of 1,000 files per project, that
files can be shared, and that a file is retained for 60 days after its last use, with the
clock counted per chat interaction (scope confirmed on a second look the same day).
**Unverified** (2026-10-15): whether a file kept in a project outlives the chats that used
it, and whether the 60-day clock also clears the chat history itself. The figures were
relayed without the surrounding text, and the page sits behind the CAC wall. **Unverified**
(2026-10-15): the matching limits for Gemini for Government and Grok
for Government, and the context window of any of the three government builds. No official
page read states them, and the vendors' commercial limits are not evidence for the Impact
Level 5 versions. The one hint is the Pentagon statement that models now reach the portal
on or near their commercial release day (War Department story, 2026-09-30, above).
Assessment: the other two probably track their current commercial builds, which is a guess
until they are read the same way.

## Who gets in

FACT: the Department's own story on the launch says "access to the site is available only
to personnel with a common access card and who are on the War Department's nonclassified
network"
([War Department story on the launch, undated on the page](https://www.war.gov/News/News-Stories/Article/Article/4355797/hegseth-introduces-department-to-new-ai-tool/)).
FACT: it quotes the Secretary's email that the platform is available to "all military
personnel, civilians and contractors" (same story). FACT: MARADMIN 018/26 authorizes use "by
all Marines, civilians, and contractors using government-furnished equipment" (MARADMIN
018/26, above). FACT: the Navy memorandum designates the platform for "mission owners and
all Department of the Navy (DON) users" without separating contractors from civilians (DON
CIO memo, above). FACT: a later Navy CIO memorandum, published 2026-03-16 on AI adoption and
training, describes the platform as "available on the desktops of all military personnel,
civilians, and contractors across the Department of War"
([DON CIO memo of 2026-03-16](https://www.doncio.navy.mil/ContentView.aspx?id=20459)).
Assessment: a contractor with a Common Access Card on a government machine on the
unclassified network is in. A contractor on company equipment or off that network is not,
and nothing read describes an exception.

## What may go in, and what the vendors keep

Impact Level 5 is an unclassified level. FACT: the Defense Information Systems Agency's
Cloud Computing Security Requirements Guide says "Level 5 accommodates CUI that requires a
higher level of protection than that afforded by Level 4 as deemed necessary by the
information owner, public law, or other government regulations"
([DISA Cloud Computing SRG, web page edition, undated on the page](https://dl.dod.cyber.mil/wp-content/uploads/cloud/SRG/index-old.html)).
FACT: the same guide says Level 5 also supports unclassified National Security Systems, and
that Level 6 is the level where "only information classified as SECRET or below" may be
hosted (same guide). **Unverified** (2026-10-15): whether the current edition, the January
2025 DoD Cloud Service Provider SRG, changed that wording; it sits behind a JavaScript wall
at https://public.cyber.mil/dccs/ and the staged copy is the older HTML edition.

FACT: the Navy memorandum says the platform "is fully authorized to handle CUI" and that
"Protected Health Information (PHI) and Personally Identifiable Information (PII) are not
permitted for use on this platform" (DON CIO memo, above). FACT: MARADMIN 018/26 says the
same, adding classified information to the prohibited list (MARADMIN 018/26, above).

Two of the three vendors say in writing that the Department's data does not train their
models. FACT: Google's announcement of the award says "the data used by the DoW on this
platform is never used to train Google's public models"
([Google Cloud press release, 2025-12-09](https://www.googlecloudpresscorner.com/2025-12-09-Chief-Digital-and-Artificial-Intelligence-Office-Selects-Google-Clouds-AI-to-Power-GenAI-mil)).
FACT: OpenAI's announcement says data processed on the portal "remains isolated to the
government environment and is not used to train or improve OpenAI's public or commercial
models"
([OpenAI, "Bringing ChatGPT to GenAI.mil"](https://openai.com/index/bringing-chatgpt-to-genaimil/)).
**Unverified** (2026-10-15): any equivalent statement from xAI for the Grok for Government
build; its announcement page says only that personnel can use its technologies at Impact
Level 5, and no training or retention statement was found.

On retention, one of the three apps has answered. FACT: ChatGPT Mil keeps an uploaded file
for 60 days after its last use, counted per chat interaction, and files can be shared, as
the app states inside GenAI.mil (read on a government machine on 2026-10-15; no public
page). The limits paragraph above has the rest. **Unverified** (2026-10-15): retention for
Gemini for Government and Grok for Government, whether a project's files outlive the chats
that used them, and whether administrators can read a user's chats or projects. No public
page read states any of these, and the pages that would are behind the CAC wall.

Assessment: for a shipyard planner that rules out anything with names and personnel data
(rosters, qualifications by person, medical) and rules in the work-package material that is
marked CUI. The 60-day clock makes a chat, and the project behind it, a working file, not
an archive: anything worth keeping goes back into the record system at work before the
clock runs out. My own rule still
applies on top: nothing marked CUI comes out of the portal into these notes, in any layer.

## The Navy memo, read

The memo the trade press summarized exists and says a little more than the summary. FACT:
the joint memorandum of the Assistant Secretary of the Navy for Research, Development and
Acquisition and the Department of the Navy Chief Information Officer, published 2026-01-28,
"formally designates GenAI.mil as the mandated Controlled Unclassified Information (CUI) and
Impact Level 5 (IL5) generative artificial intelligence (AI) platform for mission owners and
all Department of the Navy (DON) users" (DON CIO memo, above). FACT: it directs all DON
organizations and commands "to transition to GenAI.mil effective immediately and no later
than April 30, 2026" (same memo). FACT: it orders a report of all current and planned
generative AI capabilities to a DON CIO task force within 15 days, and consolidation
notices within 45 days under the Department's Cattle Drive initiative (same memo). FACT: it
says it will be updated semiannually as capabilities are added, and cites a Secretary of War
memorandum of 2025-12-09 titled "Harness Artificial Intelligence Now with GenAI" (same
memo). FACT: it is signed by the acting DON CIO, M. Barry Tanner, and Vice Admiral E. S.
Okano (same memo). FACT: the Marine Corps message of eight days earlier says NIPRGPT, the
predecessor chat tool, "is in the process of sunsetting" (MARADMIN 018/26, above).

## Claude's status

Claude is not on the portal, and the legal position changed after the to-do was written.
FACT: on 2026-08-27 the district court for the Northern District of California granted
Anthropic summary judgment, holding that the Department's actions "constituted unlawful
retaliation in violation of the First Amendment"
([Order on cross motions for summary judgment, docket 250, dated 2026-08-27](https://storage.courtlistener.com/recap/gov.uscourts.cand.465515/gov.uscourts.cand.465515.250.0_1.pdf)).
FACT: the same order held that Anthropic was denied the pre-deprivation process the Fifth
Amendment requires, and that the designation violated 10 U.S.C. § 3252 and was arbitrary
and capricious (same order).
FACT: TechCrunch's report of that ruling named the judge as Rita Lin and said a second suit
in Washington, D.C. was still pending
([TechCrunch, 2026-08-28](https://techcrunch.com/2026/08/28/anthropic-gets-its-first-court-win-over-the-pentagons-supply-chain-risk-label/)).
FACT: on 2026-09-25 the D.C. Circuit decided that second case, which concerned the
Department's decision to exclude Claude from its supply chain under the Federal Acquisition
Supply Chain Security Act of 2018
([D.C. Circuit opinion, argued 2026-05-19, decided 2026-09-25](https://www.courtlistener.com/opinion/10983481/anthropic-pbc-v-united-states-department-of-war/)).
FACT: Judge Katsas wrote for the court and Judge Henderson dissented (same opinion).
FACT: Nextgov reported on 2026-09-28 that the ruling means the Department "can prohibit its
employees and contractors from using the AI company's products for DOD business," that the
designation has been in force since March 2026, and that Anthropic said it was "considering
all options, including further review"
([Nextgov, 2026-09-28](https://www.nextgov.com/acquisition/2026/09/anthropic-lawsuit-supply-chain-risk/416255/)).
Assessment: the two cases ran on two different statutes, and the Department won the one
that governs its purchasing. For anyone doing Department business as a contractor, the
operative fact is the September ruling, not the August one. Claude stays off the portal and
off Department work until that changes.

## So: loop, graph, or neither?

Assessment, in the terms of [the loops and graphs chapter](23-loops-graphs-and-state).
What the portal offers is one node of a graph with its state attached. The standing
instructions (a custom GPT, an Agent Designer agent, a playbook) are the node's prompt. The
project with its files and history is the state. Retrieval over the uploaded documents is
how the node reads it. What is missing is everything outside the node: no edge from another
system can trigger it, no scheduler re-runs it, and nothing downstream can read its output
except a person. That is a workflow with a human on both ends, and for work documents that
is probably the right shape anyway, because the reviewer is the back pressure.

Three of the five state failures from the chapter apply. **Lost**: this is the failure
with a number on it. ChatGPT Mil keeps a file for 60 days after its last use and counts the
clock per chat, so a standing analysis that runs less often than that comes back to a chat
with no files. Whether the project's own files survive is the open question; until it is
read, treat the project the same way. A shared project outlives the person who built it
only while someone else keeps using it. Sharing
exists; nothing read says who can be given it or who can see it (Grok's "teams" and
ChatGPT's projects are mentioned, their visibility is not). **Bloated**: a
months-long project accumulates files, and retrieval over a growing pile is where answers
start to drift. **Split**: two people uploading to one project are two writers on one state.
Doubled and misread do not arise while a person starts every run and reads every result.

## What to try, inside the portal only

A method, not a result; the results stay at work.

1. Done for ChatGPT Mil on 2026-10-15: 100 MB per document, 20 MB per image, 1,000 files
   per project, sharing, and 60 days from last use counted per chat. Still to read on the
   same machine: whether a project's files outlive the chats that used them, who a share
   can go to, and the matching numbers for Gemini and Grok. The portal FAQ at
   https://genai.mil/resources/faq is the other place to look.
2. Build one custom GPT or one Agent Designer agent with the standing analysis instructions
   and a fixed output template. Fixed output is what makes two runs comparable.
3. One project per document stream. Drop each new document in, run the same agent, and
   read the result against the document before anything leaves the portal.
4. Run the same document twice on different days and compare. That measures drift before
   it matters.
5. Check whether Grok's automations or Agent Designer can be started by a timer or by a new
   file. If either can, the outer loop exists and this note needs a new section.
6. Keep the method notes here. Never the documents, never a quotation from one.

## Open questions

- Whether a ChatGPT Mil project's files outlive the chats that used them, who a share can
  go to, and the matching numbers for Gemini for Government and Grok for Government, all
  behind the CAC wall.
- An xAI statement on training and retention for Grok for Government on the portal, in
  its own words.
- The Secretary of War memorandum of 2025-12-09, "Harness Artificial Intelligence Now with
  GenAI," which both Navy memos cite and which was not found online.
- Whether Agent Designer agents can call anything outside the chat, such as a document
  library, or only read what is uploaded.
- The current edition of the DISA guide, to confirm the Impact Level 5 wording.

## Source ledger

| # | Source | Tier | AI-generated | Verified |
|---|--------|------|--------------|----------|
| 1 | War Department release, GenAI.mil launch, 2025-12-09 (date from release feed) | 2 | no | read 2026-10-15 |
| 2 | War Department story, "Hegseth introduces Department to new AI tool," undated | 2 | no | read 2026-10-15 |
| 3 | War Department release, xAI agreement, 2025-12-22 (date from release feed) | 2 | no | read 2026-10-15 |
| 4 | War Department release, OpenAI partnership, 2026-02-09 (date from release feed) | 2 | no | read 2026-10-15 |
| 5 | War Department release, ChatGPT Mil launch, 2026-08-31 (date from release feed) | 2 | no | read 2026-10-15 |
| 6 | War Department release, Grok for Government launch, 2026-08-31 (date from release feed) | 2 | no | read 2026-10-15 |
| 7 | War Department story, GenAI.mil Excite Day, 2026-09-30 (date from story feed) | 2 | no | read 2026-10-15 |
| 8 | MARADMIN 018/26, 2026-01-20 | 2 | no | read 2026-10-15 |
| 9 | DON CIO and ASN(RD&A) joint memorandum, 2026-01-28 | 1 | no | read 2026-10-15 |
| 10 | DON CIO memorandum on AI adoption and training, 2026-03-16 | 1 | no | read 2026-10-15 |
| 11 | DISA Cloud Computing Security Requirements Guide, HTML edition (older, V1R3), undated | 1 | no | read 2026-10-15 |
| 12 | Google Public Sector blog, Agent Designer, undated in capture | 4 | no | read 2026-10-15 |
| 13 | Google Cloud press release, 2025-12-09 | 4 | no | read 2026-10-15 |
| 14 | OpenAI, "Bringing ChatGPT to GenAI.mil" (page metadata 2026-09-30; text is the February announcement) | 4 | no | read 2026-10-15 |
| 15 | Nextgov, Gemini 3.1 Pro and 100,000 agents, 2026-04-27 | 3 | no | read 2026-10-15 |
| 16 | DefenseScoop, Grok and ChatGPT join Gemini, 2026-08-31 | 3 | no | read 2026-10-15 |
| 17 | DefenseScoop commentary, "A first look at GenAI.mil," 2026-01-12 | 3 | no | read 2026-10-15 |
| 18 | N.D. Cal. order on cross motions for summary judgment, docket 250, 2026-08-27 | 1 | no | read 2026-10-15 |
| 19 | D.C. Circuit opinion, Anthropic PBC v. United States Department of War, 2026-09-25 | 1 | no | read 2026-10-15 |
| 20 | TechCrunch, district court ruling, 2026-08-28 | 4 | no | read 2026-10-15 |
| 21 | Nextgov, D.C. Circuit ruling, 2026-09-28 | 3 | no | read 2026-10-15 |
| 22 | ChatGPT Mil in-app limits (file sizes, file count, sharing, retention), inside GenAI.mil, no public page | 1 | no | read inside the app on a government machine 2026-10-15, scope (per project, per chat) confirmed the same day; not staged |

## See also

- **In this series:** [← Loops, graphs, and the state between them](23-loops-graphs-and-state) · [Overview](topics/ai-engineering/)
- **[Loops, graphs, and the state between them](23-loops-graphs-and-state)**: the chapter this is a worked case of, and the five state failures it names.
- **[Workflows vs agents](06-workflows-vs-agents)**: a human on both ends is a workflow, and that is the shape the portal gives you.
- **[Memory](08-memory-for-agents)**: what a project with a retention clock is and is not.

## Sources

Staged under `10_staging/genai-mil-work-documents/sources/`, read 2026-10-15.

- War Department, *The War Department Unleashes AI on New GenAI.mil Platform*: https://www.war.gov/News/Releases/Release/Article/4354916/the-war-department-unleashes-ai-on-new-genaimil-platform/
- War Department, *Hegseth Introduces Department to New AI Tool*: https://www.war.gov/News/News-Stories/Article/Article/4355797/hegseth-introduces-department-to-new-ai-tool/
- War Department, *The War Department to Expand AI Arsenal on GenAI.mil With xAI*: https://www.war.gov/News/Releases/Release/Article/4366573/the-war-department-to-expand-ai-arsenal-on-genaimil-with-xai/
- War Department, *GenAI.mil's Rapid Expansion Continues With OpenAI Partnership*: https://www.war.gov/News/Releases/Release/Article/4401775/genaimils-rapid-expansion-continues-with-openai-partnership/
- War Department, *Department of War Launches OpenAI's ChatGPT Mil on GenAI.mil*: https://www.war.gov/News/Releases/Release/Article/4586352/department-of-war-launches-openais-chatgpt-mil-on-genaimil/
- War Department, *Department of War Launches Starshield AI's Grok for Government on GenAI.mil*: https://www.war.gov/News/Releases/Release/Article/4586482/department-of-war-launches-starshield-ais-grok-for-government-on-genaimil/
- War Department, *Government AI Models Advancing, Increasing Productivity*: https://www.war.gov/News/News-Stories/Article/Article/4615232/government-ai-models-advancing-increasing-productivity/
- Marine Corps, *MARADMIN 018/26, Enterprise Generative Artificial Intelligence Availability and Governance*: https://www.marines.mil/News/Messages/Messages-Display/Article/4383324/enterprise-generative-artificial-intelligence-availability-and-governance/
- Department of the Navy CIO, *DON Enterprise IT Service Designation for GenAI.MIL*, joint memo of 2026-01-28: https://www.doncio.navy.mil/ContentView.aspx?ID=20455
- Department of the Navy CIO, *Accelerating AI Adoption, Free Training Resources, and Measuring Time Savings*, memo of 2026-03-16: https://www.doncio.navy.mil/ContentView.aspx?id=20459
- Defense Information Systems Agency, *Department of Defense Cloud Computing Security Requirements Guide*, HTML edition: https://dl.dod.cyber.mil/wp-content/uploads/cloud/SRG/index-old.html
- Google Public Sector, *Gemini for Government: Build custom AI agents for unclassified work on GenAI.mil*: https://cloud.google.com/blog/topics/public-sector/gemini-for-government-build-custom-ai-agents-for-unclassified-work-on-genaimil/
- Google Cloud press release, *Chief Digital and Artificial Intelligence Office Selects Google Cloud's AI to Power GenAI.mil*: https://www.googlecloudpresscorner.com/2025-12-09-Chief-Digital-and-Artificial-Intelligence-Office-Selects-Google-Clouds-AI-to-Power-GenAI-mil
- OpenAI, *Bringing ChatGPT to GenAI.mil*: https://openai.com/index/bringing-chatgpt-to-genaimil/
- Nextgov, *Pentagon adds Google's latest model to GenAI.mil as usage soars*: https://www.nextgov.com/defense/2026/04/pentagon-adds-googles-latest-model-genaimil-usage-soars/413127/
- DefenseScoop, *Grok and ChatGPT join Gemini in Pentagon's enterprise genAI portal*: https://defensescoop.com/?p=131114
- DefenseScoop, *A first look at GenAI.mil: Evolving a secure platform into a warfighting edge*: https://defensescoop.com/?p=123238
- United States District Court, Northern District of California, *Anthropic PBC v. United States Department of War*, order of 2026-08-27 (docket 250): https://storage.courtlistener.com/recap/gov.uscourts.cand.465515/gov.uscourts.cand.465515.250.0_1.pdf
- United States Court of Appeals for the D.C. Circuit, *Anthropic PBC v. United States Department of War*, decided 2026-09-25: https://www.courtlistener.com/opinion/10983481/anthropic-pbc-v-united-states-department-of-war/
- TechCrunch, *Anthropic gets its first court win over the Pentagon's supply-chain risk label*: https://techcrunch.com/2026/08/28/anthropic-gets-its-first-court-win-over-the-pentagons-supply-chain-risk-label/
- Nextgov, *Anthropic loses legal fight to shed DOD's designation as a 'supply-chain risk'*: https://www.nextgov.com/acquisition/2026/09/anthropic-lawsuit-supply-chain-risk/416255/
- ChatGPT Mil, limits shown inside the app on GenAI.mil (Common Access Card only, no public address; read inside the app on 2026-10-15; not staged)
