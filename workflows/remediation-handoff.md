# Workflow: Remediation Handoff & Operational Integration

This document describes the **remediation handoff workflow** that occurs after LL88 compliance evaluation and reporting, transitioning regulatory findings into actionable operational work.

The workflow ensures that compliance outcomes:
- Translate into clear remediation scope
- Integrate cleanly with project and financial systems
- Maintain traceability back to regulatory determinations
- Avoid duplicating or re-interpreting compliance logic

---

## Workflow Objective

Convert **non-compliant or conditionally compliant findings** into structured remediation work while preserving regulatory intent, auditability, and operational clarity.

---

## Preconditions

The workflow begins only when:
- Compliance evaluation has completed
- A finalized compliance report exists
- Non-compliant or conditionally compliant areas are identified

Compliant properties may bypass this workflow entirely.

---

## Actors & Systems

### Actors
- Compliance analyst
- Operations manager
- Project coordinator

### Systems
- LL88 Compliance Subsystem
- Core Operations Platform
- Estimating and purchasing workflows
- Incentive coordination systems

---

## High-Level Flow

1. Remediation eligibility identification
2. Scope derivation
3. Project creation and linkage
4. Financial and incentive alignment
5. Status tracking and feedback loop

Each step maintains a clear boundary between **compliance determination** and **execution planning**.

---

## Step 1: Remediation Eligibility Identification

The subsystem identifies remediation candidates based on:
- Non-compliant areas
- Conditional compliance requiring upgrades
- Exception conditions with remediation paths

Eligibility rules are deterministic and derived from compliance evaluation outputs.

---

## Step 2: Remediation Scope Derivation

For eligible areas, the system derives a **proposed remediation scope**, including:
- Areas requiring upgrades
- Fixture types implicated
- Control deficiencies
- Compliance-driven constraints

### Design Principle
> Remediation scope is derived from compliance findings, not manually re-entered.

This minimizes transcription errors and logic drift.

---

## Step 3: Project Creation & Linkage

Derived remediation scope is handed off to the core Operations Platform.

### Activities
- Create remediation projects
- Link projects to originating compliance records
- Preserve references to evaluated areas and fixtures

Compliance logic does not move into project execution workflows.

---

## Step 4: Financial & Incentive Alignment

Once projects exist:
- Cost estimation workflows are initiated
- Incentive eligibility is evaluated
- Utility and rebate requirements are aligned with compliance findings

The LL88 subsystem provides **context**, not financial logic.

---

## Step 5: Status Tracking & Feedback Loop

As remediation progresses:
- Project status updates are reflected in compliance records
- Completion triggers eligibility for re-survey or re-evaluation
- Compliance state transitions are recorded explicitly

This creates a closed loop between compliance and execution.

---

## Exception Handling

The workflow supports:
- Partial remediation
- Phased upgrades
- Deferred remediation due to access or budget constraints

Exceptions are documented and linked to compliance interpretation records.

---

## Audit & Traceability

For any remediation project, the system can identify:
- Originating compliance determination
- Areas and fixtures driving remediation
- Derived scope assumptions
- Project execution outcomes

This ensures remediation decisions remain defensible and reviewable.

---

## Architectural Intent

This workflow is designed to:
- Keep compliance logic authoritative
- Prevent operational workflows from redefining regulatory outcomes
- Enable scalable remediation across multi-property portfolios
- Support future compliance re-evaluation after remediation

Compliance and execution remain **connected but decoupled**.

---
