PRAXIS MANTIS v3.6

Policy Planning Engine — Cost-Aware, Budget-Governed

Author: Samuel Lawson
Division: Dark Science Division
Associated Engine: PRAXIS TITAN P6.x
System Class: Governed Analytic Planner
Execution Mode: Offline, deterministic, file-driven
Autonomy Level: Decision support only (no execution, no mutation)

1. System Overview

PRAXIS MANTIS is a governed policy planning engine designed to operate on top of a validated PRAXIS TITAN baseline risk state.

MANTIS does not modify system state, execute mitigations, or autonomously act.
Its sole function is to select, rank, and sequence mitigation policies under hard budget constraints, producing a fully auditable plan artifact.

The system is explicitly designed for safety-grade analytic workflows, where cost, risk, and governance must remain visible, deterministic, and human-controlled.

2. Core Purpose

MANTIS exists to answer a single question:

“Given the current risk state and a fixed budget, what mitigation actions are affordable, effective, and justifiable?”

It achieves this by:

Selecting mitigation policies that reduce system failure probability (p_top)

Enforcing absolute budget ceilings using ISC events

Preventing unsafe, unaffordable, or speculative actions

Producing a deterministic, auditable mitigation plan

Operating identically across urgency modes

MANTIS plans only.
Execution is always external.

3. Explicit Non-Capabilities

MANTIS is intentionally constrained.

It does not:

Execute mitigation actions

Mutate TITAN state

Override or refund budgets

Self-authorize execution

Depend on cloud services

Perform probabilistic inference or learning

Operate as an autonomous agent

All behavior is explicit, bounded, and auditable.

4. Operational Inputs & Outputs
Inputs
Artifact	Description
baseline_summary.json	Canonical TITAN P6.x output (risk state, p_top, drivers, ISC history)
Environment Variables	Optional runtime flags (e.g. PRAXIS_NO_CHRONOS)
CLI Arguments	Execution mode selection (`--mode HYBRID
Outputs
Artifact	Description
mantis_plan.json	Immutable mitigation plan (policy selection + explanations)
Console Trace	Deterministic execution log
Chronos Entry	Optional, disabled by default

Outputs are write-only.
No runtime mutation occurs.

5. Execution Modes

Execution mode affects policy ranking only, never legality.

Mode	Behavior
HYBRID	Balanced: goal-driven with cost awareness
ASAP	Aggressive: fastest goal compliance under budget
EFF	Efficiency-maximizing: best risk reduction per ISC

Budget rules are absolute across all modes.

6. Budget Governance Model
Cost Unit

ISC Events (Internal System Cost)

Budget Rules

Absolute ceiling

No negative balances

No soft overruns

No refunds

Fail-closed enforcement

Enforcement Points

Pre-selection affordability gate

Per-policy affordability check

Final plan validation

Budget Controller Output (Authoritative)
{
  "ok": true,
  "ceiling": 120.0,
  "spent": 105.2,
  "remaining": 14.888,
  "units": "isc_events"
}


If a policy exceeds remaining budget, it is unselectable.

7. Policy Model

Policies are explicit, atomic, and budget-filterable objects.

Canonical Policy Fields
Field	Purpose
name	Stable policy identifier
action_type	Semantic category (patch, scan, recalibration)
target	System component
expected_delta_p_top	Risk reduction
expected_isc_cost	Budget cost
r_eff	Efficiency metric
expected_new_p_top	Post-policy projected state
goal_compliant_after	Goal satisfaction flag

Policies are never inferred or synthesized at runtime.

8. Active Policy Catalog (v3.6)

POLICY_B_FAST_PATCH

Low cost

Small risk reduction

Not goal-compliant alone

POLICY_A_DEEP_SCAN

Moderate cost

High validation value

Often goal-compliant

POLICY_C_PARTIAL_RECAL

Critical fallback policy

Affordable under constrained budgets

Enables progress when full recalibration is blocked

FULL_RECAL is intentionally excluded when unaffordable.

9. Planning Logic (High-Level)
Load baseline
↓
Extract p_top and goal
↓
Query budget controller
↓
Filter policies by affordability
↓
Rank by mode-specific heuristic
↓
Select best viable policy (or sequence)
↓
Write plan to disk
↓
(Optional) Chronos commit


At no point is system state mutated.

10. Explainability Layer (v3.6+)

MANTIS produces explainable rankings without altering budget authority.

Each candidate policy includes:

Affordability status

Mode-specific score

Tie-breaker rationale

Human-readable explanation bullets

Blocked policies may be included for audit transparency.

Explainability is descriptive, not justificatory.

11. Bounded Multi-Step Planning

MANTIS can emit a bounded mitigation sequence under strict rules:

Fixed maximum steps (K)

Simulated budget ledger

No refunds

No repeats

Fail-closed termination

Multi-step plans are recommendations, not commitments.

12. Failure Modes (Intentional)
Condition	Result
No affordable policy	REFUSAL
Budget exhausted	HALT
Missing baseline	ABORT
Engine failure	Graceful fail-closed

No silent success is permitted.

13. Engineering Blueprint
Core Modules
praxis_core/
└── mantis/
    ├── runtime.py            # Orchestrator / CLI
    ├── policy_engine.py      # Policy catalog + ranking
    ├── budget_controller.py  # ISC accounting
    ├── schemas.py            # Optional validation

Data Flow
TITAN → baseline_summary.json
      → MANTIS runtime
      → policy_engine
      → budget_controller
      → mantis_plan.json

14. Code DNA (Design Philosophy)
Architectural Traits

Deterministic

File-driven

Explicit over implicit

Budget-first, goal-second

Fail-closed

Human-auditable

This System Is Not

An LLM agent

A reinforcement learner

An autonomous optimizer

A self-executing controller

15. Security & Governance Posture

Human-in-the-loop by design

Immutable outputs

Explicit cost visibility

No self-authorization

No recursive execution

MANTIS meets safety-grade analytic system standards.

16. Current Status (v3.6)

✅ Runtime stable

✅ Budget enforcement verified

✅ Policy fallback functioning

✅ Multi-mode support verified

⚠ Schema warnings acknowledged (non-fatal)

❌ Chronos disabled by default (intentional)

17. Approved Non-Breaking Extensions

Explainable policy ranking enhancements

Multi-step mitigation sequencing

Formal schema enforcement

Chronos audit re-enablement

PRAXIS MANTIS is a planner, not an actor.
Cost authority is absolute.
Failure is explicit.
Silence is forbidden.

