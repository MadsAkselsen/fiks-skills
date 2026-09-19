# Issue tracker: Local Markdown

Specs and tickets live as markdown files in a **scratch root**. Nothing here touches Jira or any other tracker.

## Finding the scratch root

Start at the current working directory and go up. The first folder whose name looks like a Jira key (`^[A-Z][A-Z0-9_]*-[0-9]+$`, for example `FIKS3-3465`) is the **Feature workspace**; its `.scratch/` folder is the scratch root (create it if missing). Every repo checked out inside the Feature workspace shares that scratch root.

If no folder up the path looks like a Jira key, use `.scratch/` at the root of the current repo. Keep it out of git through the repo's local exclude file (`git rev-parse --git-path info/exclude`), not the tracked `.gitignore`.

## Conventions

- One effort per directory: `<scratch-root>/<slug>/`
- The spec is `<scratch-root>/<slug>/spec.md`
- Implementation issues are one file per ticket at `<scratch-root>/<slug>/issues/<NN>-<ticket-slug>.md`, numbered from `01`, never a single combined tickets file
- Each ticket file has a `Repos:` line naming the repo folder(s) it changes and their branches
- Triage state is recorded as a `Status:` line near the top of each issue file (see `triage-labels.md` for the role strings)
- Comments and conversation history append to the bottom of the file under a `## Comments` heading
- Refer to other files by path relative to the Feature workspace, never by absolute path

## When a skill says "publish to the issue tracker"

Create a new file under `<scratch-root>/<slug>/` (creating the directory if needed).

## When a skill says "fetch the relevant ticket"

Read the file at the referenced path. The user will normally pass the path or the ticket number directly.

## Wayfinding operations

Used by `/wayfinder`. The **map** is a file with one **child** file per ticket.

- **Map**: `<scratch-root>/<effort>/map.md` (the Notes / Decisions-so-far / Fog body).
- **Child ticket**: `<scratch-root>/<effort>/issues/NN-<slug>.md`, numbered from `01`, with the question in the body. A `Type:` line records the ticket type (`research`/`prototype`/`grilling`/`task`); a `Status:` line records `claimed`/`resolved`.
- **Blocking**: a `Blocked by: NN, NN` line near the top. A ticket is unblocked when every file it lists is `resolved`.
- **Frontier**: scan `<scratch-root>/<effort>/issues/` for files that are open, unblocked, and unclaimed; first by number wins.
- **Claim**: set `Status: claimed` and save before any work.
- **Resolve**: append the answer under an `## Answer` heading, set `Status: resolved`, then append a context pointer (gist + link) to the map's Decisions-so-far in `map.md`.
