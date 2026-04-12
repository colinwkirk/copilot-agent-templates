# Workflow Reuse Check Agent

This package contains two markdown files:

- `workflow-reuse-check.agent.md`
- `workflow-generator-reuse-check-delta.md`

## What this does

The `workflow-reuse-check` agent is a narrow, advisory agent that inspects existing global and repository-local assets and determines whether a request should:

- reuse an existing asset as-is
- adapt an existing asset
- justify creating a new asset

It does **not** generate workflows or write production code.

The delta file contains the exact changes to apply to your existing `global-tetraphasic-workflow-generator` agent so it can hand off to the reuse-check agent first.

## Intended architecture

- `workflow-reuse-check` = librarian / reuse advisor
- `global-tetraphasic-workflow-generator` = factory / workflow generator
- generated tetraphasic workflow packs = coordinator entrypoint plus four phase agents

The name `tetraphasic` remains intentional.
The core workflow still has four substantive phases: Architect, Drafter, Implementer, and Reviewer.
The coordinator and optional QA role are orchestration or extension roles, not extra default phases.

This keeps the workflow generator from being both inventory system and generator at the same time.

## Typical usage

### Direct reuse analysis

```text
@workflow-reuse-check inspect the keyforge-piano repo and global assets for anything that should be reused or adapted for the following request:
<task or blob here>
```

### Workflow generation with handoff

```text
@global-tetraphasic-workflow-generator use the following context to generate or adapt a workflow for the keyforge-piano repo and any dependent repos as needed:
<task or blob here>
```

With the delta applied, the workflow generator should first consult a prior reuse analysis or hand off to `@workflow-reuse-check` before creating new assets, then emit coordinator-driven workflow packs when a full tetraphasic flow is justified.

## Suggested file locations

Global agent:
- `~/.copilot/agents/workflow-reuse-check.agent.md`

Supporting delta/reference doc:
- wherever you keep your human-facing notes, or directly merge the changes into your generator agent file
