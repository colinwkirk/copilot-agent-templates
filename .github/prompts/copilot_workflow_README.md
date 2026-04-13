# Copilot Workflow: Coordinator-Driven Tetraphasic + Prompt Layer

## Overview
This setup combines:
- A coordinator plus structured phase agents (Architect → Drafter → Implementer → Reviewer)
- Global prompt files for repeatable thinking patterns
- Lightweight workflow selection

Goal:
- reduce agent drift
- prevent over-engineering
- enforce consistent execution
- stay fast for small tasks

---

## Core Concepts

### Global Instructions
Location:
~/.copilot/copilot-instructions.md

Purpose:
- persistent guardrails
- workflow discipline

---

### Prompt Files
Locations:
~/.copilot/prompts
.github/prompts

Purpose:
- reusable thinking scaffolds
- used on demand

---

### Agents
Location:
.github/agents

Purpose:
- execution roles
- phase separation
- coordinator-led orchestration

---

## Workflow Philosophy

Default:
Coordinator → Architect → Drafter/Implementer → Reviewer → Coordinator

The coordinator is the entrypoint.
The architect still owns planning, phase boundaries, acceptance criteria, and post-review decisions.
The drafter remains optional when the architect explicitly approves a simple-phase bypass.
Per-phase validation mode should be explicit: `TEST_FIRST`, `TEST_WITH_IMPLEMENTATION`, or `MANUAL_VALIDATION_ONLY`.
An optional QA stage should appear only for advanced regression, integration, end-to-end, multi-service, or live-environment validation.
Generated acceptance criteria should call these choices out in clearly labeled `Validation Posture` and `QA Decision` sections.

Use lighter workflows when:
- small scope
- low risk
- low ambiguity

---

## Global Prompt Files

architecture-kickoff.prompt.md
- use for new/ambiguous work
- enforces repo inspection + scope + risks

small-task-triage.prompt.md
- decides workflow size

implementation-handoff.prompt.md
- tight execution scope
- validation + rollback

review-pass.prompt.md
- correctness + regression checks

workflow-reuse-check.prompt.md
- prevents duplicate workflows

---

## Usage Patterns

### Large Work
1. architecture-kickoff
2. Coordinator
3. Architect
4. Drafter when needed
5. implementation-handoff
6. Implementer
7. review-pass
8. Reviewer

---

### Small Work
1. small-task-triage

Then:
- direct OR
- implementation-handoff → implement → review-pass

---

### Workflow Creation
workflow-reuse-check first

Before generating project workflow files, the workflow generator should make a packaging decision:
- `config-global-agents`: rely on a stable global generic coordinator/phase-agent set plus repository-local config and kickoff artifacts
- `project-specific-generated-agents`: generate dedicated repository-local agent files for the workflow

Prefer `config-global-agents` when minimizing agent sprawl is the main goal and only one workflow is expected to be active in the workspace at a time.
Prefer `project-specific-generated-agents` when the repository must be self-contained, shareable, or able to host multiple simultaneous workflows without ambiguous agent targeting.

If the correct packaging mode is unclear, the generator should ask a short intake question set before scaffolding.

For `config-global-agents`, the standard repository-local outputs are:
- `DOCS/WORKFLOW_CONFIG.md`
- `DOCS/<current-phase-tracker>.md`
- `.github/prompts/<task>-workflow-kickoff.prompt.md`

That kickoff prompt should invoke `@tetraphasic-coordinator`, and the global generic phase agents should read `DOCS/WORKFLOW_CONFIG.md` first.

When a new full workflow is generated, it should produce:
- one coordinator agent
- four phase agents
- optionally one QA agent when advanced validation is required
- one current-phase tracker file
- kickoff/context prompts as needed

---

## Best Practices

- Chain prompts intentionally
- Keep prompts small
- Prefer reuse
- Escalate early if scope grows
- Separate concerns
- Update the current-phase tracker before and after every coordinator handoff
- Prefer stronger validation discipline inside the existing four phase roles before adding a separate QA agent
- Use QA only when advanced validation exceeds the normal reviewer scope, and require QA to verify workspace connectivity/auth prerequisites before environment-dependent checks

---

## Example

Task: Fix onboarding skip bug

Flow:
1. small-task-triage → reduced workflow
2. implementation-handoff
3. implement
4. review-pass

---

## Setup

Global prompts:
~/.copilot/prompts

settings.json:
{
  "chat.promptFilesLocations": [
    "/Users/yourname/.copilot/prompts"
  ]
}

Repo:
.github/prompts
.github/agents

For coordinator-driven subagent orchestration, ensure the environment supports:
- agent mode or custom agents
- subagent invocation via the `agent` tool
- frontmatter allowlists using `agents` where a coordinator invokes phase agents

---

## Outcome

- consistent workflows
- low drift
- fast small tasks
- disciplined large tasks
