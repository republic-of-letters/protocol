# Contributing

This repo has one workflow, and it lives in **[AGENTS.md](AGENTS.md)** — read that.
Who is on this project, who the Runner is, and what data it runs on is in
**[PROJECT.md](PROJECT.md)**.

The one-paragraph version: research directions are **topics** (`topics/T<NN>/TOPIC.md`
— hypotheses, status, decision log). Work inside a topic happens in **rounds** — one
branch, one folder under `exchange/`, one pull request. The Proposer writes `ASK.md`
and the code; the Runner runs it on the real data and pushes results back into the
same PR. No raw data ever enters this repo; results are aggregates, figures, and
small tables. Three decisions are always made by humans, never agents: topic
GO/kill/authorship, code touching the data server, and merging (AGENTS.md §13).

Open a topic: copy `topics/_TEMPLATE/`. Start a round: `./scripts/new-round.sh
"short slug"`. Check it: `bash scripts/check.sh`.
