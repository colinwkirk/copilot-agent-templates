# Example: Real-Time Collaborative Document Editor (From Scratch)

This example demonstrates how to set up the tetraphasic workflow for building a **new application from scratch** — a real-time collaborative document editor with CRDT-based conflict resolution.

## Why This Warrants the Workflow

A collaborative editor involves:
- CRDT data structures for conflict-free merging
- WebSocket infrastructure for real-time sync
- Presence awareness (cursors, selections)
- Document persistence and versioning
- Auth and access control

These systems are deeply interdependent. Building them in a single unstructured AI session invites drift, scope creep, and subtle correctness bugs. The tetraphasic workflow keeps each piece scoped, reviewed, and safe.

## What's Included

| File | Purpose |
|------|---------|
| `pre-prompt.md` | Project context and AI guardrails |
| `prompt.md` | Initial prompt to kick off the Architect |
| `agents/collab-editor-architect.agent.md` | Architect agent definition |
| `agents/collab-editor-drafter.agent.md` | Drafter agent definition |
| `agents/collab-editor-implementer.agent.md` | Implementer agent definition |
| `agents/collab-editor-reviewer.agent.md` | Reviewer agent definition |

## How to Use

1. Copy the `agents/` files into your project's `.github/agents/` directory.
2. Add `pre-prompt.md` content to your `.github/copilot-instructions.md` (or attach it as context).
3. Open Copilot Chat and paste the contents of `prompt.md` to start the planning phase.
