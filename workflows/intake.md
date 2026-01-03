# Workflow: Survey Intake → Compliance Evaluation

This document describes the **survey intake workflow** for the LL88 Compliance Subsystem, from initial data capture through compliance evaluation readiness.

The workflow is designed to handle:
- Incomplete or inconsistent field data
- Regulatory ambiguity
- Iterative data enrichment
- Clear separation between data capture and rule evaluation

---

## Workflow Objective

Convert raw field survey inputs into a **validated, normalized compliance dataset** suitable for LL88 rule evaluation and reporting, while preserving traceability back to original observations.

---

## Actors & Systems

### Actors
- Field surveyor
- Compliance analyst
- Operations staff

### Systems
- Core Operations Platform
- LL88 Compliance Subsystem
- Mobile survey tooling (where applicable)
- Document storage services

---

## High-Level Flow

1. Survey data capture
2. Intake validation
3. Normalization & enrichment
4. Compliance readiness check
5. Handoff to evaluation workflow

Each stage is intentionally **idempotent** and may be revisited as data improves.

---

## Step 1: Survey Data Capture

Survey data is collected at the area and fixture level and may originate from:
- Desktop-based intake forms
- Mobile field survey tools (offline-first where required)

### Characteristics
- Partial data is allowed
- Unknown values are explicitly captured
- Freeform notes are encouraged for ambiguous conditions

### Design Principle
> It is preferable to capture uncertain data than to block intake.

---

## Step 2: Intake Validation

Upon submission, survey records undergo **structural validation**, not compliance validation.

### Validation Checks
- Required identifiers present (Property, Area, Survey Date)
- Fixture counts and locations are internally consistent
- Reference data values are valid (e.g., fixture type, control type)

### Outcomes
- Records may be accepted with warnings
- Blocking errors are reserved for structural failures only

This prevents early-stage logic from suppressing real-world variability.

---

## Step 3: Normalization & Enrichment

Validated survey inputs are normalized into LL88-owned tables.

### Activities
- Map raw inputs to standardized classifications
- Resolve reference data relationships
- Attach survey records to the appropriate compliance record
- Preserve original input values for auditability

### Key Rule
> Normalization never destroys source context.

---

## Step 4: Compliance Readiness Check

Before rule evaluation, the subsystem determines whether data is **evaluation-ready**.

### Readiness Criteria
- Minimum required attributes populated
- No unresolved blocking ambiguities
- Explicit flags set for known exceptions

### Possible States
- Ready for evaluation
- Ready with exceptions
- Not ready (requires follow-up)

This allows compliance evaluation to be **intentional**, not implicit.

---

## Step 5: Handoff to Compliance Evaluation

Once ready:
- Survey data is locked for evaluation
- A compliance evaluation event is triggered
- Downstream workflows (reporting, project derivation) become eligible

Survey records remain editable, but evaluation snapshots are immutable.

---

## Exception Handling

The workflow explicitly supports:
- Re-surveys
- Partial area coverage
- Conflicting observations
- Regulatory edge cases requiring human interpretation

Exceptions are documented in LL88-owned interpretation tables rather than embedded in core data.

---

## Audit & Traceability

For every compliance outcome, the system can trace:
- Source survey records
- Validation decisions
- Normalization mappings
- Interpretation notes

This supports regulatory audits and internal review.

---

## Architectural Intent

This workflow is designed to:
- Decouple data capture from regulatory logic
- Support iterative data quality improvement
- Minimize rework under changing interpretations
- Scale across multi-building portfolios

The intake process favors **clarity and traceability** over premature enforcement.

---
