---
project:  <slug>                # short name, matches the repo name
protocol: v2.1                  # pinned; upstream = republic-of-letters/protocol
created:  2026-01-01
visibility: display-only        # display-only | apply-to-join | open  (what the showcase may say)
---

# PROJECT.md — the project constitution

`AGENTS.md` is the invariant protocol; this file is everything specific to *this*
project. An agent reads this second (AGENTS.md §14). Keep it current — it is the
single place where members, the Runner, and the data statement live.

## Direction

<One sentence: what this project is trying to find out.>

## Members

| Handle       | Side      | Brings                                   | Mentor |
| ------------ | --------- | ---------------------------------------- | ------ |
| `@<handle>`  | data side | the dataset, the compute; the **Runner** | —      |
| `@<handle>`  | idea side | hypotheses, code                         | —      |

- **Runner:** `@<handle>` — the only party that touches the dataset (AGENTS.md §2).
- A member with a `Mentor` entry is a mentored member: the mentor's approval is part
  of the merge gate for that member's rounds (AGENTS.md §2, §13).
- Membership changes are a decision — record them in the table *and* note who
  approved, in the PR that edits this file.

## Data statement

| Field | Value |
| ----- | ----- |
| Core dataset(s) | <name, one line each> |
| Physical location | <"the Runner's server" — never a path or hostname> |
| Licence / usage terms | <source terms, redistribution rights, citation requirements> |
| Sensitive content | <personal data? de-anonymisation risk? what §5.2 content-safety should watch for> |
| Restricted-licence side data | <e.g. WRDS on @handle's machine — stays off-repo per AGENTS.md §15> |

Schema and read patterns: [`data/SCHEMA.md`](data/SCHEMA.md).

## Conventions

- Sampling seed: `42` unless a round's `ASK.md` says otherwise.
- <anything else project-specific: units, timezone of timestamps, main sample flag…>

## Visibility & showcase

What may leave this repo (all publication passes the merge gate — a sanitized
card/summary leaves via PR, never directly):

- **Tier:** `display-only` — title + abstract + published artifacts only /
  `apply-to-join` — the above + how-to-join instructions /
  `open` — the repo itself is public.
- **Approved for public display:** <nothing yet | list of merged rounds/figures>
