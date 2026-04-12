# Global Copilot Instructions

These rules apply to all Copilot agents operating in this repository.

Agents must follow the structured workflow and guardrails defined below.

This repository may also be opened alongside one or more application repositories as a shared template source.
When that happens, agents should treat this repository as a reference library and generate requested workflow artifacts in the relevant target repository unless the user explicitly asks to modify templates here.

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

GPT models may produce or update Markdown workflow artifacts such as `.agent.md`, `.prompt.md`, `.instructions.md`, planning docs, and `copilot-instructions.md`, but they must not produce `.py`, `.yaml`, or other source code files.

## Coding Models

The following models may modify code:

- Claude Opus
- Claude Sonnet
- Gemini Pro

These models may implement or review code when acting in the appropriate role.

---

# Agent Workflow

All work must follow the structured development loop:

Coordinator → Architect → Drafter/Implementer → Reviewer → Coordinator

An optional QA stage may be inserted only when the approved workflow explicitly requires advanced validation beyond the normal reviewer contract.

The coordinator owns orchestration.
The architect still owns planning, phase boundaries, acceptance criteria, and post-review decisions.

Agents must not bypass or reorder this sequence except for the architect-approved Drafter bypass and the narrowly scoped reviewer-to-implementer refinement shortcut.

Workflow-generation agents may exist outside this loop only to create or refine the workflow artifacts themselves.
They must preserve this sequence in every workflow they generate.

---

# Tetraphasic Workflow Generator Responsibilities

A GPT-5.4 workflow-generation agent may:

- read the template files in this repository
- generate or refine coordinator-driven tetraphasic workflow agent files
- generate kickoff prompts and shared pre-prompts
- update `copilot-instructions.md` when repository-wide workflow guidance is needed

The workflow generator must:

- identify the intended destination repository before writing generated assets
- use this repository as a template source when it is present only for reference
- write workflow artifacts into the target repository unless the user explicitly asks to scaffold or revise the templates in this repository
- preserve the coordinator-driven Architect → Drafter/Implementer → Reviewer cycle
- keep GPT-authored outputs limited to Markdown workflow artifacts
- clearly define task identifiers, task titles, plan filenames, and phase filenames
- tailor repository-survey guidance to the target project or feature
- prefer a small, structurally useful Phase 1
- keep model assignments aligned with role responsibilities

---

# Architect Responsibilities

The architect owns planning and the substantive phase-control decisions within the coordinator-driven workflow.

The architect:

- analyzes the current system
- defines architecture
- creates phase plans
- writes acceptance criteria
- defines the phase validation mode and minimum approval evidence
- decides whether advanced QA is actually required for the phase
- decides whether to approve, refine, or re-plan after review

The architect **must not modify production code**.

Architect outputs:

```
DOCS/${PLAN_FILE}.md  
DOCS/${PHASE_FILE}_ACCEPTANCE_CRITERIA.md
```

Acceptance criteria should expose validation and advanced-QA choices as clearly named sections, not buried bullets.

Coordinator-tracked progress artifact:

```
DOCS/${CURRENT_PHASE_FILE}.md
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
- translates the validation mode into concrete test or manual-validation steps

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
- follows the declared validation mode and writes tests where required

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
- verifies tests and regressions or manual evidence, depending on the validation mode
- identifies missing work or defects
- recommends approval, refinement, or re-plan

The reviewer **does not implement code and does not re-architect the system**.

Reviewer output:

```
DOCS/${PHASE_FILE}_REVIEW.md
```

Optional advanced QA output:

```
DOCS/${PHASE_FILE}_QA_REPORT.md
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

Validation mode must be explicit for every approved phase:

- `TEST_FIRST` is preferred for logic-bearing phases
- `TEST_WITH_IMPLEMENTATION` is appropriate when tests belong in the slice but strict failing-test-first order is impractical
- `MANUAL_VALIDATION_ONLY` is allowed only with explicit justification

Do not add a separate QA phase or QA agent unless a future workflow explicitly requires a distinct test-planning or evidence-audit role.

If advanced QA is required:

- scope it narrowly to regression, integration, end-to-end, multi-service, or live-environment validation
- keep it out of the default loop
- require it to verify workspace connectivity and authentication prerequisites before execution
- block clearly and surface user guidance needs when the required environment access is missing

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

Large coordinator-driven workflows should maintain a current-phase tracker in `DOCS/` so unattended subagent progress is visible and recoverable.
That tracker should also show active subagent, last returned signal, blocker summary, outstanding user guidance, and a concise handoff history.

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