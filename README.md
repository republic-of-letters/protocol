# protocol

The collaboration protocol of the **Republic of Letters** — a template for research
projects where humans and their AI agents work together across institutions, over
data that never leaves its owner's machine.

The name is the 17th-century one: the *Respublica Literaria*, the long-distance
community of scholars who did science by correspondence. Here the letters are pull
requests. A **round** — one question, one branch, one PR — is a letter: it carries a
question and code out, and a result back. Merged rounds on `main` are the permanent
archive of everything a project tried, including what failed and why.

## The loop

```
        ┌────────────────┐        pull request         ┌────────────────┐
        │   Proposer     │  ── ask + code  ─────────▶   │     Runner     │
        │  (ideas/code)  │                              │  (data/compute)│
        │                │   ◀───── result + review ──  │                │
        └────────────────┘        same pull request     └────────────────┘
                 │                                                │
                 └──────────────  one round = one PR  ────────────┘
```

1. A **topic** is one candidate paper — hypotheses, members, authorship, and a
   decision log, in `topics/T<NN>-<slug>/TOPIC.md`.
2. Each **round** of work is one branch, one folder under `exchange/`, one PR.
3. The Proposer writes the question and the code; the Runner safety-reviews it,
   runs it on the real data, and pushes results back into the *same* PR.
4. **No raw data ever enters the repo** — results are aggregates, figures, small
   tables. CI enforces it.

## Humans decide; agents do the legwork

Most traffic is produced by AI agents acting for the members. Three gates are
always held by people, by design (`AGENTS.md` §13):

1. **Topic gate** — GO / NO-GO / pivot / kill, and authorship (agreed no later
   than GO).
2. **Data gate** — no code runs against real data until the Runner's human passes
   it through the safety gate (automated scan **plus** reading the code), sandboxed.
3. **Merge gate** — nothing enters the permanent record until the humans on both
   sides are content and CI is green.

Any human can stop any round at any time with the `blocked` label. Trust in what a
project publishes is structural: an artifact only reaches `main` — and only from
there any showcase — by passing all three gates.

## Start a project from this template

```bash
gh repo create <owner>/<project-name> --template republic-of-letters/protocol --private --clone
cd <project-name>
```

Then work through **[`SETUP.md`](SETUP.md)** — a ten-minute checklist (fill
`PROJECT.md`, set the Runner in CODEOWNERS, create labels, protect `main`, invite
members). New members — human or agent — start at **[`ONBOARDING.md`](ONBOARDING.md)**;
agents then follow **[`AGENTS.md`](AGENTS.md)**, the complete prescriptive contract.

## What's in the box

```
AGENTS.md            ← the protocol (for humans and agents) — the canonical contract
PROJECT.md           ← per-project constitution: members, Runner, data statement
SETUP.md             ← day-0 checklist for a new project (delete when done)
ONBOARDING.md        ← takes a brand-new member (and their agent) from zero to first round
data/SCHEMA.md       ← data dictionary skeleton: tables, columns, scale rules
topics/_TEMPLATE/    ← copy to open a topic (one candidate paper)
exchange/_TEMPLATE/  ← copy to start a round (the new-round script does it for you)
exchange/R000-…/     ← a fully worked example round, end to end (synthetic data)
scripts/
  new-round.sh       ← scaffold the next round folder + branch
  check.sh           ← the rules CI enforces (also runnable locally)
  scan-round.sh      ← the Runner's static safety triage (AGENTS.md §5.2)
  setup-labels.sh    ← create the standard labels on a fresh repo
.github/             ← PR template, issue forms, CODEOWNERS, the CI workflow
```

## The protocol is versioned

This is **protocol v2.1**. Every project pins the version it was created from (in
`PROJECT.md`) and carries its own copy of `AGENTS.md`. When a project learns
something — a rule that saved it, a rule that got in the way — the fix comes back
here as a PR, and the next project inherits it. Protocol changes are discussed in
this repo's issues.
