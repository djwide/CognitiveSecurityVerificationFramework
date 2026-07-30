# Join Matrix Template

This template provides a starting structure for documenting the permitted, prohibited, and conditionally allowed joins between information domains in an LLM-enabled system. Complete this document as part of ISVF control **ISVF-DOM-01** (Domain Inventory and Join Matrix).

A **join** is any combination of information across domains — whether through retrieval, tool use, context assembly, memory writes, or output synthesis.

> The completed join matrix should be version-controlled, reviewed on a defined cadence, and updated after material system changes or new domain additions.

---

## System Identification

| Field | Value |
|---|---|
| System Name | `[System name]` |
| System Owner | `[Name / role]` |
| Idea Security Owner | `[Name / role]` |
| Document Version | `[e.g., 1.0]` |
| Last Reviewed | `[YYYY-MM-DD]` |
| Next Review Due | `[YYYY-MM-DD]` |

---

## Join Classification Definitions

| Classification | Symbol | Meaning |
|---|---|---|
| Allowed | ✅ | This join is explicitly permitted under current policy. |
| Prohibited | ❌ | This join is explicitly prohibited. The system must not combine these domains. |
| Conditional | ⚠️ | This join is allowed only with elevated approval, additional controls, or specific conditions (see Notes). |
| Not Applicable | — | These domains do not interact and this combination is out of scope. |

---

## Join Matrix

List domain IDs from the [Domain Inventory](domain-inventory-template.md) as both row and column headers. Fill each cell with the join classification.

<!-- TODO: Expand this matrix with your actual domain IDs from the domain inventory. -->
<!-- TODO: Add a rationale/notes column or footnotes for all Conditional and Prohibited entries. -->

|  | DOM-001 | DOM-002 | DOM-003 | DOM-004 | DOM-005 |
|---|---|---|---|---|---|
| **DOM-001** | — | `[✅/❌/⚠️]` | `[✅/❌/⚠️]` | `[✅/❌/⚠️]` | `[✅/❌/⚠️]` |
| **DOM-002** | `[✅/❌/⚠️]` | — | `[✅/❌/⚠️]` | `[✅/❌/⚠️]` | `[✅/❌/⚠️]` |
| **DOM-003** | `[✅/❌/⚠️]` | `[✅/❌/⚠️]` | — | `[✅/❌/⚠️]` | `[✅/❌/⚠️]` |
| **DOM-004** | `[✅/❌/⚠️]` | `[✅/❌/⚠️]` | `[✅/❌/⚠️]` | — | `[✅/❌/⚠️]` |
| **DOM-005** | `[✅/❌/⚠️]` | `[✅/❌/⚠️]` | `[✅/❌/⚠️]` | `[✅/❌/⚠️]` | — |

---

## Join Notes Register

For all Conditional (⚠️) and Prohibited (❌) entries, document the rationale and any conditions that apply.

<!-- TODO: Add an entry for each Conditional or Prohibited join in the matrix above. -->

| Join (Domain A × Domain B) | Classification | Rationale | Conditions / Approval Required | Approver | Date Approved |
|---|---|---|---|---|---|
| DOM-001 × DOM-004 | ⚠️ | `[Why this join requires approval]` | `[Conditions or approval workflow]` | `[Named approver]` | `[YYYY-MM-DD]` |
| DOM-002 × DOM-005 | ❌ | `[Why this join is prohibited]` | N/A | N/A | N/A |

---

## High-Sensitivity Join Log

Log all joins that were conditionally approved, including the written rationale, approver, and any additional controls applied.

<!-- TODO: Populate this log whenever a conditional join is approved. -->

| Entry | Join | Approval Date | Approver | Rationale Summary | Additional Controls | Review Date |
|---|---|---|---|---|---|---|
| 001 | DOM-001 × DOM-004 | `[YYYY-MM-DD]` | `[Name]` | `[Summary]` | `[Controls applied]` | `[YYYY-MM-DD]` |

---

## Change Log

| Version | Date | Author | Summary of Changes |
|---|---|---|---|
| 1.0 | `[YYYY-MM-DD]` | `[Author]` | Initial draft |

---

<!-- TODO: Define who has authority to change a join classification from Allowed to Conditional or from Conditional to Prohibited -->
<!-- TODO: Define the process for adding a new domain to the matrix (review cycle, approval required?) -->
<!-- TODO: Define how the join matrix is enforced technically (retrieval policy, ABAC, context assembly controls) -->
<!-- TODO: Define the review cycle — at minimum annually and after any new domain is added or any system change -->
<!-- TODO: Link prohibited and conditional joins to corresponding USC entries -->

---

*See also: [`examples/domain-inventory-template.md`](domain-inventory-template.md) | [`examples/unreachable-statement-classes-template.md`](unreachable-statement-classes-template.md)*  
*Related control: [`controls/control-catalog.md#isvf-dom-01`](../controls/control-catalog.md)*
