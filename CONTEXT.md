# FIKS Skills

The FIKS developers' curated set of agent skills: Matt Pocock's skills, adapted where FIKS wants it, plus skills FIKS writes itself. It exists so every FIKS developer gets the same workflow on any agent, while Matt's ongoing changes are adopted only by deliberate decision.

## Language

**Upstream**:
Matt Pocock's public skills repository, the origin of the skills FIKS adapts.
_Avoid_: Matt's repo, source repo

**Mirror**:
An unedited copy of Upstream as of a recorded commit, kept in this repo so that any change in Upstream can be seen exactly.
_Avoid_: Vendor copy, snapshot

**FIKS skills**:
The skills this repo distributes to developers: adapted Upstream skills plus skills that FIKS wrote itself.
_Avoid_: Our skills, the fork

**Baseline**:
The Upstream commit the FIKS skills started from; the first Mirror commit and the first Decision log entry.
_Avoid_: Initial version, v1

**Sync**:
The manual, developer-approved process of bringing changes in Upstream into the FIKS skills. Never automatic.
_Avoid_: Update, pull, merge

**Decision log**:
The record of how each Upstream change was handled in a Sync (adopted, adapted, or rejected) and why, so settled questions are not reopened without reason.
_Avoid_: Changelog, sync history

**Developer**:
A person at FIKS who uses the FIKS skills, and who may also add their own skills alongside them.
_Avoid_: User, consumer

**Consumer repo**:
Any repository in which a Developer works with the FIKS skills. FIKS has hundreds.
_Avoid_: Target repo, project repo

**FIKS default**:
The conventions the FIKS skills assume in a Consumer repo that has no configuration of its own: a local-markdown issue tracker, the standard triage labels, and a single-context domain doc layout.
_Avoid_: Fallback, baseline config

## Work

**Feature**:
A unit of work tracked by exactly one Jira issue. It may span several repos and branches, but is worked by one Developer at a time; passing it to another Developer is a handover.
_Avoid_: Jira ticket, epic, project

**Feature workspace**:
A folder named after a Feature's Jira key. It holds the checkouts of every repo the Developer touches for that Feature, together with the Feature's local specs and tickets, which every repo inside it shares.
_Avoid_: Work folder, ticket folder

**Spec**:
The written description of what a Feature should do, produced from a grilling session and kept as a local file.
_Avoid_: PRD, requirements doc

**Ticket**:
One small, independently buildable slice of a Feature, kept as a local file. Not a Jira issue.
_Avoid_: Issue, task, Jira ticket

**Scratch root**:
The `.scratch/` folder where a Feature's specs and Tickets are kept: the one in the Feature workspace, or the repo's own when there is no Feature workspace.
_Avoid_: Tracker folder, work directory

**Handover**:
Passing a Feature to another Developer. The sender sends the scratch root and a handoff document and pushes the branches.
_Avoid_: Reassignment, transfer

## Configuration

**Repo config**:
Optional files in a Consumer repo that override the FIKS default for that repo only. Most Consumer repos have none.
_Avoid_: Setup files, agent docs
