# Tetraphasic Agentic Workflow Templates for GitHub Copilot

A collection of foundational templates for implementing a structured, coordinator-driven tetraphasic agentic workflow using GitHub Copilot in VS Code. This repository provides the instructions, agent definitions, and prompts necessary to safely and consistently execute complex tasks using AI assistance.

## Table of Contents

- [The Coordinator-Driven Tetraphasic Workflow](#the-coordinator-driven-tetraphasic-workflow)
- [Repository Structure](#repository-structure)
- [Why Use a Coordinator-Driven Tetraphasic Workflow?](#why-use-a-coordinator-driven-tetraphasic-workflow)
- [Getting Started](#getting-started)
- [Packaging Modes](#packaging-modes)
- [Using `DOCS/WORKFLOW_CONFIG.md`](#using-docsworkflow_configmd)
- [Wiring This Up Globally](#wiring-this-up-globally)
- [Shortcomings Of The Reuse Pattern](#shortcomings-of-the-reuse-pattern)
- [When To Prefer Generated Discrete Agents](#when-to-prefer-generated-discrete-agents)
- [Examples](#examples)

## The Coordinator-Driven Tetraphasic Workflow

This system divides work into a coordinator entrypoint plus four distinct phase roles, ensuring high-quality output through rigorous planning, scoping, implementation, and review. Agents must follow this precise structured development loop:

**Coordinator → Architect → Drafter/Implementer → Reviewer → Coordinator**

An optional QA stage may be inserted only when the approved workflow explicitly requires advanced validation beyond the normal reviewer contract.

0. **Coordinator**
   * **Role:** Orchestrates the workflow loop. Invokes phase agents as subagents, maintains the current-phase tracker, and routes based on each agent's returned signal.
   * **Output:** Updated current-phase tracker after each handoff.
   * **Constraint:** Does not implement code, redesign phases, or relax scope guardrails.

1. **Architect**
   * **Role:** Analyzes the system, defines architecture, and creates phase plans with explicit validation posture and QA decisions.
   * **Output:** Phase plan, acceptance criteria (with `Validation Posture` and `QA Decision` sections).
   * **Constraint:** Never modifies production code.

2. **Drafter**
   * **Role:** Translates approved phases into concrete execution briefs for the Implementer. Identifies relevant files, risks, and edge cases. Translates validation mode into concrete test steps.
   * **Output:** Implementation drafts/briefs.
   * **Constraint:** Does not change architecture or write production code.

3. **Implementer**
   * **Role:** Executes exactly one approved phase or refinement pass. Honors the declared validation mode (`TEST_FIRST`, `TEST_WITH_IMPLEMENTATION`, or `MANUAL_VALIDATION_ONLY`).
   * **Output:** Production code, tests, and an implementation report.
   * **Constraint:** Cannot begin later phases or expand scope.

4. **Reviewer**
   * **Role:** Validates the implementation against the approved phase document. Checks correctness, regressions, and validation mode compliance.
   * **Output:** Review document with a decision to approve, refine, or re-plan.
   * **Constraint:** Does not implement code or re-architect the system.

5. **QA** *(Optional)*
   * **Role:** Handles advanced validation only when the approved workflow explicitly calls for it, such as regression, integration, end-to-end, multi-service, or environment-dependent checks that exceed the normal reviewer role.
   * **Output:** QA report and, when needed, a QA plan.
   * **Constraint:** Not part of the default loop. Should be introduced only when the architect or approved workflow explicitly requires it.

## Repository Structure

```
copilot-agent-templates/
  .github/
    copilot-instructions.md   ← Global workflow rules (copy into your repo)
    agents/
      tetraphasic_workflow_generator_full.agent.md  ← Generates workflow packs
      workflow-reuse-check.agent.md                 ← Checks for reusable assets
    prompts/
      architecture-kickoff.prompt.md
      implementation-handoff.prompt.md
      review-pass.prompt.md
      small-task-triage.prompt.md
      tetraphasic-workflow-generator-global-prompt.prompt.md
      workflow-reuse-check.prompt.md
      copilot_workflow_README.md
  templates/                  ← Template files to copy into your project
    agent-coordinator-template.agent.md
    agent-architect-template.agent.md
    agent-drafter-template.agent.md
    agent-implementer-template.agent.md
    agent-reviewer-template.agent.md
    agent-qa-template.agent.md
    current-phase-tracker-template.md
    pre-prompt-template.md
    prompt-template.md
    example-pre-prompt.md
    example-prompt.md
    copilot-instructions.md
  examples/
    quant-fee-bleed-refactor/
    realtime-collab-editor/
```

## Why Use a Coordinator-Driven Tetraphasic Workflow?

When relying on AI for complex tasks, context windows limit performance. By separating the work into specialized phases orchestrated by a coordinator, this workflow ensures that:
- Code architecture is thoroughly vetted before any code is written.
- Tasks are executed with hyper-focused context windows, preventing AI drift.
- A strict review loop catches mistakes early and stops regressions.
- Validation mode is explicit per phase — preventing ambiguity about when tests should be written.
- Unattended multi-phase runs remain inspectable via the current-phase tracker.
- Temporary files and outputs stay organized organically.

## Getting Started

**Prerequisites:** You will need [VS Code](https://code.visualstudio.com/) with the [GitHub Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat) extension installed.

**Option A — Use the workflow generator (recommended):**
1. Open this repository alongside your target repository in VS Code.
2. Invoke `@global-tetraphasic-workflow-generator` in Copilot Chat with a description of your project or feature.
3. The generator will first decide whether the workflow should use `config-global-agents` or `project-specific-generated-agents`.
4. If that packaging decision is ambiguous, it should ask a short intake question set before scaffolding.
5. It will then produce either a repository-local config-driven workflow setup or a complete coordinator-driven workflow pack tailored to your repository.

Whether the generator emits an optional QA agent depends on the approved validation needs of the workflow. QA should not be generated by default.

### Packaging Modes

- `config-global-agents`: best when you already maintain a stable global tetraphasic agent set in your user profile and want to reduce agent sprawl in VS Code. The repository gets config and kickoff artifacts instead of dedicated per-project phase agents.
- `project-specific-generated-agents`: best when you need the workflow to be self-contained inside the repository, shared with collaborators, or able to run multiple workflows side by side in the same workspace without ambiguous agent targeting.

If multiple workflows may be active in the same workspace at once, `project-specific-generated-agents` is the safer default.

In `config-global-agents` mode, the standard repository-local artifacts are:
- `DOCS/WORKFLOW_CONFIG.md`
- `DOCS/<current-phase-tracker>.md`
- `.github/prompts/<task>-workflow-kickoff.prompt.md`

Those artifacts are consumed by a stable global generic agent set headed by `@tetraphasic-coordinator`.

### Using `DOCS/WORKFLOW_CONFIG.md`

`DOCS/WORKFLOW_CONFIG.md` is the repository-local contract for the config-driven path. The global generic agents read it first and use it to resolve all workflow artifact paths and project-specific guardrails.

At minimum, populate it with:
- project identity: task title, task identifier, project name, core objective
- artifact paths: plan file, current-phase tracker file, and the `PHASE_N` file patterns
- bootstrap settings: initial phase, preferred validation mode, desired Phase 1 focus
- repository survey targets: the modules or directories the architect should inspect first
- domain guardrails: correctness, safety, migration, compatibility, or testing constraints
- packaging assumptions: whether multiple workflows will run in the workspace and whether config mode is still safe

How it works in practice:
1. The kickoff prompt calls `@tetraphasic-coordinator`.
2. The coordinator reads `DOCS/WORKFLOW_CONFIG.md` first.
3. The coordinator reads the configured current-phase tracker and resolves the active phase artifact paths.
4. Each generic phase agent also reads `DOCS/WORKFLOW_CONFIG.md` first before touching plan, acceptance, draft, implementation, review, or QA artifacts.
5. If repository reality conflicts with the packaging assumptions in the config, the workflow should stop and be regenerated using `project-specific-generated-agents`.

Use [workflow-config-template.md](/Users/colinwkirk/workspace/copilot-agent-templates/templates/workflow-config-template.md) as the starting point.

### Wiring This Up Globally

The config-driven path assumes the generic tetraphasic agents live in your user profile rather than in each repository.

Recommended setup:
1. Keep these global agents in `.copilot/agents/`:
   `tetraphasic-coordinator`, `tetraphasic-architect`, `tetraphasic-drafter`, `tetraphasic-implementer`, `tetraphasic-reviewer`, and `tetraphasic-qa`.
2. Keep the shared prompts in `.copilot/prompts/`, especially the generator prompt and workflow README prompt files.
3. Ensure VS Code is configured to discover your global prompts and agents from your `.copilot` profile.
4. In each target repository, generate or create only the repository-local config artifacts:
   `DOCS/WORKFLOW_CONFIG.md`, the current-phase tracker, and the kickoff prompt.
5. Start the workflow from the repository-local kickoff prompt, which should invoke `@tetraphasic-coordinator`.

The practical split is:
- global `.copilot`: reusable generic agents and reusable generator logic
- repository `.github` and `DOCS/`: task-specific config, prompts, tracker, and workflow artifacts

If collaborators will not share the same global `.copilot` setup, prefer `project-specific-generated-agents` instead.

### Using This With Copilot CLI

This repository is optimized for VS Code custom agents and prompts first. If you are using a Copilot CLI environment, the practical model is slightly different.

What transfers cleanly to CLI use:
- the repository-local workflow artifacts in `DOCS/` and `.github/prompts/`
- the workflow shape itself: coordinator planning, phased execution, review discipline, and optional QA
- the config-driven path centered on `DOCS/WORKFLOW_CONFIG.md`

What usually does not transfer one-to-one:
- automatic discovery of VS Code custom agents from `.copilot/agents/` or `.github/agents/`
- subagent orchestration exactly the way VS Code custom agent mode handles it
- prompt-file UX that depends on the editor surfacing prompt and agent pickers

Recommended CLI usage:
1. Use the workflow generator in VS Code to scaffold the workflow artifacts when possible.
2. If you are running from CLI, treat the generated kickoff prompt and `DOCS/WORKFLOW_CONFIG.md` as the primary inputs.
3. Paste or reference the kickoff prompt content directly in the CLI session.
4. Keep the workflow state in the repository files exactly as described by the config and current-phase tracker.
5. Use the CLI as the execution surface, but treat the repository-local workflow files as the source of truth.

Practical recommendation:
- use VS Code when you want full custom-agent orchestration
- use CLI when you mainly want the workflow documents, prompts, and phase discipline without depending on editor-specific agent discovery

### Shortcomings Of The Reuse Pattern

The `config-global-agents` path is lighter, but it is not free.

Common shortcomings:
- hidden dependency on user profile state: the workflow depends on the global generic agents being present and current in `.copilot`
- weaker team portability: collaborators may have the repository config files but not the same global agents or prompt setup
- ambiguity under concurrency: if you want to run multiple workflows side by side in one workspace, generic global agents are easier to confuse than dedicated project agents
- less baked-in domain context: highly specialized guardrails live in `DOCS/WORKFLOW_CONFIG.md` instead of directly in a dedicated agent definition
- harder auditability: a repository-local workflow pack is easier to inspect, review, and commit as a complete self-contained system
- more reliance on packaging assumptions: if those assumptions drift, the workflow may need to stop and be regenerated in discrete-agent mode

### When To Prefer Generated Discrete Agents

Use the workflow generator to create repository-local discrete agents when:
- multiple workflows may be active in the same workspace at once
- the workflow must be committed, shared, and reproducible for collaborators without requiring matching user-profile setup
- the domain is unusually specialized, safety-critical, or constraint-heavy and you want those constraints embedded directly in the phase agent definitions
- you expect long-lived project-specific workflows that should remain inspectable as repository assets
- you want stronger operational clarity about which exact agent belongs to which task or initiative

In other words:
- prefer `config-global-agents` for low-friction reuse and reduced agent sprawl
- prefer `project-specific-generated-agents` for portability, concurrency safety, and stronger per-project encapsulation

In either packaging mode, the optional QA agent should be treated as an extension role, not as part of the default tetraphasic loop.

**Option B — Copy templates manually:**
1. Copy the contents of `templates/` into your project (agent files go in `.github/agents/`, prompt files in `.github/prompts/`).
2. Copy `.github/copilot-instructions.md` into your project's `.github/` folder.
3. Replace all `${placeholder}` values with your task-specific names.
4. Use the provided prompt template to initialize the coordinator.

## Examples

The `examples/` directory contains two fully worked-out configurations showing how to adapt the templates for real-world scenarios:

### [Real-Time Collaborative Document Editor](examples/realtime-collab-editor/) (From Scratch)
A greenfield project building a CRDT-based collaborative editor with WebSocket sync, presence awareness, and version history. Demonstrates how to use the workflow to safely decompose a complex new system into implementable phases.

### [Quant Trading Fee Bleed Refactor](examples/quant-fee-bleed-refactor/) (Deep Refactor)
A live quantitative trading system losing alpha to fee bleed across multiple exchanges. Demonstrates how to use the workflow for a high-stakes refactor where correctness is critical, starting with non-destructive instrumentation before making any behavioral changes.

Each example includes a complete coordinator agent, all four phase agent definitions, a pre-prompt with project context, and a starter prompt ready to paste into Copilot Chat.
