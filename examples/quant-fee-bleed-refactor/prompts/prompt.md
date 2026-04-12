@fee-bleed-coordinator

Start the Meridian Fee Bleed Remediation project.

The coordinator and tetraphasic workflow have been configured, but the planning artifacts do not exist yet. Start by invoking the architect in **Planning Mode**.

Your task:

1. Analyze the current codebase, focusing specifically on:
   * `src/services/fees/fee_calculator.py` — how per-fill fees are computed and whether fee tiers are resolved dynamically or hardcoded
   * `src/services/fees/schedules/` — the current fee schedule definitions and whether they reflect live exchange rates
   * `src/execution/fill_processor.py` — how raw fills are tagged as maker/taker and how fees are attached to trade records
   * `src/execution/order_router.py` — whether the router considers effective fee rates when selecting venues
   * `src/services/reconciliation/` — what post-trade fee auditing exists today

2. Based on the current codebase reality, design the **fee bleed remediation plan** covering: fee-tier accuracy, maker/taker classification, fee-aware routing, and reconciliation hardening.

3. Produce the following files:

- DOCS/FEE_BLEED_PLAN.md
- DOCS/PHASE_1_ACCEPTANCE_CRITERIA.md
- DOCS/FEE_BLEED_CURRENT_PHASE.md

In the acceptance criteria, include a clearly labeled `Validation Posture` section and a clearly labeled `QA Decision` section.

The plan should:
* identify the root causes of fee bleed based on code inspection
* quantify the expected impact of each fix where possible
* define the data/logging instrumentation needed before behavioral changes
* break the work into distinct, sequential phases (instrumentation first, then fixes, then optimization)
* declare the Phase 1 validation mode (`TEST_FIRST`, `TEST_WITH_IMPLEMENTATION`, or `MANUAL_VALIDATION_ONLY`) and justify it
* declare whether advanced QA is required for Phase 1 and justify it only if the phase truly needs integration, regression, end-to-end, or multi-service validation
* ensure Phase 1 is non-destructive (logging/instrumentation only) and deployable independently
* define regression expectations: all changes must be validated against production fill data

After writing those documents, hand off to the coordinator-driven next phase selected by the architect recommendation.
