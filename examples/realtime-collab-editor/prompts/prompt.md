@collab-editor-architect

Start the SyncDoc collaborative editor project.

The agents and tetra-phasic workflow have been configured, but the planning artifacts do not exist yet. This means you should begin in **Planning Mode**.

Your task:

1. Analyze the current codebase (currently empty scaffolding), focusing specifically on:
   * The Yjs CRDT document layer and how updates will flow
   * The WebSocket server architecture for real-time sync
   * The persistence layer for document storage and versioning

2. Based on the requirements, design the **core document sync infrastructure** required for this project to succeed.

3. Produce the following files:

- DOCS/SYNCDOC_PLAN.md
- DOCS/PHASE_1_ACCEPTANCE_CRITERIA.md

The plan should:
* define the CRDT document architecture and Yjs integration
* define the WebSocket connection lifecycle and message protocol
* define the persistence schema for documents and versions
* define the authentication flow for WebSocket connections
* break the work into distinct, sequential phases
* ensure Phase 1 is small, structurally foundational, and implementable immediately (likely: basic Yjs doc + WebSocket server + single-doc sync between two clients)

After writing those documents, hand off to:

@collab-editor-drafter
