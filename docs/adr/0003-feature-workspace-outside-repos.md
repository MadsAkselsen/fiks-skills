# Keep a Feature's specs and tickets in a Jira-key folder outside the repos

Upstream's local tracker writes `.scratch/<feature>/` inside the current repo. A FIKS Feature spans several repos and branches, so specs and tickets would be split across repos and would vanish when a branch is switched. Instead they live in `.scratch/` in the **Feature workspace**: the folder named after the Jira key (for example `FIKS3-3465`) that holds a checkout of every repo the Feature touches. Skills find it by going up from the current folder to the nearest parent named like a Jira key, so the folder can live anywhere. Without one, they use a locally excluded `.scratch/` in the repo.

A Feature has one developer at a time. Passing it on is a handover: send the `.scratch/` folder and the `/handoff` document, and push the branches. Nothing touches Jira.

## Considered Options

- **`.scratch/` committed in each repo** (Upstream's default): splits a Feature across repos and loses files on branch switches.
- **`.scratch/` ignored in each repo**: survives branch switches but is still split across repos.
- **A shared planning repo** so several developers see the same tickets: parked; the layout is kept compatible with it.
- **Specs and tickets in Jira**: parked, to avoid disturbing the existing Jira setup.
