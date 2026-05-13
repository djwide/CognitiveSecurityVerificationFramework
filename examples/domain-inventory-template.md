# Domain Inventory Template

This template provides a starting structure for documenting the information domains relevant to an LLM-enabled system. Complete this document as part of CSVF control **CSVF-DOM-01** (Domain Inventory and Join Matrix).

> The completed domain inventory should be version-controlled, reviewed on a defined cadence, and updated after material system changes.

---

## System Identification

| Field | Value |
|---|---|
| System Name | `[System name]` |
| System Owner | `[Name / role]` |
| Cognitive Security Owner | `[Name / role]` |
| Document Version | `[e.g., 1.0]` |
| Last Reviewed | `[YYYY-MM-DD]` |
| Next Review Due | `[YYYY-MM-DD]` |

---

## Domain Inventory

For each information domain accessible to or processable by this system, complete the following fields.

### Domain Entry Format

| Field | Description |
|---|---|
| Domain ID | Unique identifier (e.g., DOM-001) |
| Domain Name | Short human-readable name (e.g., "HR Records") |
| Description | What information this domain contains |
| Data Types | Examples of data types present |
| Sensitivity Classification | See tiers below |
| Regulatory or Legal Basis | Applicable frameworks (HIPAA, FERPA, GLBA, ITAR, etc.) |
| Domain Owner | Named individual or team |
| Ingestion Method | How data enters the system (RAG, fine-tune, system prompt, tool, memory, etc.) |
| Notes | Any additional context |

### Sensitivity Classification Tiers

| Tier | Label | Examples |
|---|---|---|
| 1 | Public | Published marketing, publicly available documentation |
| 2 | Internal | General business communications, non-sensitive operational data |
| 3 | Confidential | Customer records, financial data, internal strategy |
| 4 | Restricted | Legal privilege, regulated data (HIPAA, FERPA, GLBA), trade secrets |
| 5 | Critical | Export-controlled, classified-adjacent, highest-value IP |

---

## Domain Register

<!-- TODO: Replace with actual domains for your system. Add rows as needed. -->

| Domain ID | Domain Name | Description | Data Types | Sensitivity Tier | Regulatory Basis | Domain Owner | Ingestion Method | Notes |
|---|---|---|---|---|---|---|---|---|
| DOM-001 | `[e.g., Customer Support]` | `[Description]` | `[e.g., support tickets, chat logs]` | `[1–5]` | `[e.g., CCPA]` | `[Owner]` | `[e.g., RAG]` | |
| DOM-002 | | | | | | | | |
| DOM-003 | | | | | | | | |

---

## Change Log

| Version | Date | Author | Summary of Changes |
|---|---|---|---|
| 1.0 | `[YYYY-MM-DD]` | `[Author]` | Initial draft |

---

<!-- TODO: Define the review and approval process for adding new domains -->
<!-- TODO: Define the process for removing or archiving a domain when it is no longer in scope -->
<!-- TODO: Define how domain sensitivity tiers interact with access control (ABAC/RBAC) -->
<!-- TODO: Link each domain to its corresponding rows in the join matrix -->

---

*See also: [`examples/join-matrix-template.md`](join-matrix-template.md)*  
*Related control: [`controls/control-catalog.md#csvf-dom-01`](../controls/control-catalog.md)*
