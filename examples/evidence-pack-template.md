# Evidence Pack Template

This template provides a starting structure for the ISVF Evidence Pack: the single evidentiary record that a system's cognitive boundaries are defined, enforced, tested, and monitored over time. Complete this document as part of ISVF control **ISVF-ASR-01**.

> The evidence pack is the primary artifact for audit, procurement review, and risk acceptance decisions. It should be updated at least annually and within 30 days of any material system change or incident.

---

## System Identification

| Field | Value |
|---|---|
| System Name | `[System name]` |
| System Description | `[Brief description of what the system does]` |
| System Owner | `[Name / role]` |
| Idea Security Owner | `[Name / role]` |
| Evidence Pack Version | `[e.g., 1.0]` |
| Evidence Pack Date | `[YYYY-MM-DD]` |
| Review Cycle | `[e.g., Annual + after material changes]` |
| Next Review Due | `[YYYY-MM-DD]` |
| Prepared By | `[Name / role]` |

---

## Section 1: System Boundary Description and Architecture

### 1.1 System Boundary Description

`[Provide a plain-language description of the system boundary: what the system does, what information it can access, and where it is deployed (cloud, on-premises, hybrid).]`

### 1.2 Architecture Diagram

`[Attach or link to the current architecture diagram. The diagram should show: information domains, retrieval paths, tool connections, memory layers, output paths, and enforcement points.]`

**Attachment:** `[Link or filename]`  
**Diagram Date:** `[YYYY-MM-DD]`

---

## Section 2: Domain Inventory and Join Matrix

### 2.1 Domain Inventory

**Document:** `[Link to completed domain-inventory-template.md for this system]`  
**Version:** `[Version number]`  
**Last Reviewed:** `[YYYY-MM-DD]`

**Summary:** `[Number of domains, sensitivity tier distribution]`

### 2.2 Join Matrix

**Document:** `[Link to completed join-matrix-template.md for this system]`  
**Version:** `[Version number]`  
**Last Reviewed:** `[YYYY-MM-DD]`

**Summary:** `[Number of joins: X allowed, Y prohibited, Z conditional]`

<!-- TODO: Define a summary format for the domain/join statistics -->

---

## Section 3: Unreachable Statement Class Catalog

**Document:** `[Link to completed unreachable-statement-classes-template.md for this system]`  
**Version:** `[Version number]`  
**Last Reviewed:** `[YYYY-MM-DD]`

**Summary:**

| USC Tier | Count |
|---|---|
| Critical (Tier 1) | `[n]` |
| High (Tier 2) | `[n]` |
| Medium (Tier 3) | `[n]` |
| Low (Tier 4) | `[n]` |

---

## Section 4: Boundary Enforcement Map

**Document:** `[Link to boundary enforcement map / architecture diagram with enforcement annotations]`  
**Version:** `[Version number]`  
**Last Reviewed:** `[YYYY-MM-DD]`

### 4.1 Enforcement Points Summary

| Layer | Enforcement Mechanism | Owner | Last Verified |
|---|---|---|---|
| Retrieval | `[e.g., ABAC policy in vector store]` | `[Owner]` | `[YYYY-MM-DD]` |
| Context Assembly | `[e.g., context filter middleware]` | `[Owner]` | `[YYYY-MM-DD]` |
| Tool Invocation | `[e.g., tool permission manifest]` | `[Owner]` | `[YYYY-MM-DD]` |
| Memory | `[e.g., memory scoping policy]` | `[Owner]` | `[YYYY-MM-DD]` |
| Output | `[e.g., output validator service]` | `[Owner]` | `[YYYY-MM-DD]` |

<!-- TODO: Define minimum required enforcement point documentation for each system type -->

---

## Section 5: Pre-Ingestion Classification and Redaction Evidence

**Summary:** `[Describe how data is classified and redacted before entering AI-adjacent stores.]`

| Evidence Item | Location / Reference | Date |
|---|---|---|
| Ingestion pipeline documentation | `[Link]` | `[YYYY-MM-DD]` |
| Redaction log samples | `[Link]` | `[YYYY-MM-DD]` |
| Classification policy | `[Link]` | `[YYYY-MM-DD]` |

---

## Section 6: Output Validation, Canary, and Purge Controls

### 6.1 Output Validation

**Configuration:** `[Link to output validator configuration]`  
**Coverage:** `[Which domains and USC categories are covered by validators?]`

### 6.2 Canary and Honeytoken Deployment

**Deployment Record:** `[Link]`  
**Alert Routing:** `[Link or description]`  
**Last Test of Alert Trigger:** `[YYYY-MM-DD]`

### 6.3 Purge Playbooks

**Playbook:** `[Link to purge playbook]`  
**Last Tabletop Exercise:** `[YYYY-MM-DD]`  
**Time-to-Purge Target:** `[Define SLA — TODO]`

