# GhostMessenger

**Bring the buddy list back to life — as a small AI team you can actually manage.**

GhostMessenger is a planned web application for **Kiroween 2026 — Resurrection**. It reimagines the retired MSN / Windows Live Messenger experience for a modern purpose: coordinating a small team of AI assistants that research, write, and review a document together.

## Current status

**Planning and repository setup. There is no working application or hosted demo yet.** The initial design, acceptance criteria, implementation plan, and Kiro-ready specifications are committed here. Development with Kiro and the accompanying evidence still need to happen; the presence of `.kiro/` alone is not evidence that Kiro has been used.

## The first useful workflow

1. Provide the URL of a public GitHub repository and ask for a project introduction.
2. **Researcher** reads permitted repository material and collects facts with source links.
3. **Writer** uses those facts to draft an introduction and README suggestions.
4. **Reviewer** flags unsupported claims, omissions, and contradictions.
5. The user requests a revision or approves and downloads the Markdown result.

The goal is to reduce repeated copy-and-paste between AI conversations while keeping the user aware of what is happening and able to intervene.

## What comes back to life

| Classic messenger interaction | Modern purpose |
| --- | --- |
| Buddy list and online / busy presence | See which assistant is available, working, blocked, or waiting for approval |
| Conversation windows and message history | Inspect work, correct a brief, and follow handoffs |
| File transfer | Pass versioned source notes and drafts between assistants; download the finished document |
| Nudge | Notify the user when a decision is needed or a task fails |

The first version has three fixed roles and one bounded workflow. Model responses must come from an actual model integration. A demonstration mode, if implemented, must be clearly labeled and must not be presented as a real completed run.

## Project documents

- [Product design](docs/plans/2026-11-01-ghostmessenger-design.md)
- [Implementation plan](docs/plans/2026-11-01-ghostmessenger-implementation.md)
- [Hackathon project description](docs/hackathon/project-description.md)
- [Submission checklist](docs/hackathon/submission-checklist.md)
- [Kiro development evidence](docs/hackathon/kiro-usage.md)
- [Kiro requirements](.kiro/specs/ghostmessenger/requirements.md), [design](.kiro/specs/ghostmessenger/design.md), and [tasks](.kiro/specs/ghostmessenger/tasks.md)

## Development principles

- Persist runs, messages, approval decisions, and artifact revisions.
- Preserve source links; do not invent repository features or claim that a planned feature already works.
- Keep model credentials on the server and out of browser bundles and Git history.
- Treat fetched repository content as data, not instructions for tools or the application.
- Retry a failed stage without repeating completed stages or overwriting approved artifacts.
- Use original branding and artwork inspired by the messenger era. This project is not affiliated with Microsoft and does not connect to the retired Messenger service.

## Hackathon references

- [Official rules](https://kiroween.devpost.com/rules)
- [Resources and credits](https://kiroween.devpost.com/resources)

This is a new repository created on November 1, 2026. No pre-existing application code has been imported. See the checklist for the remaining requirements before a competition submission.

## License

[MIT](LICENSE).
