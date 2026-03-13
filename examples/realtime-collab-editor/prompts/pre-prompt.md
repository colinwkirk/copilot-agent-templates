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
- This project utilizes a **Tetra-phasic Agent Workflow** (`Architect` -> `Drafter` -> `Implementer` -> `Reviewer`).
- Features must be broken down and strictly scoped via file contracts (`PLAN.md`, `ACCEPTANCE_CRITERIA.md`, `IMPLEMENTATION_DRAFT.md`, `REVIEW.md`).
- Existing systems are off-limits and presumed working. Refactoring untouched code is prohibited unless an explicit "Refactor Phase" is defined and approved by the Architect.
- Do not optimize prematurely. Follow the acceptance criteria exactly.