---

## Section 7: Join Approvals and Cross-Domain Inference Test Results

### 7.1 Active High-Sensitivity Join Approvals

| Join | Approval Date | Approver | Review Due |
|---|---|---|---|
| `[DOM-A × DOM-B]` | `[YYYY-MM-DD]` | `[Name]` | `[YYYY-MM-DD]` |

### 7.2 DIR Test Results Summary

**Most Recent DIR Test Date:** `[YYYY-MM-DD]`  
**Model Version Tested:** `[Version]`

| USC ID | USC Name | Tier | DIR | Pass/Fail |
|---|---|---|---|---|
| USC-001 | `[Name]` | `[1–4]` | `[X%]` | `[Pass/Fail]` |

---

## Section 8: Authorization Enforcement Outside the Model

**Summary:** `[Describe how authorization is enforced outside the model — what control plane exists and how it works.]`

| Evidence Item | Location / Reference | Date |
|---|---|---|
| Authorization architecture description | `[Link]` | `[YYYY-MM-DD]` |
| Code review / config evidence | `[Link]` | `[YYYY-MM-DD]` |
| Test demonstrating model cannot bypass authorization | `[Link]` | `[YYYY-MM-DD]` |

---

## Section 9: Release-Gate Adversarial Testing Records

| Test Type | Test Date | Tester / Team | Findings Summary | Remediation Status |
|---|---|---|---|---|
| OWASP LLM Test Suite | `[YYYY-MM-DD]` | `[Tester]` | `[Summary]` | `[Open/Closed]` |
| Red-team adversarial | `[YYYY-MM-DD]` | `[Tester]` | `[Summary]` | `[Open/Closed]` |
| CRS multi-session extraction | `[YYYY-MM-DD]` | `[Tester]` | `[CRS: X]` | `[Open/Closed]` |

---

## Section 10: Telemetry and Drift Monitoring

**Event Schema:** `[Link to event schema documentation]`  
**Log Retention Period:** `[Define — TODO]`  
**LER Trend:** `[Link to LER trend chart or data]`  
**Last Drift Review:** `[YYYY-MM-DD]`  
**Reachability Drift Events:** `[None / count and summary]`

---

## Section 11: Change Log (System Changes)

| Date | Change Description | Change Author | Regression Tests Run | DIR Delta |
|---|---|---|---|---|
| `[YYYY-MM-DD]` | `[e.g., model version updated]` | `[Author]` | `[Yes/No]` | `[±X%]` |

---

## Section 12: Incident Records and Remediation Status

| Incident ID | Date | Description | Severity | USC Violated | Status | Remediation Summary |
|---|---|---|---|---|---|---|
| INC-001 | `[YYYY-MM-DD]` | `[Description]` | `[1–4]` | `[USC-ID or None]` | `[Open/Closed]` | `[Summary]` |

---

## Section 13: Vendor-Control Artifacts for Approved Cloud Pathways

| Vendor | Pathway / Product | DPA Reference | Retention Settings | Config Baseline | Last Reviewed |
|---|---|---|---|---|---|
| `[Vendor name]` | `[Product/API]` | `[Link]` | `[Summary]` | `[Link]` | `[YYYY-MM-DD]` |

---

## Section 14: Risk Acceptances

Document explicit risk acceptances for high-sensitivity joins, residual exposures, or accepted DIR thresholds that exceed baseline policy.

| Risk Item | Description | Accepted By | Acceptance Date | Review Date | Rationale |
|---|---|---|---|---|---|
| RA-001 | `[Description]` | `[Name / role]` | `[YYYY-MM-DD]` | `[YYYY-MM-DD]` | `[Rationale]` |

---

## Certification

> By signing this evidence pack, the Idea Security Owner certifies that the information above accurately reflects the current state of the system's cognitive security controls, that known gaps are documented under risk acceptances, and that the evidence pack will be updated as defined above.

| Role | Name | Signature | Date |
|---|---|---|---|
| Idea Security Owner | | | |
| System Owner | | | |

---

<!-- TODO: Define whether an independent reviewer or auditor signature is required -->
<!-- TODO: Define a summary page format for procurement / executive audiences -->
<!-- TODO: Develop a SOC-style crosswalk mapping evidence pack sections to AICPA Trust Services Criteria -->
<!-- TODO: Define how the evidence pack relates to SOC 2 Type II audit artifacts -->
<!-- TODO: Add sector-specific sections (HIPAA, FedRAMP, etc.) as annexes -->

---

*See also:*
- *[`examples/domain-inventory-template.md`](domain-inventory-template.md)*
- *[`examples/join-matrix-template.md`](join-matrix-template.md)*
- *[`examples/unreachable-statement-classes-template.md`](unreachable-statement-classes-template.md)*
- *[`controls/control-catalog.md#csvf-asr-01`](../controls/control-catalog.md)*
