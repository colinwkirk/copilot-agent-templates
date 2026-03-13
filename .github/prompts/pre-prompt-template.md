# Project Context & AI Guidelines (Pre-Prompt Template)

> **Usage**: Attach this text as context or save it to `.github/copilot-instructions.md` so the Architect and Drafter agents naturally inherit the base rules of the project before writing their plans.

## Project Identity
- **Project Name:** [Project Name, e.g., KeyForge Piano]
- **Core Purpose:** [1-2 sentences on what the system does. e.g., A low-latency musical tool for gamified theory practice.]
- **Tech Stack:** [e.g., Flutter, Dart, Riverpod, sqflite, flutter_soloud]

## Architectural Guardrails
- **State Management:** [e.g., Use Riverpod. Prefer stateless widgets with `ConsumerWidget`.]
- **Performance / Memory:** [e.g., Audio engine ops must spawn in an Isolate. Avoid heavy UI blocking tasks.]
- **Persistence:** [e.g., Never use local SharedPreferences for user-sensitive config; use the SQLite repository.]
- **Styling constraints:** [e.g., Honor the global Theme schema. Never hardcode colors.]

## AI Agent Workflow Rules
- This project utilizes a **Tetra-phasic Agent Workflow** (`Architect` -> `Drafter` -> `Implementer` -> `Reviewer`).
- Features must be broken down and strictly scoped via file contracts (`PLAN.md`, `ACCEPTANCE_CRITERIA.md`, `IMPLEMENTATION_DRAFT.md`, `REVIEW.md`).
- Existing systems are off-limits and presumed working. Refactoring untouched code is prohibited unless an explicit "Refactor Phase" is defined and approved by the Architect.
- Do not optimize prematurely. Follow the acceptance criteria exactly.