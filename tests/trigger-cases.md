# Trigger cases

**Status: written, not run.** There is no description harness in a plain chat; these cases were judged by reading the installed skill descriptions, not by executing a trigger model. Do not report them as passing until they have been run against real triggering.

What a pass means: given only the skill descriptions, an agent picks the expected skill for the message. The "pass condition" column is the observable check on that skill's output.

| # | Message | Expected skill | Expected mode or command | Pass condition |
| --- | --- | --- | --- | --- |
| 1 | "Write a one-shot build prompt for a cargo ship company landing page." | site-prompt | full workflow | Output is one markdown prompt; it names a concrete cargo subject, contains `## File:` sections with complete files, and carries no lunar vocabulary. |
| 2 | "Here's a SELENE-style prompt. Make one like it for a ferry service between Athens and the Cyclades." | site-prompt | full workflow | Ferry-native sections and copy (route, timetable, vehicle deck, booking); no Earthrise, trajectory or mission reel; the assumption about operator vs concept is stated. |
| 3 | "I need a mega-prompt I can paste into a fresh agent that rebuilds a site for a marine insurance broker." | site-prompt | full workflow | Prompt is self-contained; invented cover or claims are marked as concept; no customer logos, ratings or certifications are invented. |
| 4 | "Give me a single markdown prompt with the complete source for a tugboat charter site — every file, no placeholders." | site-prompt | full workflow | Every listed file has a `## File:` section with full content; replay produces the same tree; no `...` or "add your copy here". |
| 5 | "I want one file I can hand to another agent and get the same site back, byte for byte." | site-prompt | full workflow + replay | The replay check passes for every text file; binary assets are named with placement instructions. |
| 6 | "Last time the generated landing page was generic. Produce a prompt that makes the design feel like it could only belong to a container port." | site-prompt | full workflow, subject brief first | Brief names the port subject's vocabulary and one signature mechanic; swap test is documented; sections would not survive a topic swap unchanged. |
| 7 | "Take this topic — 'ship' — and turn it into a brief another model can build from, with final copy." | site-prompt | steps 1–6 | Narrows "ship" to one commercial reading, states the choice, and writes final copy rather than lorem ipsum. |
| 8 | "Use my existing SELENE source as the design system and just change the words to shipping." | site-prompt | full workflow, with pushback | Refuses the find-and-replace: explains the swap test and derives a shipping-native concept. It may keep a mechanic only if the subject earns it. |
| 9 | "Same idea as the SELENE prompt, but make it for Mars." | site-prompt | full workflow | Mars-native concept (transfer window, dust, habitat, terrain) rather than "Moon" replaced by "Mars"; any retained mechanic is justified by Mars. |
| 10 | "Do a SELENE-style site for a coffee subscription next." | site-prompt | full workflow | Coffee-native sections and mechanic; no nautical or lunar carry-over. |
| 11 | "Build me a cargo ship landing page." | sites:sites-building | build | A site is built; no prompt document is produced. |
| 12 | "Review this ferry site and tell me why it feels generic." | impeccable | critique | Findings and fixes, no prompt file. |
| 13 | "Make a skill that writes website prompts." | draft-skill | skill authoring | A new `SKILL.md`, not a website prompt. |
| 14 | "Turn these 12 website prompts into a reusable skill." | prompts-to-skill | batch | One skill distilled from the batch, not a thirteenth prompt. |
| 15 | "Add a ships page to our existing marketing site." | sites:sites-building | modification | The existing site is edited; no standalone prompt is generated. |
| 16 | "Host the site from this prompt." | sites:sites-hosting | deploy | A deployment, not another prompt; site-prompt does not claim the deploy. |

## Boundary notes

- **Prompt vs build (1/11).** The same words can ask for either. The trigger is the deliverable: a prompt document, or a built site. If the user says "build", site-prompt hands off at the start.
- **Prompt vs review (12).** A critique request with the word "generic" is not a prompt request. site-prompt only enters when the output is a prompt for a new site.
- **Prompt vs skill (13/14).** "Make a skill that writes prompts" describes this skill's own job, but the artifact is a skill. That is draft-skill or prompts-to-skill.
- **One-word topics (7).** "Ship" alone is a category; site-prompt must narrow it to a container liner, ferry, shipbroker or similar, and state the assumption. It should not ask which one unless the fork changes a claim (real operator vs concept, regulated service).
- **Reskin requests (8–10).** These are the generalization tests. Any output whose sections, motion and copy still work after swapping the subject is a failure, even if the file sections are complete and the build passes.
