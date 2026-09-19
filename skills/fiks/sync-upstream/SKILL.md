---
name: sync-upstream
description: Bring changes from Matt Pocock's skills repo into the FIKS skills, one reviewed decision at a time, and record each decision. Maintainers only; run inside the fiks-skills repo.
disable-model-invocation: true
metadata:
  internal: true
---

Sync the FIKS skills with **Upstream** (`https://github.com/mattpocock/skills`). The developer decides every change; you investigate and recommend. Never run `npx skills update` in this repo: it overwrites FIKS edits without asking.

Where things live:

- `upstream/UPSTREAM.md`: the last-synced Upstream commit.
- `upstream/mattpocock-skills/`: the **Mirror**, an unedited copy of Upstream at that commit. Never edit it by hand.
- `skills/`: the FIKS skills. Adapted Upstream skills keep Upstream's relative paths; FIKS-only skills are under `skills/fiks/`.
- `docs/decisions/`: the **Decision log**, one file per sync.
- `docs/adr/` and `CONTEXT.md`: read both before starting.

## 1. Prepare

- Work on a new branch (`sync/upstream-<date>`) with a clean working tree.
- Read `upstream/UPSTREAM.md` for the old commit. Read the three newest decision-log files in full; search older ones by skill name when a change touches that skill.
- Clone Upstream into a temporary folder. Its HEAD is the new commit. If it equals the old one, say so and stop.

## 2. Find what changed

Between old and new commit, in `skills/`, `README.md`, `CHANGELOG.md`, and `.claude-plugin/`: `git diff --stat -M`, `git log --oneline`, and the CHANGELOG entries. Group by skill as added, removed, renamed, or modified. Note changes to the installer instructions or plugin files too.

## 3. Write an impact report per change

For each changed Upstream skill:

- **What changed**: plain language, with the key hunks.
- **FIKS edits in this skill**: diff the Mirror's old copy against the FIKS copy. Then `git merge-file -p <FIKS file> <old Mirror file> <new Upstream file>` for each changed file: does it merge cleanly or conflict?
- **Earlier decisions**: matching decision-log entries. If a change touches something previously rejected, cite that decision and say whether the change gives a real new reason to reopen it.
- **Cross-references**: grep `skills/` for the skill's name (as `/name`, `` `name` ``, and Skill-tool calls). If the change renames or removes a skill, list every FIKS skill that would break.
- **Recommendation**: adopt, adapt, reject, or defer, with the reason.

For a new Upstream skill FIKS lacks: what it does, where it would sit, how it relates to the FIKS workflow, and a recommendation. Also report any change to how Upstream installs or updates skills, and to the lockfile format the installer writes.

## 4. Decide

Walk the developer through the reports one skill at a time, biggest impact first. Ask for a decision on each. Anything unanswered is recorded as `deferred`.

## 5. Apply

1. Refresh the Mirror to the new commit (copy `skills/`, `.claude-plugin/`, `README.md`, `CHANGELOG.md`, `LICENSE` from the clone, no `.git`). Update `upstream/UPSTREAM.md` with the commit and date. Commit this alone: `sync: refresh mirror to <sha7>`. The Mirror moves to the new commit even for changes FIKS rejects.
2. Apply adopted and adapted changes to `skills/`. Use `git mv` for renames.
3. Write `docs/decisions/<date>-sync-<sha7>.md`: one entry per change with the decision (`adopted`, `adapted`, `rejected`, `deferred`), the reason, the files affected, and for rejections a "revisit if" note. Record new FIKS edits too.
4. Verify: `npx skills add . --list` shows the expected skills and not `sync-upstream`; `skills/engineering/setup-matt-pocock-skills/issue-tracker-local.md` and `skills/fiks/fiks-defaults/issue-tracker.md` are identical; every FIKS edit point listed in the Decision log still holds (for example the `fiks-defaults` fallback line in `to-spec`, `to-tickets`, `triage`, `wayfinder`, `code-review`).
5. Commit on the branch.

## 6. Hand off

Do not push or open a pull request without asking. The pull request description links the decision-log file. Two approvals are required, set as a branch policy on the host.
