---
name: fiks-doctor
description: "Diagnose and repair a developer's install of the FIKS skills: missing, outdated, or overwritten skills, symlink problems, missing bash. Use when FIKS skills look broken, missing, or out of date, or after installing skills from another source."
---

Check that the FIKS skills installed on this machine match the FIKS repo, tell the developer what is wrong in plain language, and repair it once they agree.

**FIKS source:** `https://github.com/MadsAkselsen/fiks-skills` (the one place this address is written; change it here when the repo moves).

Use whichever shell is available. The commands below are the same in PowerShell and bash unless noted.

## 1. See what is installed

Run `npx skills list -g` and `npx skills list`. Note each skill's name and folder.

## 2. Get the FIKS versions

Clone the source into a temporary folder: `git clone --depth 1 <FIKS source> <temp folder>`. The FIKS skills are the folders under `skills/` that hold a `SKILL.md`, except those with `internal: true` in their metadata.

## 3. Compare

For every FIKS skill, compare its installed folder with the cloned folder (`git diff --no-index`, `diff -r`, or `Compare-Object`) and classify it:

- **Missing**: not installed.
- **Matches**: identical to FIKS.
- **Differs**: outdated, edited by hand, or replaced by a same-named skill from another source. The lockfile `~/.agents/.skill-lock.json` lists a `source` per skill; use it as a hint for which of the three, not as proof.

Ignore skills whose names FIKS does not use. Those are the developer's own or come from other sources, and are none of your business.

## 4. Check the environment

- Symlinks: are installed skill folders dangling or unreadable links?
- `bash --version`: `/wizard` needs it (Git Bash or WSL on Windows).
- `node --version` and `npx --version`.

## 5. Report and repair

Show a short table (skill, state) and what you propose, in plain language. Repair only after the developer agrees:

- Missing or differing skills: `npx skills add <FIKS source> -g -y`. Add `--copy` if symlinks fail.
- A skill that stays wrong afterwards: `npx skills remove <name> -g`, then add FIKS again.

Do not use `npx skills update -g` to repair a replaced skill: it follows whichever source the skill was last installed from, so it keeps the wrong version. Never remove a skill FIKS does not own.

Delete the temporary clone when done.
