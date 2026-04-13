@tetraphasic-coordinator

Start the workflow described in `DOCS/WORKFLOW_CONFIG.md`.

Read `DOCS/WORKFLOW_CONFIG.md` first.

Your task:

1. Use the workflow config as the source of truth for artifact paths, guardrails, survey priorities, and packaging assumptions.
2. If the configured current-phase tracker already exists, continue from it.
3. If the configured plan and current-phase acceptance artifacts do not exist yet, invoke the architect in Planning Mode.
4. Use the global generic tetraphasic agents rather than generating project-specific phase agents during execution.
5. Maintain the current-phase tracker before and after every subagent handoff.

If repository reality or packaging assumptions conflict with `config-global-agents`, stop and recommend regeneration using `project-specific-generated-agents`.