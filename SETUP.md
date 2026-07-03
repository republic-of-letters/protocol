# SETUP.md — day-0 checklist for a new project

You just created a repo from `republic-of-letters/protocol`. Ten minutes of setup
turns it into a working project. Do these in order; delete this file when done.

An agent can do all of it except where marked 🧑.

## 1. Fill `PROJECT.md`

The constitution: direction, member table, who the Runner is, the data statement,
visibility tier. Everything else references it.

## 2. Point CODEOWNERS at the Runner

Edit `.github/CODEOWNERS` — replace the placeholder with the Runner's handle. This
is what auto-notifies the Runner the instant any round opens.

## 3. Fix the placeholder URLs

- `.github/ISSUE_TEMPLATE/config.yml` — replace `OWNER/REPO` with this repo's path.

## 4. Fill `data/SCHEMA.md`

The data dictionary: canonical table names, grain, approximate sizes, key columns,
and — critically — which tables are **never load whole**. Then open
`scripts/scan-round.sh` and set `BIG_TABLES_ERE` to match those table names, so the
safety scan flags eager loads of them.

## 5. Create the labels

```bash
bash scripts/setup-labels.sh          # round:running/answered, blocked, priority:high, question, topic:T01
```

## 6. Protect `main`

CI green + one approving review before merge; no force pushes. 🧑 confirm with the
repo owner, then:

```bash
gh api --method PUT "repos/{owner}/{repo}/branches/main/protection" \
  --input - <<'JSON'
{
  "required_status_checks": {"strict": false, "contexts": ["check"]},
  "enforce_admins": false,
  "required_pull_request_reviews": {"required_approving_review_count": 1},
  "restrictions": null,
  "allow_force_pushes": false,
  "allow_deletions": false
}
JSON
```

## 7. Invite the members

🧑 The repo owner invites each member listed in `PROJECT.md`:

```bash
gh api --method PUT "repos/{owner}/{repo}/collaborators/<handle>" -f permission=push
```

## 8. Send `ONBOARDING.md` to new members

Email it (it is written to be read *before* the reader has repo access). Tell them
which repo they were invited to — the guide says `OWNER/REPO` where it means this
repo. A member's agent finishes onboarding by completing **the drill** (a hello
round, see ONBOARDING Phase 3) — don't skip it; it is how you find out whether
their agent can follow the protocol *before* real work depends on it.

## 9. Open T01

Copy `topics/_TEMPLATE/` to `topics/T01-<slug>/`, fill `TOPIC.md` (status
`proposing`), PR it. First real round follows.

---

Protocol questions or improvements → issues/PRs on
[`republic-of-letters/protocol`](https://github.com/republic-of-letters/protocol).
