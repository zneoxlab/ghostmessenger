# GhostMessenger Product Design

Date: November 1, 2026

Status: Product direction approved by the owner; implementation has not started.

## Purpose and audience

Help an individual developer turn material from a public GitHub repository into a checked project introduction without repeatedly copying context between separate AI chats. Reimagine the retired MSN / Windows Live Messenger interaction model as a human-managed AI team.

## First release

- Browser-based application with an original messenger-era buddy list and conversation view.
- Three fixed contacts: Researcher, Writer, Reviewer.
- One workflow: public GitHub source material → evidence notes → draft → review → user approval → Markdown download.
- English interface and demonstration content, with Chinese documentation available separately if useful.
- Explicit progress, visible handoffs, failure recovery, and durable artifacts.

Full autonomous programming, shell execution, private repository access, general-purpose agent creation, payments, and reconnection to the historical Messenger network are outside this first release.

## Main interaction

1. A new task asks for a repository URL and a short audience / purpose brief.
2. The server validates the URL and fetches bounded public source material. Initially support the README and repository metadata; retain file paths, revisions, and URLs.
3. Researcher produces evidence notes. Every claim that will be presented as a repository fact references a fetched source.
4. Writer produces a versioned Markdown draft from the brief and evidence notes.
5. Reviewer checks claims against those notes, marking issues and the scope of the check. It does not claim to have tested software.
6. The user edits the brief or asks for revision, or approves a specific artifact version.
7. Export retrieves the approved stored content; it does not regenerate content.

## Messenger interactions with a job to do

- **Presence:** Available, Working, Waiting for you, Failed, Done reflect persisted stage state.
- **Messages:** Real execution events explain what was read, handed off, and produced.
- **File transfer:** Each handoff references an immutable artifact revision.
- **Nudge:** A quiet visual notification draws attention to an approval request or failure. Sound is optional and off by default.
- **Reconnect:** Reopening the page restores the current run, transcript, and artifacts.

## Architecture

Plan for a TypeScript browser client, a small HTTP API, a server-side workflow runner, and a durable relational store. The browser must not hold provider credentials. The server owns stage transitions and stores artifacts before it announces completion. Start with a sequential workflow and bounded retries; add parallel work only if evidence shows a need.

Suggested records:

- Run: ID, brief, repository URL / revision, current stage, status, timestamps.
- Stage attempt: ID, run ID, role, attempt number, input artifact IDs, status, provider request reference, error.
- Message / event: monotonically ordered ID, run ID, role, type, content, timestamp.
- Artifact: ID, run ID, kind, revision, content, source references, producing stage attempt.
- Approval: ID, run ID, artifact ID / revision, decision, timestamp.

Transitions are transactional. The server permits only one active attempt for a stage. A retry creates a new attempt; it must not overwrite an approved artifact. A changed brief creates a new revision and invalidates the affected downstream stages.

## Failure behavior

- Invalid or unavailable repository: explain the failure and preserve the brief for correction.
- Missing source evidence: flag uncertainty rather than inventing facts.
- Model timeout or rate limit: preserve completed work and offer a bounded retry.
- Browser disconnect: processing state remains on the server; reload restores it.
- Review finds unsupported claims: show the issues and allow revision before approval.
- Nothing approved: make the approval requirement clear and do not label a download as approved.

## Safety, accessibility, and originality

Only fetch supported GitHub hosts and paths, enforce size and time limits, and prevent redirects into private network addresses. Treat fetched text as untrusted source data. Render generated Markdown without executing raw HTML or scripts. Ensure keyboard access, visible focus, readable status labels, and a narrow-screen layout. Use original assets instead of Microsoft logos, sounds, or copied UI artwork.

## Acceptance demonstration

Give the app a public repository, watch actual research / writing / review complete, correct one unsupported claim, approve a final revision, download it, and reload to verify that the same run and approved artifact are restored. The downloadable text must exactly match the approved text shown in the UI.

## Honest evidence

A working demo, real model integration, Kiro development sessions, automated checks, and visual verification are all pending. Record each only after it happens. A repository and specifications are preparation, not a completed competition entry.
