# Tetraphasic Workflow Generator (Global Invocation)

You are preparing input for the `global-tetraphasic-workflow-generator` agent.

Your job is to:
- interpret the provided context
- normalize it into a structured request
- enforce reuse-first and scope discipline
- then invoke the agent with a clean, well-defined task

---

## Instructions

Use the provided context as raw input. It may include:
- debugging notes
- partial ideas
- trial-and-error exploration
- incomplete or conflicting thoughts

Do NOT treat it as a finalized specification.

---

## Requirements

- Prefer reuse or adaptation of existing agents, prompts, or workflows over creating new ones
- Only generate new workflow assets if no existing asset is a reasonable fit
- Keep Phase 1 tightly scoped and structurally useful
- Do not over-expand scope beyond what is clearly required
- If the task is small or low-risk, allow a reduced workflow instead of forcing tetraphasic
- Decide whether the workflow should use `config-global-agents` or `project-specific-generated-agents`
- Ask a short intake question set only when that packaging choice is ambiguous and materially affects the outcome

---

## Tasks

1. Extract:
   - the core problem
   - the actual objective
   - relevant scope and boundaries

2. Determine:
   - whether this requires:
     - reuse
     - a reduced workflow
   - a full coordinator-driven tetraphasic workflow
    - whether the packaging mode should be:
       - `config-global-agents`
       - `project-specific-generated-agents`

    Packaging concerns to resolve when needed:
    - Will multiple workflows run simultaneously in one workspace?
    - Do stable generic global tetraphasic phase agents already exist?
    - Must the result be self-contained and shareable in the repository?
    - Are the domain guardrails specialized enough to justify dedicated agents?

3. Prepare a clean, structured request for the workflow generator

---

## Invocation

Invoke:

@global-tetraphasic-workflow-generator

With:

- a clear task definition
- normalized scope
- reuse-first requirement
- explicit output expectations

---

## Output Expectations

The final output must include:

- recommended workflow shape (reuse / reduced / tetraphasic)
- recommended packaging mode (`config-global-agents` / `project-specific-generated-agents`)
- coordinator-driven workflow artifacts when a full tetraphasic flow is applicable
- generated or updated workflow artifacts (if applicable)
- rationale for reuse vs new generation
- rationale for packaging-mode choice
- clearly scoped Phase 1 (if applicable)

When `config-global-agents` is selected, expect the generator to standardize on:
- `DOCS/WORKFLOW_CONFIG.md`
- `DOCS/[CURRENT_PHASE_FILE].md`
- `.github/prompts/[task]-workflow-kickoff.prompt.md`

---

## Context

<user-provided input goes here>