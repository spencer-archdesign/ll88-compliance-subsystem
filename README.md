# LL88 Compliance Subsystem  
*Bounded Compliance Module within an Operations Platform*

## Overview

This repository documents the **LL88 (NYC Local Law 88) compliance subsystem** implemented within a broader internal operations platform used to manage properties, projects, invoicing, purchasing, and regulatory workflows.

Rather than existing as a standalone database, LL88 compliance logic operates as a **bounded subsystem** that owns compliance-specific workflows while sharing canonical business entities (e.g., Properties, Projects, Invoices) with the core platform.

This documentation focuses on **architecture, data boundaries, workflows, constraints, and design decisions** — not on platform-specific implementation details.

---


## Documentation
- [Shared vs Owned Data Model](data/shared-vs-owned.md)
- [Tables in Scope](data/tables-in-scope.md)

---

## Problem Statement

NYC Local Law 88 requires building owners to:
- Survey existing lighting systems
- Determine compliance status
- Produce formal documentation
- Coordinate filings and incentive programs
- Meet strict regulatory deadlines

In practice, this process involves:
- Incomplete or inconsistent field data
- Multi-building portfolios
- Manual documentation bottlenecks
- Tight coupling between compliance, operations, and billing

A standalone compliance tool would duplicate data and introduce risk.  
The challenge was to **embed compliance intelligence into an existing operational system without fragmenting data ownership**.

---

## System Context

The LL88 subsystem operates within a **modular monolith** architecture:

- The **core platform** manages:
  - Properties
  - Projects
  - Clients
  - Invoices
  - Purchasing
- The **LL88 subsystem** manages:
  - Compliance rules and interpretations
  - Survey-derived compliance attributes
  - Analysis and reporting workflows
  - Regulatory documentation outputs

This approach preserves a **single source of truth** while allowing compliance logic to evolve independently.

---

## Subsystem Boundary

### LL88 Subsystem Owns
- Compliance status determination
- Lighting inventory classification for LL88
- Rule evaluation (compliant / non-compliant)
- Compliance reporting logic
- Regulatory documentation generation
- Compliance audit history

### Shared (Read/Write via Platform Contracts)
- Properties
- Projects
- Units / Areas
- Invoices
- Client records

### External Touchpoints
- Field survey inputs (manual + mobile)
- Document storage systems
- Utility incentive coordination
- Regulatory filing workflows

---

## Key Workflows

1. **Survey Intake**
   - Field data captured via desktop and mobile workflows
   - Validation against required compliance attributes
   - Flagging of incomplete or ambiguous inputs

2. **Compliance Analysis**
   - Lighting inventory classified against LL88 rules
   - Exceptions and edge cases handled explicitly
   - Compliance status calculated at area, building, and portfolio levels

3. **Reporting & Documentation**
   - Automated generation of compliance summaries
   - Standardized regulatory output formats
   - Traceability back to source survey data

4. **Operational Handoff**
   - Non-compliant findings routed into project workflows
   - Install scope derived from compliance gaps
   - Incentive eligibility aligned with findings

5. **Audit & Change Tracking**
   - Historical snapshots of compliance status
   - Support for re-surveys and rule interpretation changes

---

## Architecture Highlights

- **Bounded context within a shared data model**
- **Event-driven automation** (status changes trigger downstream actions)
- **Server-side processing** for document generation and batch operations
- **Explicit separation between compliance logic and financial workflows**
- **Designed for regulatory ambiguity and exception handling**

---

## Constraints & Tradeoffs

### Constraints
- Regulatory rules subject to interpretation
- Survey data quality varies by site and access
- Deadlines impose hard operational cutoffs
- System must support partial data states

### Tradeoffs Made
- Modular monolith chosen over microservices to reduce operational overhead
- Shared entities preferred over duplication to maintain data integrity
- Documentation-first automation favored over opaque logic

---

## Outcomes

- Reduced manual compliance processing time
- Improved auditability and traceability
- Enabled multi-building compliance management
- Minimized data duplication across operations and compliance

(Quantitative metrics intentionally omitted / anonymized.)

---

## Security & Data Handling

- Client and property data anonymized in this documentation
- No production data, credentials, or proprietary logic included
- All examples use sanitized or representative structures

---

## What I’d Do Differently Today

- Earlier formalization of subsystem boundaries
- Clearer contracts around shared entities
- Greater separation between survey ingestion and compliance evaluation
- Earlier investment in mobile-first survey tooling

These lessons directly informed later architectural decisions.

---

## Related Work

- Mobile survey architecture (in progress)
- Incentive and rebate workflow automation
- Controls and lighting system design subsystems

---

## Author Notes

This repository is intended to demonstrate **systems thinking, architectural decision-making, and real-world constraint handling**, rather than platform-specific implementation details.

Questions and discussion welcome.
