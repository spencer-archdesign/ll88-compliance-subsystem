# Tables in Scope – LL88 Compliance Subsystem

This document defines the **data entities in scope** for the LL88 Compliance Subsystem and clarifies how they relate to the broader Operations Platform.

The intent is to:
- Make subsystem boundaries explicit
- Avoid duplication of canonical entities
- Clarify ownership and dependency relationships
- Support future refactoring or subsystem extraction

This is not a full schema export; it is a **conceptual scope definition**.

---

## LL88-Owned Tables (In Scope)

These tables are **fully owned** by the LL88 Compliance Subsystem and contain compliance-specific logic, interpretation, and outputs.

### Core Compliance Entities

| Table | Description |
|----|----|
| `LL88_Compliance_Record` | Primary compliance state for a property or project |
| `LL88_Lighting_Inventory` | Lighting inventory normalized for LL88 analysis |
| `LL88_Rule_Evaluation` | Results of rule execution, exceptions, and overrides |
| `LL88_Compliance_Summary` | Aggregated compliance outcomes by area/building |
| `LL88_Report_Snapshot` | Immutable record of generated compliance documents |

### Supporting / Control Entities

| Table | Description |
|----|----|
| `LL88_Audit_Log` | Compliance-related change history |
| `LL88_Interpretation_Notes` | Human judgment applied to ambiguous conditions |
| `LL88_Status_History` | Time-based tracking of compliance state changes |

**Scope Rationale:**  
These entities exist solely to support LL88 compliance workflows and have no meaning outside the subsystem.

---

## Shared Platform Tables (Referenced)

These tables are **owned by the core Operations Platform** and referenced by LL88 to maintain a single source of truth.

| Table | Role in LL88 |
|----|----|
| `Properties` | Physical and legal context for compliance |
| `Projects` | Operational work derived from compliance findings |
| `Clients` | Ownership and regulatory responsibility |
| `Areas / Units` | Granularity for survey and compliance evaluation |
| `Invoices` | Financial linkage to compliance-driven work |

**Usage Rules:**
- LL88 does not redefine these entities
- Compliance-specific attributes are stored in LL88-owned tables
- Relationships are preferred over duplication

---

## Shared Reference / Lookup Tables

These tables provide **standardized classification data** and are treated as read-only by the LL88 subsystem.

| Table | Purpose |
|----|----|
| `Lighting_Types` | Fixture categorization |
| `Control_Types` | Sensor and control strategies |
| `Mounting_Types` | Physical installation context |
| `Area_Types` | Hallways, stairwells, garages, etc. |
| `Wattage_Ranges` | Validation and normalization |

These tables ensure consistent interpretation across compliance, operations, and financial workflows.

---

## Key Relationships (Conceptual)

- `Properties` → `LL88_Compliance_Record` (1-to-many over time)
- `Areas / Units` → `LL88_Lighting_Inventory`
- `LL88_Lighting_Inventory` → `LL88_Rule_Evaluation`
- `LL88_Rule_Evaluation` → `LL88_Compliance_Summary`
- `LL88_Compliance_Record` → `Projects` (conditional, compliance-driven)

Relationships are intentionally **explicit and traceable** to support audits and re-evaluation.

---

## Excluded from Scope

The following platform areas are intentionally **out of scope** for LL88:

- General accounting logic
- Purchasing workflows
- Vendor management
- Non-compliance-related project tracking

This prevents regulatory logic from leaking into unrelated operational domains.

---

## Architectural Intent

The scoped table model supports:
- Modular evolution of compliance logic
- Safe iteration under regulatory change
- Clear ownership and accountability
- Potential future extraction of the subsystem

This structure reflects real-world operation of a compliance-driven system embedded within a broader operations platform.

---
