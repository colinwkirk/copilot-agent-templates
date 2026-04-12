# Project Context & AI Guidelines

## Project Identity
- **Project Name:** SyncDoc
- **Core Purpose:** A real-time collaborative document editor allowing multiple users to simultaneously edit rich-text documents with conflict-free merging, presence awareness, and version history.
- **Tech Stack:** TypeScript, Node.js, Express, WebSocket (ws), Yjs (CRDT library), React, TipTap editor, PostgreSQL, Redis

## Architectural Guardrails
- **State Management:** Use Yjs documents as the single source of truth for document state. React UI state is derived from the Yjs doc via TipTap bindings.
- **Real-Time Sync:** All document mutations flow through Yjs update events and are broadcast via WebSocket. Never bypass the CRDT layer for direct DOM manipulation.
- **Persistence:** Yjs document state is serialized and persisted to PostgreSQL. Redis is used only for ephemeral pub/sub (presence, awareness). Do not store document content in Redis.
- **Auth:** JWT-based authentication. All WebSocket connections must be authenticated before receiving document updates. Never trust client-supplied user IDs.
- **Performance:** Yjs updates must be batched and debounced (200ms) before persistence writes. WebSocket broadcasts are immediate.

## AI Agent Workflow Rules
- This project utilizes a **Coordinator-Driven Tetraphasic Workflow** (`Coordinator` -> `Architect` -> `Drafter/Implementer` -> `Reviewer` -> `Coordinator`).
- The coordinator (`@collab-editor-coordinator`) is the workflow entrypoint. It orchestrates subagent handoffs and maintains `DOCS/SYNCDOC_CURRENT_PHASE.md` as the phase tracker.
- Features must be broken down and strictly scoped via file contracts (`PLAN.md`, `CURRENT_PHASE.md`, `ACCEPTANCE_CRITERIA.md`, `IMPLEMENTATION_DRAFT.md`, `REVIEW.md`).
- The architect must declare a per-phase validation mode: `TEST_FIRST`, `TEST_WITH_IMPLEMENTATION`, or `MANUAL_VALIDATION_ONLY`.
- Advanced QA is opt-in only. It is required only for regression, integration, end-to-end, or multi-service validation that exceeds the normal reviewer scope.
- Existing systems are off-limits and presumed working. Refactoring untouched code is prohibited unless an explicit "Refactor Phase" is defined and approved by the Architect.
- Do not optimize prematurely. Follow the acceptance criteria exactly.
