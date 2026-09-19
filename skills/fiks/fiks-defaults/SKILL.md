---
name: fiks-defaults
description: The FIKS default issue tracker (local markdown), triage labels, and domain doc layout, for repos with no docs/agents config. Use when a skill needs the issue tracker, triage labels, or domain docs and none were provided.
---

FIKS repos do not need a setup run. When a skill needs the issue tracker, the triage labels, or the domain doc layout, and the repo has no `docs/agents/*.md` file for it, use the FIKS default below.

A repo's own `docs/agents/*.md` files, when present, always win over these defaults.

- **Issue tracker**: local markdown files in the Feature workspace's `.scratch/` (or the repo's own `.scratch/` when there is no Feature workspace). Read [issue-tracker.md](./issue-tracker.md) for how to find the scratch root and where specs, tickets, and wayfinding maps go.
- **Triage labels**: the five canonical roles, each label equal to its name. Read [triage-labels.md](./triage-labels.md).
- **Domain docs**: single-context (`CONTEXT.md` and `docs/adr/` at the repo root). Read [domain.md](./domain.md).

Do not tell the user to run `/setup-matt-pocock-skills`. Do not call Jira or any other tracker.
