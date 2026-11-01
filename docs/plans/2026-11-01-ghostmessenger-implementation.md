# GhostMessenger Implementation Plan

> **For implementers:** Use the available `executing-plans` skill to implement and verify this plan task by task. The owner has approved the product direction and delegated routine implementation choices.

**Goal:** Ship a messenger-style application that turns public repository material into a sourced, reviewed, approved Markdown introduction.

**Architecture:** A browser client displays contacts, conversations, and artifacts. A server executes the three stages sequentially and persists state and versions. Model credentials stay on the server; approvals bind to immutable artifacts.

**Tech Stack:** Planned TypeScript client and API, React / Vite for the web interface, a server-side model adapter, and SQLite for local durable state. Verify current supported runtime and package versions from official documentation before scaffolding; lock the chosen versions.

---

## Current starting point

This repository contains documents and Kiro-ready specifications only. No package manifest, source implementation, test suite, or runnable server exists yet. Use Kiro for actual implementation and record factual evidence in `docs/hackathon/kiro-usage.md`.

## Task 1: Establish the application and explicit configuration

Files: create `package.json`, `package-lock.json`, `tsconfig.json`, `vite.config.ts`, `index.html`, `src/main.tsx`, `src/App.tsx`, `server/index.ts`, and `.env.example`.

1. Verify the installed Node version and current official Vite / React requirements.
2. Scaffold the client and a small server. Add scripts named `dev`, `build`, `typecheck`, `test`, and `test:e2e` with actual working commands.
3. Define `MODEL_PROVIDER`, `MODEL_API_KEY`, `MODEL_NAME`, and `DATABASE_PATH` in `.env.example` without real values. Startup must report missing configuration honestly.
4. Add a server health endpoint that reports its version and whether model configuration is present, without exposing credential values.
5. Run `npm run typecheck` and `npm run build`; expect exit code 0. Start `npm run dev`; expect the UI and health endpoint to load.
6. Commit as `chore: scaffold GhostMessenger application`.

## Task 2: Persist runs and constrain transitions

Files: create `server/db.ts`, `server/schema.sql`, `server/workflow/state.ts`, `server/workflow/state.test.ts`, and `server/routes/runs.ts`.

1. Write tests for approval bound to an exact artifact revision, only one running attempt per stage, and retry preserving earlier artifacts.
2. Run `npm test -- server/workflow/state.test.ts`; expect these tests to fail before the state implementation exists.
3. Implement the run, stage attempt, event, artifact, and approval records described in the design. Use transactions for transitions.
4. Define states explicitly:

   ```ts
   type RunStatus = 'queued' | 'running' | 'waiting_for_approval' | 'failed' | 'completed';
   type Role = 'researcher' | 'writer' | 'reviewer';
   ```

5. Add create / read run endpoints and ordered event retrieval. A read after restarting the server must restore the same state and artifact content.
6. Re-run the targeted tests and type checking; expect exit code 0. Commit the persistence change.

## Task 3: Fetch bounded source material

Files: create `server/sources/github.ts`, `server/sources/github.test.ts`, and `server/workflow/evidence.ts`.

1. Write tests rejecting unsupported hosts, private / loopback destinations, unbounded downloads, and failed fetches that would otherwise be represented as successful evidence.
2. Run the targeted test file and verify failure before implementation.
3. Initially support public GitHub repository metadata and README content. Preserve repository revision, file path, URL, and fetched text for traceability.
4. Apply time, size, and redirect limits. Keep source content separate from application / system instructions.
5. Confirm tests pass and manually fetch a permitted small public repository. Expect a bounded evidence object with actual source URLs or an explicit fetch failure.
6. Commit the source importer.

## Task 4: Run three real stages with recoverable failures

Files: create `server/models/adapter.ts`, `server/workflow/runner.ts`, `server/workflow/runner.test.ts`, and `server/prompts/{researcher,writer,reviewer}.ts`.

1. Write a controlled integration test where Writer fails, then retries; Researcher must run once, completed artifacts must remain, and retries must be bounded.
2. Write a test where Reviewer finds an unsupported claim; the run must wait for user action rather than silently approve itself.
3. Implement an actual provider adapter from its current official API documentation. Use a provider-neutral interface; do not assume Kiro credits can pay for this application's runtime model calls.
4. Execute the fixed Researcher → Writer → Reviewer sequence. Persist stage input references, outputs, and errors before emitting the related events.
5. Re-run tests with controlled provider doubles. Separately run one real model workflow when valid runtime credentials are available; report a real-run check as pending until it succeeds.
6. Commit the workflow runner.

## Task 5: Make the messenger interface useful

Files: create `src/components/{BuddyList,Conversation,TaskComposer,ArtifactPanel,ApprovalPanel}.tsx`, `src/styles.css`, and `src/api.ts`.

1. Use original messenger-era visuals, readable typography, and a functional presence indicator for each fixed role.
2. Render ordered real server events, source references, artifact handoffs, and actionable failures.
3. Add brief submission, stage retry, revision feedback, approval, and Markdown download actions.
4. Ensure the approved download is the exact stored artifact, using `GET /api/runs/:id/artifacts/:artifactId/download` or an equivalently explicit route.
5. Verify desktop and narrow-screen layouts, large text, keyboard operation, visible focus, and status labels that do not depend on color alone. Do not expose provider configuration in the product task flow.
6. Run `npm run typecheck` and `npm run build`; expect exit code 0. Commit the interface.

## Task 6: Verify and prepare competition evidence

Files: create `tests/e2e/document-workflow.spec.ts`; update `README.md`, `docs/hackathon/kiro-usage.md`, and `docs/hackathon/submission-checklist.md`.

1. Add one meaningful end-to-end scenario: repository input → real state transitions using a controlled provider → review issue → revision → approval → export → reload / restored run.
2. Assert that the downloaded text equals the approved artifact content and that the approved revision is unchanged after reload.
3. Run `npm run test`, `npm run test:e2e`, `npm run typecheck`, and `npm run build`; expect all commands to exit 0. Record actual outputs and any limits.
4. Capture and inspect the running desktop and narrow-screen UI. Run a separate real-provider demonstration; retain source and artifact evidence without credentials.
5. Document actual Kiro sessions and the files / features they produced. Do not infer Kiro usage from this plan or from `.kiro/`.
6. Prepare the public demo URL, a public YouTube video under three minutes, English project description, and Kiro writeup. Mark each checklist item only when complete.
7. Commit and push the verified implementation; keep unverified items explicitly pending.

## Completion criteria

The app has been run, visually inspected, and tested with a real model; a user can approve and download a persistent result; Kiro usage is evidenced; all required competition materials are available. Repository setup alone does not meet these criteria.
