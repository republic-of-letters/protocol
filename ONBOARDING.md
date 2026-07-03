# ONBOARDING.md — from zero to your first round

> **For a new member's agent.** Read this if your human has never used GitHub, or
> this machine has never been set up. It takes you from nothing to a working clone,
> then hands you to [`AGENTS.md`](AGENTS.md) for the actual work.
>
> **You are probably reading this from an email, not from the repo** — that's expected.
> The repo is private; you can't see it until you've been invited (Phase 0 below).
> Wherever this guide says `OWNER/REPO`, substitute the repo you were invited to —
> it is named in the invite email. "Your contact" below is the member who sent you
> this guide.

Two kinds of step appear here:

- 🧑 **human step** — needs a person (account signup, browser login). You, the agent,
  cannot do these; explain them to your human and wait.
- 🤖 **agent step** — you can run this yourself once the tools are authenticated.

---

## Phase 0 — Get access (the bootstrap handshake)

This is a four-move handshake between your side and your contact. Nothing else works
until it's done.

1. 🧑 **Create a GitHub account** (skip if your human already has one). You cannot do
   this yourself — it needs a person (an email inbox and a "verify you're human" puzzle).
   So **walk your human through it one step at a time**, waiting for each before the next:
   1. open <https://github.com/signup>;
   2. enter an email address → **Continue**;
   3. create a password → **Continue**;
   4. choose a username — lowercase, no spaces; this becomes their public handle and the
      name needed for the invite → **Continue**;
   5. solve the "are you human?" puzzle;
   6. GitHub emails a short verification code — have them read it back, enter it;
   7. if asked to pick a plan, **Free** is enough; skip the personalization questions.

   **Record the username** — every later step (the invite, the clone) keys off it.

2. 🤖/🧑 **Send the username back to your contact.** Reply on the same email thread this
   guide arrived in with one line: *"GitHub username: `<their-username>`."*

3. ⏳ **Your contact invites that username** as a collaborator with push access. You'll
   know it landed when GitHub emails *"… invited you to collaborate."*

4. 🧑 **Accept the invite.** Easiest: click **Accept** in that email, or open
   `https://github.com/OWNER/REPO` and accept the banner. (Scriptable
   alternative once `gh` is set up — see Phase 1: `gh api /user/repository_invitations`
   to find the id, then `gh api --method PATCH /user/repository_invitations/<id>`.)

After step 4 your human has access to the private repo. Now set up the tools.

## Phase 1 — Install and authenticate the tools

Your human needs **git** and the **GitHub CLI (`gh`)**. `gh` is the painless path: it
logs in through the browser and wires up git so pushing "just works" — no SSH keys.

🤖 **Install** (pick the platform):

```bash
# macOS (Homebrew)
brew install git gh

# Windows (PowerShell)
winget install --id Git.Git -e && winget install --id GitHub.cli -e

# Debian/Ubuntu: git from apt, gh per https://cli.github.com (official apt repo)
sudo apt-get update && sudo apt-get install -y git
# then follow the gh install snippet at https://cli.github.com
```

If a package manager isn't available, the installers at <https://git-scm.com/downloads>
and <https://cli.github.com> work for any OS.

🧑 **Authenticate** (interactive — a person completes the browser step):

```bash
gh auth login
#   → GitHub.com   → HTTPS   → "Login with a web browser"
#   copy the one-time code, paste it in the browser, approve
```

🤖 **Verify it worked** (this is the proof you're in):

```bash
gh auth status
gh repo view OWNER/REPO              # prints the repo's README → you have access
```

If `gh repo view` errors with "Not Found", the invite from Phase 0 step 4 hasn't been
accepted yet — go back and accept it.

## Phase 2 — Clone and identify yourself

🤖

```bash
gh repo clone OWNER/REPO ~/REPO
cd ~/REPO

# set the identity your commits are attributed to (use the human's name/email)
git config user.name  "<human name>"
git config user.email "<the email on the GitHub account>"
```

You now have the whole protocol, the project constitution (`PROJECT.md`), the data
dictionary, the templates, and a worked example locally.

## Phase 3 — The drill: prove the loop works before real work depends on it

Before your first real round, run **one practice round** end to end. This is not
busywork: it verifies — while the stakes are zero — that you (the agent) can follow
the protocol, that CI passes, and that the PR flow works from your machine. Every
member's agent does this once.

🤖 Read [`AGENTS.md`](AGENTS.md) once end to end first. Pay particular attention to:

- **§13 Human gates** — what you may do autonomously and what always needs your
  human's say-so;
- **§5.2 / §7 / §8** — the safety red lines, the code contract, and the
  no-raw-data boundary.

Then, with your human's go-ahead to open the PR (§13 applies even to drills):

```bash
git switch main && git pull
./scripts/new-round.sh "hello <your-handle>"
```

In the new round folder:

1. fill `ASK.md` — kind `analysis`, topic per `topics/` (or `T00` if the project
   says so for drills); the question can be the same as the worked example
   [`exchange/R000-example-hello/`](exchange/R000-example-hello/);
2. adapt the example's `run.py` — it synthesises its own data (seed 42), so it runs
   anywhere with no `DATA_ROOT`;
3. run it, and label the output as the protocol requires:
   `SYNTHETIC — smoke test, do not cite` at the top of the draft `RESULT.md` (§8);
4. `bash scripts/check.sh` — must pass;
5. open the PR, and include this line in the description:
   *"Read AGENTS.md; acknowledged the red lines (§5.2, §8) and the human gates (§13)."*

Your contact reviews and merges it. When the drill PR is merged, you are onboarded.

## Phase 4 — Your first real round

Look at [`topics/`](topics/) to see what's being worked on — every round belongs to
a topic, and your `ASK.md` names it (`topic: T<NN>`). If your question doesn't fit
an existing topic, propose a new one first (copy `topics/_TEMPLATE/`, see AGENTS.md
§12). To write code that runs against the data, read
[`data/SCHEMA.md`](data/SCHEMA.md).

```bash
git switch main && git pull
./scripts/new-round.sh "your short question slug"
# fill ASK.md, write run.py, bash scripts/check.sh, open the PR (AGENTS.md §4)
```

The Runner picks it up, runs it on the real data, and pushes results back into the
same PR.

## If you get stuck

Open an [issue](../../issues/new/choose) on the repo, or reply on the email thread. A
blocker is worth surfacing immediately — don't burn time guessing.
