# Tetraphasic Agentic Workflow Templates for GitHub Copilot

A collection of foundational templates for implementing a structured, coordinator-driven tetraphasic agentic workflow using GitHub Copilot in VS Code. This repository provides the instructions, agent definitions, and prompts necessary to safely and consistently execute complex tasks using AI assistance.

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
3. The generator will produce a complete coordinator-driven workflow pack tailored to your repository.

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
