# Current Phase Tracker: ${Task Title}

Use this file as the coordinator-owned source of truth for unattended workflow progress.
Update it before and after every subagent invocation.

## Workflow Status
- Task: ${Task Title}
- Task Identifier: ${task}
- Overall Status: NOT_STARTED
- Active Phase: PHASE_1
- Validation Mode: UNSET
- Validation Status: NOT_STARTED
- Advanced QA Requirement: NONE
- Advanced QA Status: NOT_NEEDED
- Current Owner: Coordinator
- Active Subagent: none
- Current Action: Waiting for first architect invocation
- Last Completed Step: none
- Last Returned Signal: none
- Next Intended Route: Coordinator -> Architect
- Refinement Loop Count: 0

## Workflow Visibility
- Blocker Summary: none
- Outstanding User Guidance: none
- Current Route Reason: Initial planning start

## Active Artifacts
- Plan: DOCS/${PLAN_FILE}.md
- Acceptance Criteria: DOCS/${PHASE_FILE}_ACCEPTANCE_CRITERIA.md
- Implementation Draft: DOCS/${PHASE_FILE}_IMPLEMENTATION_DRAFT.md
- Implementation Report: DOCS/${PHASE_FILE}_IMPLEMENTATION_REPORT.md
- Review: DOCS/${PHASE_FILE}_REVIEW.md
- Refinement: DOCS/${PHASE_FILE}_REFINEMENT.md
- QA Plan: DOCS/${PHASE_FILE}_QA_PLAN.md
- QA Report: DOCS/${PHASE_FILE}_QA_REPORT.md

## Latest Coordinator Notes
- Waiting for workflow start.

## Handoff History
- none yet

## Update Rules
- Set `Overall Status` to one of: `NOT_STARTED`, `IN_PROGRESS`, `WAITING_ON_SUBAGENT`, `BLOCKED`, `COMPLETE`
- Set `Validation Mode` to one of: `UNSET`, `TEST_FIRST`, `TEST_WITH_IMPLEMENTATION`, `MANUAL_VALIDATION_ONLY`
- Set `Validation Status` to one of: `NOT_STARTED`, `TESTS_PENDING`, `TESTS_IN_PROGRESS`, `TESTS_RECORDED`, `MANUAL_EVIDENCE_RECORDED`, `VALIDATED`, `BLOCKED`
- Set `Advanced QA Requirement` to one of: `NONE`, `REQUIRED`
- Set `Advanced QA Status` to one of: `NOT_NEEDED`, `PENDING`, `IN_PROGRESS`, `EVIDENCE_RECORDED`, `VALIDATED`, `BLOCKED`
- Before invoking any subagent, update:
  - `Overall Status`
  - `Validation Mode` when it becomes known
  - `Validation Status` when test or validation work changes state
  - `Advanced QA Requirement` when it becomes known
  - `Advanced QA Status` when advanced QA work changes state
  - `Current Owner`
  - `Active Subagent`
  - `Current Action`
  - `Current Route Reason`
  - `Next Intended Route`
  - `Outstanding User Guidance` when clarification or access is needed
  - `Latest Coordinator Notes`
  - append a concise line to `Handoff History`
- After a subagent returns, update:
  - `Validation Mode` when artifacts define or change it
  - `Validation Status` based on the returned evidence
  - `Advanced QA Requirement` when artifacts define or change it
  - `Advanced QA Status` based on the returned evidence
  - `Last Completed Step`
  - `Last Returned Signal`
  - `Current Owner`
  - `Active Subagent`
  - `Current Action`
  - `Blocker Summary` when needed
  - `Next Intended Route`
  - `Latest Coordinator Notes`
  - append a concise line to `Handoff History`
- If refinement repeats, increment `Refinement Loop Count`
- If the workflow is blocked, record the blocking reason clearly in `Latest Coordinator Notes`
- If the workflow completes, set `Overall Status: COMPLETE` and make the final route `none`