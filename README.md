# fiks-skills

The FIKS developers' agent skills: [Matt Pocock's skills](https://github.com/mattpocock/skills), adapted where FIKS decided to, plus skills FIKS writes itself. Works with any agent that supports skills (Claude Code, Codex, and others).

> **Status: work in progress.** This README describes the agreed setup. The repo is still being restructured to match it; see `docs/decisions/` once it exists. Terms used here are defined in [CONTEXT.md](./CONTEXT.md).

## Install

One command, once per developer:

```bash
npx skills add MadsAkselsen/fiks-skills -g
```

Choose your agent(s) when asked. The skills are installed at user level, so they are available in every repo. Nothing is added to the repos you work in.

Update whenever you like:

```bash
npx skills update -g
```

- **Your own skills are safe.** Skills you wrote yourself are never touched. Skills you installed from other sources are updated from *their own* source, not from FIKS.
- **Don't edit FIKS skills in place.** An update overwrites the edit without warning. To customise one, copy it under a new name, or propose the change to this repo.
- **Don't also install Matt Pocock's skills separately.** They share names with the FIKS versions, so one install would overwrite the other. The FIKS versions already contain them.
- **Your own `AGENTS.md`** (in your home folder, for your agent) is yours. No FIKS skill writes to it.

### If you overwrote the FIKS skills by mistake

Installing Matt Pocock's skills (or any source with the same skill names) replaces the FIKS versions, and `npx skills update -g` will *not* bring them back, because it now follows the other source. Install FIKS again:

```bash
npx skills add MadsAkselsen/fiks-skills -g
```

Same-named skills go back to the FIKS versions. If a skill still looks wrong afterwards, remove it and add FIKS again.

### Windows

- If the installer reports that it can't create symlinks, add `--copy` to the install command.
- `/wizard` generates bash scripts, so it needs `bash` (Git Bash, which comes with Git for Windows, or WSL). `/diagnosing-bugs` mentions a bash script only as a last-resort option; everything else in it works in PowerShell.
- If something looks broken or out of date, ask your agent to run `/fiks-doctor`.

## Where a Feature's files live

Each Feature has one Jira issue and one developer at a time. Work on it in a folder named after the Jira key, with one checkout per repo you touch. The folder can live anywhere; only its name matters.

```
C:\Work\Opgaver\FIKS3-3465\    <- Feature workspace
├── repo1\                     <- on its own branch
├── repo2\                     <- on its own branch
└── .scratch\
    └── <slug>\
        ├── spec.md
        └── issues\
            ├── 01-….md        <- Tickets; each lists its Repos: (repo -> branch)
            └── 02-….md
```

Every repo in the folder shares the same specs and Tickets. Skills find `.scratch/` by starting where the agent runs and going upward until they reach a folder named like a Jira key (uppercase letters or digits, a dash, a number, e.g. `FIKS3-3465`). So you can start the agent in `FIKS3-3465` itself or inside `FIKS3-3465\repo1`. Nothing here touches Jira.

Small one-repo work with no Feature workspace still works: the skills fall back to a gitignored `.scratch/` inside the repo.

## The normal workflow

Start your agent in the Feature workspace folder or inside one of its repos, then run these one after another. Inside a repo, git commands and `CONTEXT.md` work directly. In the workspace folder the agent sees every repo and moves into each one as needed. Invoke skills by name; in Claude Code that is `/grill-with-docs` and so on.

1. **`/grill-with-docs`**: the agent interviews you about the idea until it is sharp. It records terms in the repo's `CONTEXT.md` and hard-to-reverse decisions in `docs/adr/`.
2. **`/to-spec`**: turns the conversation into a spec at `.scratch/<slug>/spec.md`. No further interview.
3. **`/to-tickets`**: splits the spec into small tracer-bullet Tickets in `.scratch/<slug>/issues/`, each with blocking edges and the repos it touches. You approve the breakdown.
4. **`/implement`**: builds one Ticket test-first, then runs the review. Start each Ticket in a **fresh context** (`/clear`), working blockers first.
5. **`/code-review`**: reviews the diff against the repo's standards and against the spec. `/implement` calls it for you; run it yourself for a branch or PR. For a Ticket that spans several repos, review each repo.

Keep steps 1 to 3 in one unbroken conversation so the spec and Tickets build on the same thinking.

Other skills you may want: `/diagnosing-bugs`, `/tdd`, `/prototype`, `/wayfinder` (a huge foggy effort), `/handoff`. `/ask-matt` recommends a skill for your situation.

### Repo config is optional

Skills that need to know where issues live or which triage labels to use assume the **FIKS default**: local markdown files and the standard labels. A repo that differs adds its own `docs/agents/*.md` files, which `/setup-matt-pocock-skills` can create.

## Handing a Feature to another developer

`/handoff` is a general-purpose skill (it summarises a conversation for the next agent). For a Feature it works like this. The sender:

1. Commits and **pushes the branches** of every repo in the Feature workspace.
2. Runs `/handoff` with an argument that makes it portable:

   ```
   /handoff Handover of FIKS3-3465 to another developer. Refer to specs and Tickets by path relative to the Feature workspace (.scratch/<slug>/…). List every repo with its pushed branch, which Tickets are done, and what to do next.
   ```

3. Sends the receiver two things: the `.scratch` folder from the Feature workspace and the handoff document (`/handoff` writes it to your system's temp folder and prints the path).

The receiver:

1. Creates a folder named `FIKS3-3465` anywhere they like (their root can differ from the sender's) and puts `.scratch` in it.
2. Checks out each repo from the handoff document into that folder, on the pushed branch.
3. Starts an agent in the Feature workspace, gives it the handoff document, and continues with the next Ticket via `/implement`.

## For maintainers

Matt Pocock's skills change over time and FIKS adopts those changes only by deliberate decision.

- `upstream/mattpocock-skills/` is an unedited copy of his repo at a recorded commit.
- `skills/engineering/` and `skills/productivity/` are the FIKS versions of his skills, at the same relative paths as his. `skills/fiks/` holds skills FIKS wrote itself.
- `docs/decisions/` has one file per sync: every upstream change, whether it was adopted, adapted or rejected, and why.
- To sync, work on a branch and follow `skills/fiks/sync-upstream/SKILL.md` (an internal skill, left out of the standard install; `AGENTS.md` points agents at it). It fetches his latest commit and reports, per changed skill, what changed, how it interacts with FIKS edits, what earlier decisions say, and what other skills it might break. You decide each change; the pull request needs two approvals.
- **Never run `npx skills update` inside this repo.** It overwrites FIKS edits without asking.
