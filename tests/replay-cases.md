# Replay verifier cases

Run on 2026-10-22 against `scripts/replay-prompt.mjs` (Node 24.18.0, Windows), including a re-run after the parser was made fence-aware. Fixtures in a temp folder; SELENE refers to the original one-shot prompt and the committed source at `landingpage/selene`.

| # | Case | Command shape | Expected | Observed |
| --- | --- | --- | --- | --- |
| 1 | List SELENE sections | `prompt --list` | 21 files | 21 files, 51,578 bytes, exit 0 |
| 2 | Materialize SELENE | `prompt --out <fresh>` | 21 files written | 21 files, 51,578 bytes, exit 0 |
| 3 | Check replay against the committed source | `prompt --check <landingpage/selene>` | every section matches | 21 match, 0 differ, 0 missing; 17 binary assets, 0 extra text; exit 0 |
| 4 | Mutated source file | append one line to `src/content.ts`, then check | diff reported, exit 1 | `DIFF src/content.ts (line 69)`, exit 1 |
| 5 | Missing files | two-section prompt checked against an empty dir | two missing, exit 1 | 2 missing, exit 1 |
| 6 | Stray text file | add `notes.txt` to a two-file replay | extra reported, exit 1 | `EXTRA notes.txt (text file not in prompt)`, exit 1 |
| 7 | CRLF source | convert a replayed file to CRLF, then check | still matches | 2 match; the run's exit 1 came only from the stray file added in case 6 |
| 8 | Non-empty output dir | `--out` into a populated dir | refuses, exit 2 | `... is not empty; pass a fresh directory`, exit 2 |
| 9 | Parent path | `## File: \`../outside.txt\`` | refuses, exit 2 | `path must be relative and normalized`, exit 2 |
| 10 | Absolute path | `## File: \`C:/outside.txt\`` | refuses, exit 2 | `absolute path`, exit 2 |
| 11 | Duplicate section | same path twice | refuses, exit 2 | `duplicate file section "a.txt"`, exit 2 |
| 12 | No code fence | heading followed by prose | refuses, exit 2 | `expected a fenced code block right after the heading`, exit 2 |
| 13 | Two code blocks | two fences under one heading | refuses, exit 2 | `more than one code block (or text) after the first one`, exit 2 |
| 14 | Markdown source with inner headings | `README.md` containing `## File: \`fake.txt\`` and nested fences | one section, inner text preserved | 1 file, 109 bytes, generated README keeps its inner fences |

No case writes outside the target directory. Binary assets are reported but do not fail the check, because they cannot be embedded as text; they belong in the prompt's Assets section.

The SELENE comparison is the important one: it proves the prompt's file sections reproduce the built site exactly, not approximately.
