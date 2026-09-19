## This repo

`fiks-skills` distributes the FIKS skills. Read `CONTEXT.md` for the vocabulary and `docs/adr/` for why it is built this way.

- `skills/` is what developers install: FIKS versions of Matt Pocock's skills at Upstream's relative paths, and FIKS-only skills under `skills/fiks/`.
- `upstream/mattpocock-skills/` is the unedited Mirror. Never edit it by hand.
- `docs/decisions/` is the Decision log. Read the newest entries before changing an adapted skill.
- **Never run `npx skills update` here**: it overwrites FIKS edits without asking.
- To sync with Upstream, read and follow `skills/fiks/sync-upstream/SKILL.md`.
- Keep `skills/engineering/setup-matt-pocock-skills/issue-tracker-local.md` identical to `skills/fiks/fiks-defaults/issue-tracker.md`.

## Agent skills

This repo overrides the FIKS default so that maintainers' own plans stay in the repo.

### Issue tracker

Issues are local markdown files under `.scratch/<feature>/`. See `docs/agents/issue-tracker.md`.

### Triage labels

Default five-role vocabulary (`needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`). See `docs/agents/triage-labels.md`.

### Domain docs

Single-context: one `CONTEXT.md` and `docs/adr/` at the repo root. See `docs/agents/domain.md`.
