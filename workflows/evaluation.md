# Workflow: Compliance Evaluation & Determination

This document describes the **compliance evaluation workflow** within the LL88 Compliance Subsystem, covering how validated survey data is evaluated against regulatory requirements to produce a formal compliance determination.

The workflow is designed to:
- Apply regulatory rules consistently
- Preserve interpretive judgment where required
- Support re-evaluation as data or interpretations change
- Produce auditable, reproducible outcomes

---

## Workflow Objective

Transform **evaluation-ready survey data** into a clear, defensible LL88 compliance determination at the area, building, and portfolio levels.

---

## Preconditions

The workflow begins only when:
- Survey intake has completed validation and normalization
- Compliance readiness has been explicitly confirmed
- Required exception flags and interpretation notes are present

This prevents accidental or premature evaluation.

---

## Actors & Systems

### Actors
- Compliance analyst
- Operations reviewer

### Systems
- LL88 Compliance Subsystem
- Core Operations Platform (context only)
- Document generation services

---

## High-Level Flow

1. Rule selection and scoping
2. Area-level evaluation
3. Aggregation and roll-up
4. Exception handling
5. Compliance determination
6. Snapshot and persistence

Each step produces explicit artifacts to support traceability.

---

## Step 1: Rule Selection & Scoping

Applicable LL88 rules are selected based on:
- Building type
- Area classification
- Survey date and regulatory version
- Known exemptions or grandfathered conditions

### Design Principle
> Rules are versioned and applied explicitly, never implicitly.

This ensures reproducibility over time.

---

## Step 2: Area-Level Evaluation

Each surveyed area is evaluated independently.

### Evaluation Inputs
- Normalized lighting inventory
- Control strategies present
- Exception flags
- Interpretation notes

### Evaluation Outcomes
- Compliant
- Non-compliant
- Indeterminate (requires review)

Area-level evaluation is **deterministic**, given the same inputs and rule version.

---

## Step 3: Aggregation & Roll-Up

Area-level results are aggregated to:
- Building-level compliance
- Project-level compliance (where applicable)
- Portfolio summaries

### Aggregation Rules
- A single non-compliant area may drive building-level non-compliance
- Indeterminate areas block final determination
- Explicit weighting rules are documented and consistent

Aggregation logic is isolated from area-level rules.

---

## Step 4: Exception & Interpretation Handling

Not all conditions can be evaluated mechanically.

### Supported Exception Types
- Physical access limitations
- Structural constraints
- Conflicting survey observations
- Regulatory ambiguity

Exceptions:
- Are documented in LL88-owned interpretation tables
- Require human review and sign-off
- Are explicitly reflected in compliance outcomes

No exception silently overrides rule evaluation.

---

## Step 5: Compliance Determination

Once aggregation and exception handling are complete, the subsystem produces a **formal compliance determination**.

### Possible States
- Compliant
- Non-compliant
- Conditionally compliant
- Undetermined (requires follow-up)

The determination is attached to a compliance record and timestamped.

---

## Step 6: Snapshot & Persistence

To ensure auditability:
- Evaluation inputs are frozen
- Rule versions are recorded
- Outputs are stored as immutable snapshots

Subsequent changes require explicit re-evaluation.

---

## Downstream Triggers

A completed compliance determination may trigger:
- Regulatory reporting workflows
- Project creation for remediation
- Incentive eligibility analysis
- Client notification workflows

Triggers are event-driven and decoupled from evaluation logic.

---

## Re-Evaluation & Change Management

The system supports re-evaluation due to:
- Updated survey data
- Rule interpretation changes
- Regulatory updates
- Correction of prior errors

Re-evaluations:
- Create new snapshots
- Preserve historical determinations
- Maintain a full compliance timeline

---

## Audit & Defensibility

For any compliance outcome, the system can provide:
- Rule versions applied
- Survey data used
- Interpretation notes
- Exception approvals
- Historical comparisons

This supports both internal QA and external regulatory review.

---

## Architectural Intent

This workflow prioritizes:
- Explicit decision points
- Separation of automation and judgment
- Reproducibility over convenience
- Traceability over opacity

Compliance evaluation is treated as a **formal decision process**, not a background calculation.

---
