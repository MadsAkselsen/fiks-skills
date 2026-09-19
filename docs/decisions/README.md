# Decision log

One file per sync with Upstream: `YYYY-MM-DD-sync-<sha7>.md` (the first one is the baseline). Each entry records what Upstream changed (or what FIKS deliberately changed on its own), what was decided, and why, so settled questions are not reopened without a real new reason.

Decisions are one of:

- **adopted**: taken from Upstream as-is.
- **adapted**: taken with FIKS changes.
- **rejected**: not taken. Carries a "revisit if" note.
- **deferred**: not yet decided.

Bigger, harder-to-reverse choices about how this repo works live in `docs/adr/`.
