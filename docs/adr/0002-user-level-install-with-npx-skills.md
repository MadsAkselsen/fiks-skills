# Install FIKS skills at user level with `npx skills`

Developers install once with `npx skills add MadsAkselsen/fiks-skills -g`, which works for any agent, and update with `npx skills update -g`. Nothing is committed into the hundreds of consumer repos, so skills cannot drift between repos. Developers keep their own skills and their own user-level `AGENTS.md`; they do not edit FIKS skills in place, because an update overwrites the edit (to customise one, copy it under a new name or propose the change here).

## Considered Options

- **Skills committed into each repo**: hundreds of copies to keep in sync, and every developer gets the same set whether they want it or not.
- **A Claude Code plugin marketplace**: the easiest install and update for Claude Code, but FIKS developers use different agents.

## Consequences

The installer tracks each skill's source, so installing another source's skill with the same name replaces the FIKS one, and `update` will not undo that. The `fiks-doctor` skill and the README cover repair. Moving to Azure DevOps means each developer re-runs the install once with the new URL.
