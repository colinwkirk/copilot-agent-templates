@[Task Name]-coordinator

Start the [Project/Feature Name] project.

The coordinator and tetra-phasic workflow have been configured, but the planning artifacts do not exist yet. Start by invoking the architect in **Planning Mode**.

Your task:

1. Analyze the current codebase, focusing specifically on:
   * [Module or architectural area of interest 1]
   * [Module or architectural area of interest 2]
   * [Existing logic or components to integrate with]

2. Based on the current codebase reality, design the **[Feature Concept]** required for this project to succeed.

3. Produce the following files:

- DOCS/[TASK_IDENTIFIER]_PLAN.md
- DOCS/[TASK_IDENTIFIER]_CURRENT_PHASE.md
- DOCS/PHASE_1_ACCEPTANCE_CRITERIA.md

In the acceptance criteria, include a clearly labeled `Validation Posture` section and a clearly labeled `QA Decision` section.

The plan should:
* define the [Feature Concept] architecture
* define data structures and persistence needs
* [Specific core requirement 1, e.g., "define error boundaries"]
* [Specific core requirement 2, e.g., "define migration paths"]
* break the work into distinct, sequential phases
* declare the Phase 1 validation mode (`TEST_FIRST`, `TEST_WITH_IMPLEMENTATION`, or `MANUAL_VALIDATION_ONLY`) and justify it
* declare whether advanced QA is required for Phase 1 and justify it only if the phase truly needs integration, regression, end-to-end, multi-service, or live-environment validation
* ensure Phase 1 is small, structurally foundational, and implementable immediately

After writing those documents, hand off to:

the coordinator-driven next phase selected by the architect recommendation