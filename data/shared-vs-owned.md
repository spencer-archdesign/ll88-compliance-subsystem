# Shared vs Owned Data Model

This document defines **data ownership boundaries** between the LL88 Compliance Subsystem and the broader Operations Platform.

The goal is to ensure:
- Clear authority over data meaning and lifecycle
- Minimal duplication of canonical business entities
- Predictable change impact across subsystems
- Long-term maintainability within a modular monolith

---

## Definitions

### Owned Tables
Tables for which the **LL88 Compliance Subsystem** is the system of record.  
These tables exist specifically to support LL88 compliance logic and would not exist independently of the subsystem.

The LL88 subsystem:
- Defines schema and semantics
- Controls lifecycle and validation rules
- Owns change management and interpretation

### Shared Tables
Tables owned by the **core Operations Platform** and consumed by multiple subsystems, including LL88.

The core platform:
- Defines canonical meaning
- Controls schema evolution
- Enforces business-wide invariants

The LL88 subsystem:
- Reads and references these entities
- May write to them only via established platform contracts
- Must not redefine or reinterpret their meaning

---

## LL88-Owned Tables

These entities are **compliance-specific** and fully owned by the LL88 subsystem.

| Table | Responsibility |
|----|----|
| `LL88_Compliance_Record` | Canonical compliance state for a property or project |
| `LL88_Lighting_Inventory` | Lighting inventory as interpreted for LL88 analysis |
| `LL88_Rule_Evaluation` | Rule execution results and exception handling |
| `LL88_Report_Snapshot` | Immutable record of generated compliance outputs |
| `LL88_Audit_Log` | Compliance-specific change history and traceability |
| `LL88_Interpretation_Notes` | Human interpretation of ambiguous conditions |

**Ownership Rationale:**  
If LL88 compliance were removed from the platform, these tables would have no remaining business purpose.

---

## Platform-Owned Shared Tables

These entities represent **core business primitives** shared across multiple workflows.

| Table | Canonical Owner | LL88 Usage |
|----|----|----|
| `Properties` | Core Platform | Compliance scope and physical context |
| `Projects` | Core Platform | Work derived from compliance findings |
| `Clients` | Core Platform | Legal ownership and responsibility |
| `Areas / Units` | Core Platform | Survey and compliance granularity |
| `Invoices` | Core Platform | Billing and financial linkage |

**Consumption Rules:**
- LL88 must respect platform-defined meanings
- LL88 may enrich via relationships, not reinterpretation
- Structural changes require coordination with the core platform

---

## Shared Reference & Lookup Tables

These tables are **read-only from the LL88 perspective** and provide standardized classification data.

| Table | Owner | Purpose |
|----|----|----|
| `Lighting_Types` | Core Platform | Fixture classification |
| `Mounting_Types` | Core Platform | Physical installation context |
| `Control_Types` | Core Platform | Sensors, timers, control strategies |
| `Wattage_Ranges` | Core Platform | Validation and normalization |
| `Area_Types` | Core Platform | Hallway, stairwell, garage, etc. |

**Design Intent:**  
Standardized reference data ensures consistent interpretation across compliance, operations, and financial workflows.

---

## Boundary Enforcement Principles

- **Single Source of Truth:**  
  Canonical business entities are never duplicated for convenience.

- **No Semantic Drift:**  
  LL88 logic must adapt to platform entities — not the other way around.

- **Explicit Contracts:**  
  Changes to shared entities are treated as cross-subsystem events.

- **Isolation of Interpretation:**  
  Regulatory ambiguity and human judgment live in LL88-owned tables, not shared entities.

---

## Change Impact Considerations

| Change Type | Impact |
|----|----|
| LL88 rule updates | Isolated to LL88-owned tables |
| Property schema changes | Cross-subsystem coordination required |
| Survey attribute expansion | Evaluated for ownership before implementation |
| Reporting format changes | Confined to LL88 outputs |

This separation allows compliance logic to evolve without destabilizing core operational workflows.

---

## Architectural Rationale

The shared vs owned model supports:
- Modular evolution inside a monolithic platform
- Future extraction of compliance functionality if required
- Reduced operational risk under regulatory change
- Clear accountability for data correctness

This structure reflects lessons learned from operating a real-world, compliance-driven system under regulatory and operational constraints.

---
