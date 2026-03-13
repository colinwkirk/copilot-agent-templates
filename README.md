# Tetraphasic Agentic Workflow Templates for GitHub Copilot

A collection of foundational templates for implementing a structured, four-phase (tetraphasic) agentic workflow using GitHub Copilot in VS Code. This repository provides the instructions, agent definitions, and prompts necessary to safely and consistently execute complex tasks using AI assistance.

## The Tetraphasic Workflow

This system divides work into four distinct roles, ensuring high-quality output through rigorous planning, scoping, implementation, and review. Agents must follow this precise structured development loop:

1. **Architect**
   * **Role:** Analyzes the system, defines architecture, and creates phase plans.
   * **Output:** Phase plans & acceptance criteria.
   * **Constraint:** Never modifies production code.

2. **Drafter**
   * **Role:** Translates approved phases into concrete execution briefs for the Implementer. Identifies relevant files, risks, and edge cases.
   * **Output:** Implementation drafts/briefs.
   * **Constraint:** Does not change architecture or write production code.

3. **Implementer**
   * **Role:** Executes exactly one approved phase or refinement pass based directly on the Architect's acceptance criteria and the Drafter's brief.
   * **Output:** Production code, tests, and an implementation report.
   * **Constraint:** Cannot begin later phases or expand scope.

4. **Reviewer**
   * **Role:** Validates the implementation against the approved phase document. Checks for correctness, regressions, and missed acceptance criteria.
   * **Output:** Review document with a decision to approve, refine, or re-plan.
   * **Constraint:** Does not implement code or re-architect the system.

## Repository Structure

The core files meant to be integrated into your workspace's `.github/` folder:

* **`copilot-instructions.md`**: Global Copilot instructions that define the workflow constraints, agent responsibilities, execution safety, and phase discipline. This ensures all models operating in the repository abide by the core rules.
* **`agents/`**: Contains the individual agent definitions defining their behavior and scope.
  * `agent-architect-template.agent.md`
  * `agent-drafter-template.agent.md`
  * `agent-implementer-template.agent.md`
  * `agent-reviewer-template.agent.md`
* **`prompts/`**: Reusable templates for prompting the system and orchestrating the workflow.
  * `pre-prompt-template.md`
  * `prompt-template.md`

## Why Use a Tetraphasic Workflow?

When relying on AI for complex tasks, context windows limit performance. By separating the work into specialized phases, this workflow ensures that:
- Code architecture is thoroughly vetted before any code is written.
- Tasks are executed with hyper-focused context windows, preventing AI drift.
- A strict review loop catches mistakes early and stops regressions.
- Temporary files and outputs stay organized organically.

## Getting Started

**Prerequisites:** You will need [VS Code](https://code.visualstudio.com/) with the [GitHub Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat) extension installed.

To integrate this workflow into your own projects:
1. Copy the contents of the `.github/` directory into the `.github/` folder at the root of your target repository.
2. Review and customize the global `copilot-instructions.md` to match your project's coding standards, tech stack, or build tool requirements.
3. Update the templates in `agents/` and `prompts/` according to your specific needs (e.g. replace placeholder descriptions).
4. Use the provided prompts in Copilot Chat to initialize the Architects to begin your feature requests, bug fixes, or chores loop-by-loop.

## Examples

The `examples/` directory contains two fully worked-out configurations showing how to adapt the templates for real-world scenarios:

### [Real-Time Collaborative Document Editor](examples/realtime-collab-editor/) (From Scratch)
A greenfield project building a CRDT-based collaborative editor with WebSocket sync, presence awareness, and version history. Demonstrates how to use the workflow to safely decompose a complex new system into implementable phases.

### [Quant Trading Fee Bleed Refactor](examples/quant-fee-bleed-refactor/) (Deep Refactor)
A live quantitative trading system losing alpha to fee bleed across multiple exchanges. Demonstrates how to use the workflow for a high-stakes refactor where correctness is critical, starting with non-destructive instrumentation before making any behavioral changes.

Each example includes a complete set of agent definitions, a pre-prompt with project context, and a starter prompt ready to paste into Copilot Chat.
