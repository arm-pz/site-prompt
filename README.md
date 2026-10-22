# site-prompt

An agent skill that turns a website topic into one self-contained, paste-and-build prompt. Give it "cargo ship", "a ferry between Athens and the Cyclades" or "a marine insurance broker", and it returns one markdown file with the concept, final copy, complete source for every file, and the checks that were actually run — enough for a fresh agent to rebuild the site without seeing the session that produced it.

It does not stop at an outline or a list of sections. It narrows a category to one concrete subject, derives the page from that subject's world, builds and verifies the site in a scratch folder first, then embeds each file verbatim under a `## File:` heading. The included replay verifier materialises the prompt into a clean directory and compares it against the built site, so "complete source" is tested rather than claimed.

## Install

```
npx skills add arm-pz/site-prompt --skill site-prompt
```

Or clone this repository into `~/.qoder/skills/site-prompt` (Qoder) or `~/.claude/skills/site-prompt` (Claude Code), then start a new session. The skills CLI resolves this repo; `--list` reports one skill.

## Ask it things like

- "Write a one-shot build prompt for a cargo-ship company landing page."
- "Here's a SELENE-style prompt. Make one like it for a ferry between Athens and the Cyclades."
- "I need a mega-prompt that rebuilds a tugboat charter site — every file, no placeholders."
- "Produce a prompt for a marine insurance broker whose design could only belong to that subject."

## What a run produces

One markdown file, in this order: the subject brief and the page's job; the load-bearing behaviours with their contracts; hard build constraints; a pinned stack with exact versions; code-native asset rules; the steps and verify commands; one `## File:` section per file with the complete contents; design notes; and the checks that ran, with the remainder listed as not verified.

Every prompt has to pass two tests before it ships:

- **The swap test.** Replace the subject with something unrelated. If the sections, motion and copy still work, the concept is a template with nouns swapped and gets redesigned.
- **The replay check.** `scripts/replay-prompt.mjs` extracts every file section, writes the tree, and compares it against the built site byte for byte, with line endings normalised. The replayed tree is then built and served.

## Replay verifier

```bash
node scripts/replay-prompt.mjs prompt.md --list
node scripts/replay-prompt.mjs prompt.md --out ../clean-dir
node scripts/replay-prompt.mjs prompt.md --check ../built-site
```

`--out` refuses a non-empty directory. `--check` reports missing, differing and extra files; binary assets are named but cannot be compared. The cases it was tested against are in `tests/replay-cases.md` (14 run).

## Example

Real output from the first end-to-end run, a concept container line called Northline:

```
$ node scripts/replay-prompt.mjs prompts/cargo-ship.md --list
 477  index.html
1611  styles.css
 299  main.js
   5  favicon.svg

4 files, 66305 bytes
```

The prompt carries all 66,305 bytes of source, and `--check` against the built directory reports `4 match, 0 differ, 0 missing`. The prompt itself stays in the project that produced it; this repository ships the skill and the verifier.

## Boundaries

- **Handles:** a prompt for a new marketing or concept site, with final copy and verified source.
- **Does not handle:** building or hosting the site (that is the site tooling's job, not this skill's), reviewing or redesigning an existing page (`impeccable`, `frontend-design`), or authoring a skill ([`draft-skill`](https://github.com/arm-pz/draft-skill)).
- **Sibling:** [`tune-skill`](https://github.com/arm-pz/tune-skill) audits a skill that already exists; this skill produces a website prompt.

The name is shared with several unrelated, empty repositories. This is the agent skill from [arm-pz](https://github.com/arm-pz). `SitePrompter` goes in the opposite direction — it turns a site into a prompt.

## Status

`SKILL.md` passes tune-skill's validator: 0 fail, 0 warn, 609 description characters, 66 body lines. The replay verifier has 14 recorded cases. The trigger cases in `tests/trigger-cases.md` are written but were not run against a live trigger model, and nothing here claims otherwise.

End-to-end runs so far: SELENE (21 files, 21/21 replay match) and Northline / Rotation 7E, a concept container line (4 files, 4/4 replay match).

## License

MIT. See `LICENSE`.
