# PRAXIS MANTIS v3.6  
**Policy Planning Engine — Cost-Aware, Budget-Governed**

**Author:** Samuel Lawson  
**Division:** Dark Science Division  
**Associated Engine:** PRAXIS TITAN P6.x  
**System Class:** Governed Analytic Planner  
**Execution Mode:** Offline, deterministic, file-driven  
**Autonomy Level:** Decision support only (no execution, no mutation)

---

## 1. System Overview

PRAXIS MANTIS is a governed policy planning engine designed to operate on top of a validated PRAXIS TITAN baseline risk state.

MANTIS does not modify system state, execute mitigations, or autonomously act.  
Its sole function is to **select, rank, and sequence mitigation policies under hard budget constraints**, producing a fully auditable plan artifact.

The system is explicitly designed for safety-grade analytic workflows, where cost, risk, and governance must remain visible, deterministic, and human-controlled.

---

## 2. Core Purpose

MANTIS exists to answer a single question:

> **“Given the current risk state and a fixed budget, what mitigation actions are affordable, effective, and justifiable?”**

It achieves this by:

- Selecting mitigation policies that reduce system failure probability (`p_top`)
- Enforcing absolute budget ceilings using ISC events
- Preventing unsafe, unaffordable, or speculative actions
- Producing a deterministic, auditable mitigation plan
- Operating identically across urgency modes

MANTIS plans only. Execution is always external.

---

## 3. Explicit Non-Capabilities

MANTIS is intentionally constrained.

It does **not**:

- Execute mitigation actions  
- Mutate TITAN state  
- Override or refund budgets  
- Self-authorize execution  
- Depend on cloud services  
- Perform probabilistic inference or learning  
- Operate as an autonomous agent  

All behavior is explicit, bounded, and auditable.

---

## 4. Operational Inputs & Outputs

### Inputs

| Artifact | Description |
|--------|------------|
| `baseline_summary.json` | Canonical TITAN P6.x output (risk state, `p_top`, drivers, ISC history) |
| Environment Variables | Optional runtime flags (e.g. `PRAXIS_NO_CHRONOS`) |
| CLI Arguments | Execution mode selection (`--mode HYBRID \| ASAP \| EFF`) |

### Outputs

| Artifact | Description |
|--------|------------|
| `mantis_plan.json` | Immutable mitigation plan (policy selection + explanations) |
| Console Trace | Deterministic execution log |
| Chronos Entry | Optional, disabled by default |

Outputs are write-only. No runtime mutation occurs.

---

## 5. Execution Modes

Execution mode affects policy ranking only — **never legality**.

| Mode | Behavior |
|----|---------|
| **HYBRID** | Balanced: goal-driven with cost awareness |
| **ASAP** | Aggressive: fastest goal compliance under budget |
| **EFF** | Efficiency-maximizing: best risk reduction per ISC |

Budget rules are absolute across all modes.

---

## 6. Budget Governance Model

### Cost Unit
- **ISC Events** (Internal System Cost)

### Budget Rules
- Absolute ceiling  
- No negative balances  
- No soft overruns  
- No refunds  
- Fail-closed enforcement  

### Enforcement Points
- Pre-selection affordability gate  
- Per-policy affordability check  
- Final plan validation  

### Budget Controller Output (Authoritative)

```json
{
  "ok": true,
  "ceiling": 120.0,
  "spent": 105.2,
  "remaining": 14.888,
  "units": "isc_events"
}

---

## 2) FULL DELETE → Replace `CHANGELOG.md`

```powershell
@'
# PRAXIS MANTIS — Changelog

All notable changes to this project are documented in this file.

This project follows a research-grade, versioned release model.
Backward compatibility is preserved unless explicitly stated.

---

## [v3.6] — Governance-Ready Planning Release

### Added
- Explainable policy ranking output
- Deterministic candidate scoring and ordering
- Bounded multi-step mitigation planning
- Explicit refusal and halt semantics
- Budget-first, fail-closed planning guarantees
- Human-auditable mitigation plan artifacts (`mantis_plan.json`)

### Verified
- Absolute ISC budget enforcement
- No state mutation under any execution mode
- Identical legality rules across HYBRID / ASAP / EFF modes
- Policy fallback behavior under constrained budgets
- Deterministic output under identical inputs

### Intentionally Disabled
- Chronos audit commit (disabled by default)
- Autonomous execution
- Recursive or self-authorizing behavior

### Notes
This release formalizes PRAXIS MANTIS as a **governed policy planning engine**, not an actor.

MANTIS plans only.  
Execution authority remains external.

---
