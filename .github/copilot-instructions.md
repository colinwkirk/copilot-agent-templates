# Global Copilot Instructions

These rules apply to all Copilot agents operating in this repository.

Agents must follow the structured workflow and guardrails defined below.

---

# Model Responsibilities

Different models are used for different types of work.

## GPT-family models

GPT models are used only for:

- prompt design
- documentation
- architectural discussion
- analysis or guidance

GPT models must **never modify production code**.

If code changes are required, the agent must defer to the appropriate coding agent.

## Coding Models

The following models may modify code:

- Claude Opus
- Claude Sonnet
- Gemini Pro

These models may implement or review code when acting in the appropriate role.

---

# Agent Workflow

All work must follow the structured development loop:

Architect → Drafter → Implementer → Reviewer

Agents must not bypass or reorder this sequence.

---

# Architect Responsibilities

The architect owns planning and phase control.

The architect:

- analyzes the current system
- defines architecture
- creates phase plans
- writes acceptance criteria
- decides whether to approve, refine, or re-plan after review

The architect **must not modify production code**.

Architect outputs:

```
DOCS/${PLAN_FILE}.md  
DOCS/${PHASE_FILE}_ACCEPTANCE_CRITERIA.md
```

---

# Drafter Responsibilities

The drafter translates an approved phase into a concrete execution brief.

The drafter:

- inspects the repository
- identifies relevant files and modules
- proposes an implementation sequence
- identifies risks and edge cases
- narrows scope for the implementer

The drafter **does not change architecture or acceptance criteria**.

The drafter **does not write production code**.

Drafter output:

```
DOCS/${PHASE_FILE}_IMPLEMENTATION_DRAFT.md
```

---

# Implementer Responsibilities

The implementer executes **exactly one approved phase or refinement pass**.

The implementer:

- reads the plan, acceptance criteria, and implementation draft
- implements only the approved scope
- keeps changes tightly scoped
- preserves compatibility unless explicitly allowed otherwise
- writes tests where required

The implementer **must not begin later phases**.

Implementer output:

```
DOCS/${PHASE_FILE}_IMPLEMENTATION_REPORT.md
```

---

# Reviewer Responsibilities

The reviewer validates the implementation against the approved phase.

The reviewer:

- checks the code against the acceptance criteria
- verifies correctness
- verifies tests and regressions
- identifies missing work or defects
- recommends approval, refinement, or re-plan

The reviewer **does not implement code and does not re-architect the system**.

Reviewer output:

```
DOCS/${PHASE_FILE}_REVIEW.md
```

---

# Phase Discipline

Agents must respect strict phase boundaries.

Rules:

- only one phase or refinement pass may be active
- do not begin future phases
- do not expand scope beyond the acceptance criteria
- prefer refinement over architecture churn
- if two refinement loops fail, recommend re-planning

---

# Temporary Files

Agents must never write to `/tmp`.

Instead use:

```
${workspaceFolder}/tmp
```

This directory exists for:

- intermediate artifacts
- analysis output
- generated scratch files
- temporary reports

Do not create additional temporary directories elsewhere.

---

# Execution Safety

Agents must avoid destructive commands.

Disallowed operations include:

- rm -rf
- git reset --hard
- force pushes
- system-level package installs

Agents must not modify system configuration outside the repository.

---

# Repository Structure

Planning artifacts live in:

```
DOCS/
```

Temporary artifacts live in:

```
tmp/
```

Agents should not create new top-level directories without architectural justification.

---

# Scope Guardrails

Agents must:

- implement only the current phase
- avoid speculative future work
- prefer extension over rewrite
- maintain backward compatibility unless explicitly allowed
- document deviations clearly in implementation reports

---

# System Understanding

Before proposing architectural changes, agents must inspect the repository to understand the existing design.

Generic or template-based architecture suggestions are not acceptable.

Plans must reflect the real structure of the codebase.