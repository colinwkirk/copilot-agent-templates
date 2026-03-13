# Project Context & AI Guidelines

## Project Identity
- **Project Name:** Meridian (Quantitative Trading Platform)
- **Core Purpose:** A mid-frequency quantitative trading system executing systematic strategies across multiple centralized crypto exchanges (Binance, OKX, Bybit). Handles signal generation, order management, execution, position tracking, and P&L attribution.
- **Tech Stack:** Python 3.12, asyncio, ccxt (exchange connectivity), SQLAlchemy + PostgreSQL (trade/position store), Redis (real-time state and pub/sub), pandas/numpy (signal and analytics), pytest, Docker

## Architectural Guardrails
- **Execution Path:** All orders flow through `OrderRouter` → `ExchangeAdapter` → exchange API. Never send orders directly via ccxt outside the adapter layer.
- **Fee Calculation:** Fee logic is centralized in `services/fees/fee_calculator.py`. Per-fill fee computation must always reference the exchange's fee schedule, not hardcoded constants. Any fee-related change must be validated against production fill data.
- **Position State:** The `PositionTracker` is the single source of truth for open positions. It is updated only by the `FillProcessor`. Never mutate position state outside this path.
- **Data Integrity:** All monetary values use `Decimal` types. Never use `float` for prices, quantities, or fees. Rounding must always be explicit with defined precision.
- **Safety:** Never deploy fee or routing changes without running the reconciliation backtest against the last 30 days of production fills. All changes to the execution path require review sign-off.

## Key Modules

| Module | Path | Responsibility |
|--------|------|----------------|
| Order Router | `src/execution/order_router.py` | Venue selection, order splitting, smart routing |
| Exchange Adapters | `src/execution/adapters/` | Per-exchange API wrappers (Binance, OKX, Bybit) |
| Fee Calculator | `src/services/fees/fee_calculator.py` | Per-fill fee computation, tier resolution |
| Fee Schedules | `src/services/fees/schedules/` | Exchange-specific fee tier definitions |
| Fill Processor | `src/execution/fill_processor.py` | Processes raw fills into normalized trade records |
| Position Tracker | `src/portfolio/position_tracker.py` | Tracks open positions, cost basis, realized P&L |
| Reconciliation | `src/services/reconciliation/` | Post-trade rec, fee audit, discrepancy detection |

## AI Agent Workflow Rules
- This project utilizes a **Tetra-phasic Agent Workflow** (`Architect` -> `Drafter` -> `Implementer` -> `Reviewer`).
- Features must be broken down and strictly scoped via file contracts (`PLAN.md`, `ACCEPTANCE_CRITERIA.md`, `IMPLEMENTATION_DRAFT.md`, `REVIEW.md`).
- Existing systems are off-limits and presumed working. Refactoring untouched code is prohibited unless an explicit "Refactor Phase" is defined and approved by the Architect.
- Do not optimize prematurely. Follow the acceptance criteria exactly.
- **Financial safety:** Changes to fee calculation, order routing, or position tracking require explicit test coverage against known production fill data before approval.
