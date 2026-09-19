# Keep an unedited mirror of Upstream and sync manually

FIKS adapts Matt Pocock's skills and wants to adopt his changes only by deliberate, reviewed decision. So the repo keeps an unedited **Mirror** of Upstream at a recorded commit (`upstream/`) next to the FIKS versions (`skills/`), and a sync is a developer-approved process that diffs the Mirror's old and new commits and records each decision in `docs/decisions/`. Nobody runs `npx skills update` in this repo: we tested it, and it overwrites local edits without a warning or a diff.

## Considered Options

- **GitHub fork and `git merge upstream`**: merges hunk by hunk with nowhere to record decisions, and Upstream reorganises often, so conflicts are noisy.
- **Run `npx skills update` on the FIKS tree**: destroys FIKS edits, then mixes the reverted edits into the upstream changes in `git diff`.
- **Wrap Upstream's Claude plugin**: read-only and Claude-only, so no FIKS edits.
