# ADR-001: Treat LL88 as a Bounded Compliance Subsystem within the Core Operations Platform

## Status
Accepted

## Context

LL88 compliance workflows (survey intake, evaluation, reporting, and remediation handoff) operate within a broader operations platform that already owns canonical business entities such as Properties, Projects, Clients, Areas/Units, and Invoices.

Key constraints and realities:
- Compliance logic changes over time (regulatory updates, interpretations, edge cases).
- Field data is often incomplete or ambiguous and requires iterative enrichment.
- Outputs must be auditable and reproducible (snapshots, report versions, retained artifacts).
- The system must support operational follow-through (remediation work) without mixing regulatory determination with execution planning.
- The platform is a “modular monolith” in practice: multiple operational domains share a single database/application surface area.

We needed an architecture that:
- Preserves a single source of truth for canonical entities
- Allows compliance logic to evolve safely
- Prevents semantic drift across subsystems
- Supports traceability and audit readiness

## Decision

Implement LL88 as a **bounded compliance subsystem** inside the Core Operations Platform rather than as a standalone application/service.

This includes:
- **LL88-owned** tables/capabilities for compliance records, inventory interpretation, rule evaluation, snapshots, reporting logic, and exceptions
- **Platform-owned** canonical entities (Properties, Projects, Clients, Areas/Units, Invoices, Purchasing) referenced via relationships rather than duplicated
- A workflow model where:
  - intake performs structural validation + normalization
  - evaluation is snapshot-driven and version-aware
  - reporting publishes versioned artifacts
  - remediation is handed off to operations via explicit linkage/events, not embedded execution logic

## Alternatives Considered

### A) Standalone LL88 Application (separate database / separate codebase)
**Pros**
- Strong isolation from the operations platform
- Independent deployment and evolution

**Cons**
- Requires synchronization/duplication of canonical entities (Properties, Areas, Projects, Clients)
- Higher operational overhead (integration layer, data contracts, reconciliation)
- Increased risk of semantic drift between “compliance truth” and “operational truth”
- More complex traceability across systems (audit trail spans multiple stores)

### B) Embed LL88 fields directly into platform-owned tables (“compliance as columns”)
**Pros**
- Low initial implementation overhead
- No new subsystem boundary to manage

**Cons**
- High risk of contaminating canonical entities with regulatory interpretation
- Harder to version compliance logic and snapshots cleanly
- Increased blast radius when compliance rules change
- Encourages silent coupling between compliance logic and unrelated workflows

### C) Microservice Extraction (LL88 as a service with APIs/events)
**Pros**
- Clear runtime boundary, independent scaling and release cadence

**Cons**
- Not aligned with current platform constraints and operating model
- Requires durable eventing and contract governance
- Adds integration complexity without clear near-term payoff
- Still requires careful handling of canonical entity ownership

## Consequences

### Positive
- Compliance logic is isolated enough to evolve without destabilizing core operations.
- Canonical entities remain authoritative and shared across all workflows.
- Auditability is improved via snapshot-driven evaluation and versioned report outputs.
- Remediation can be operationalized without reinterpreting regulatory outcomes.
- Future extraction remains possible (subsystem boundary is explicit).

### Negative / Tradeoffs
- Requires discipline to maintain boundaries (avoid “just add a field to Properties” shortcuts).
- Schema changes to shared entities still require coordination and impact analysis.
- Some architectural benefits of service separation (independent deployment/scaling) are deferred.

## Notes

This ADR intentionally separates:
- **Compliance determination** (LL88 subsystem) from
- **Execution planning and delivery** (operations workflows)

This boundary is reinforced by:
- explicit ownership definitions
- snapshot-based evaluation artifacts
- clean remediation handoff patterns
