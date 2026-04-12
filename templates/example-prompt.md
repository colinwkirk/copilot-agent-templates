@learning-engine-coordinator

Start the Learning Engine project.

The learning engine coordinator and tetraphasic workflow have been added to the repository, but the planning artifacts do not exist yet. Start by invoking the architect in **Planning Mode**.

Your task:

1. Analyze the current KeyForge codebase, focusing on:

   * practice modules (intervals, chords, scales, arpeggios, etc.)
   * skill-tree or progression structures
   * scoring / correctness logic
   * streak tracking
   * any existing practice result models
   * persistence of practice data
   * recommendation or drill-selection logic (if any)

2. Based on the current architecture, design the **Learning Engine intelligence layer** described in your agent instructions.

3. Produce the following files:

DOCS/LEARNING_ENGINE_PLAN.md
DOCS/PHASE_1_ACCEPTANCE_CRITERIA.md

In the acceptance criteria, include a clearly labeled `Validation Posture` section and a clearly labeled `QA Decision` section.

The plan should:

* define the learning engine architecture
* define skill atoms / per-item practice stats
* define near-miss detection (e.g., scale-degree Levenshtein logic)
* define recommendation inputs
* break the work into phases
* declare the Phase 1 validation mode and justify it
* declare whether advanced QA is required for Phase 1 and justify it only if needed
* ensure Phase 1 is small, safe, and implementable immediately

In addition to the phase plan and acceptance criteria, generate a PHASE_1_WORKING_CONTRACT.md file that explicitly defines allowed refactors, forbidden scope expansion, compatibility expectations, and what the implementer should do if code reality differs from the plan.

Also initialize:

DOCS/LEARNING_ENGINE_CURRENT_PHASE.md

After writing those documents, continue through the coordinator-driven next phase selected by the architect recommendation.
