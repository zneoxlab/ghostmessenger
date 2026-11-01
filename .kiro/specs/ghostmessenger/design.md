# GhostMessenger Technical Design

See the [approved product design](../../../docs/plans/2026-11-01-ghostmessenger-design.md) and [implementation plan](../../../docs/plans/2026-11-01-ghostmessenger-implementation.md).

## Planned components

Browser UI → HTTP API → persistent workflow runner → server-side model adapter.

The runner also uses a bounded public GitHub source importer and a relational store for runs, stage attempts, ordered events, artifacts, and approvals. The client observes server state rather than simulating progress locally.

## Data and state invariants

- One active attempt per run / stage.
- Every handoff points to immutable source artifact IDs and revisions.
- An approved artifact is immutable.
- A changed brief invalidates affected downstream stages and requires approval of the new final revision.
- A completion event is emitted only after the corresponding result has been persisted.
- A downloaded approved artifact exactly matches its stored approved content.
- Model errors preserve earlier stage results and do not become success events.

## Implementation order

Application shell and configuration → persistent transitions → source importer → real three-stage workflow → messenger interface → end-to-end / real-provider / visual verification.

Package versions, provider choice, and deployment target remain implementation decisions. Do not add dependency credentials or pretend these choices have already been implemented.
