# GhostMessenger Requirements

## Requirement 1: Start a bounded task

As an individual developer, I want to provide a public GitHub repository and a brief so that an AI team can prepare a useful introduction.

- WHEN a supported public repository URL and brief are submitted, THE SYSTEM SHALL create a persistent run with a unique ID.
- IF the URL is unsupported, unavailable, or exceeds resource limits, THE SYSTEM SHALL explain the failure without fabricating repository contents.
- THE SYSTEM SHALL preserve source URLs, paths, and revisions for retrieved material.

## Requirement 2: Coordinate three assistants

As a user, I want to know who is working and what is handed off so that I can understand the result.

- THE SYSTEM SHALL provide Researcher, Writer, and Reviewer as three fixed contacts.
- WHEN an assistant changes state, THE SYSTEM SHALL display the actual persisted state through presence and a conversation event.
- WHEN an assistant hands off work, THE SYSTEM SHALL reference a stored artifact revision.
- THE SYSTEM SHALL bound retries and prevent duplicate active attempts for the same stage.

## Requirement 3: Preserve evidence and uncertainty

As a user, I want claims to reference the source material so that I can check accuracy.

- THE SYSTEM SHALL retain references from repository facts to retrieved material.
- IF the Reviewer detects unsupported claims, THE SYSTEM SHALL surface those issues and wait for revision or user action.
- THE SYSTEM SHALL distinguish a document review from software execution or testing.
- THE SYSTEM SHALL treat fetched content as untrusted data and SHALL NOT execute instructions embedded in it.

## Requirement 4: Approve and export

As a user, I want to approve a specific result and download it so that the exported document matches my decision.

- WHEN the user approves an artifact, THE SYSTEM SHALL bind the approval to its exact ID and revision.
- WHEN a new revision is created, THE SYSTEM SHALL require a new approval for that revision.
- WHEN an approved document is downloaded, THE SYSTEM SHALL return its stored Markdown content exactly.
- IF a run has not been approved, THE SYSTEM SHALL NOT label it as an approved completed result.

## Requirement 5: Resume and recover

- WHEN the browser reloads or reconnects, THE SYSTEM SHALL restore the run, ordered events, stage state, and artifact revisions.
- IF a model stage fails, THE SYSTEM SHALL preserve completed earlier stages and allow a bounded retry.
- WHEN user attention is required, THE SYSTEM SHALL provide a readable visual nudge; audio SHALL remain optional.

## Requirement 6: Accessible, honest operation

- THE SYSTEM SHALL support keyboard operation and narrow screens, with readable labels and visible focus.
- THE SYSTEM SHALL keep model credentials on the server and out of logs, exports, and browser bundles.
- IF a demonstration uses simulated responses, THE SYSTEM SHALL label that mode explicitly.
- THE PROJECT SHALL document actual Kiro development and reused components accurately.
