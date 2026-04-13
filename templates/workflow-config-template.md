# Workflow Config: ${Task Title}

Use this file when the workflow generator selects `config-global-agents` packaging.
It is the repository-local contract consumed by the global generic tetraphasic agents.

## Packaging Mode
- Packaging Mode: config-global-agents
- Workflow Shape: full-tetraphasic

## Agent Entry Points
- Coordinator Agent: @tetraphasic-coordinator
- Architect Agent: @tetraphasic-architect
- Drafter Agent: @tetraphasic-drafter
- Implementer Agent: @tetraphasic-implementer
- Reviewer Agent: @tetraphasic-reviewer
- QA Agent: @tetraphasic-qa

## Project Identity
- Project Name: ${Project Name}
- Task Title: ${Task Title}
- Task Identifier: ${task}
- Core Objective: ${Core Objective}
- Target Repository: ${Target Repository}

## Artifact Paths
- Workflow Config File: DOCS/WORKFLOW_CONFIG.md
- Plan File: DOCS/${PLAN_FILE}.md
- Current Phase Tracker File: DOCS/${CURRENT_PHASE_FILE}.md
- Acceptance Criteria Pattern: DOCS/PHASE_N_ACCEPTANCE_CRITERIA.md
- Implementation Draft Pattern: DOCS/PHASE_N_IMPLEMENTATION_DRAFT.md
- Implementation Report Pattern: DOCS/PHASE_N_IMPLEMENTATION_REPORT.md
- Review Pattern: DOCS/PHASE_N_REVIEW.md
- Refinement Pattern: DOCS/PHASE_N_REFINEMENT.md
- QA Plan Pattern: DOCS/PHASE_N_QA_PLAN.md
- QA Report Pattern: DOCS/PHASE_N_QA_REPORT.md

## Bootstrap
- Initial Phase: PHASE_1
- Preferred Initial Validation Mode: ${INITIAL_VALIDATION_MODE}
- Desired Phase 1 Focus: ${PHASE_1_FOCUS}
- Advanced QA Default: NONE

## Repository Survey Priorities
- ${Survey Target 1}
- ${Survey Target 2}
- ${Survey Target 3}

## Domain Guardrails
- ${Guardrail 1}
- ${Guardrail 2}
- ${Guardrail 3}

## Plan Requirements
- ${Plan Requirement 1}
- ${Plan Requirement 2}
- ${Plan Requirement 3}

## Packaging Assumptions
- Multiple simultaneous workflows in this workspace: no
- Stable global generic tetraphasic agents are available: yes
- Repository-local dedicated agents required: no
- Rationale For Config Mode: ${Packaging Rationale}

## Resolution Rules
- Read this file first before reading or writing other workflow artifacts.
- Resolve current-phase file paths by replacing `PHASE_N` in each configured pattern with the `Active Phase` value from the current-phase tracker.
- Treat the artifact paths in this file as the source of truth.
- If packaging assumptions change materially, stop and recommend regeneration using `project-specific-generated-agents`.