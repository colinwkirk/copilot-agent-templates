# Example: Quant Trading Fee Bleed Refactor

This example demonstrates how to set up the tetraphasic workflow for a **deep refactor of an existing complex system** — diagnosing and resolving fee bleed in a quantitative trading application.

## The Problem

A live quant trading system is experiencing **fee bleed**: the execution engine is hemorrhaging value through a combination of suboptimal order routing, redundant partial fills, maker/taker fee misclassification, and stale fee-tier logic that doesn't reflect current exchange schedules. The system is profitable on paper but underperforming its backtest by 12-18 bps after fees — enough to erode alpha on most mid-frequency strategies.

This is exactly the kind of problem that benefits from the tetraphasic workflow:
- The root causes span multiple subsystems (order router, fill reconciliation, fee calculator, position tracker)
- Incorrect changes could cause real financial loss
- The fix requires careful analysis before any code is touched
- Regression testing against production fill data is critical

## What's Included

| File | Purpose |
|------|---------|
| `pre-prompt.md` | Project context, tech stack, and AI guardrails |
| `prompt.md` | Initial prompt to kick off the Coordinator |
| `agents/fee-bleed-coordinator.agent.md` | Coordinator agent — workflow entrypoint and phase orchestrator |
| `agents/fee-bleed-architect.agent.md` | Architect agent definition |
| `agents/fee-bleed-drafter.agent.md` | Drafter agent definition |
| `agents/fee-bleed-implementer.agent.md` | Implementer agent definition |
| `agents/fee-bleed-reviewer.agent.md` | Reviewer agent definition |

## How to Use

1. Copy the `agents/` files into your project's `.github/agents/` directory.
2. Add `pre-prompt.md` content to your `.github/copilot-instructions.md` (or attach it as context).
3. Open Copilot Chat and paste the contents of `prompt.md` to start the coordinator, which will invoke the architect in Planning Mode.

## Likely Phase Breakdown

A real engagement with this workflow might produce phases like:

| Phase | Scope |
|-------|-------|
| **Phase 1** | Instrument the fee calculation path — add structured logging and fee-audit events to capture every fee charged vs. expected fee per fill. No behavioral changes. |
| **Phase 2** | Fix fee-tier resolution — replace the hardcoded tier map with a dynamic lookup that respects the exchange's current VIP/volume-based schedule. |
| **Phase 3** | Fix maker/taker classification — ensure limit orders that rest on the book are correctly tagged as maker, and that IOC/market orders are tagged as taker. |
| **Phase 4** | Optimize order routing — update the router to factor effective fee rates into venue selection, preferring maker-friendly routing where latency permits. |
| **Phase 5** | Reconciliation hardening — add a post-trade reconciliation job that flags fee discrepancies above a threshold and alerts the ops channel. |

Each phase goes through the full Coordinator → Architect → Drafter → Implementer → Reviewer loop before the next begins.
