# ISVF Control Catalog

This catalog enumerates the controls defined by the Idea Security Verification Framework. Controls are organized by family and mapped to ISVF mitigation IDs (as defined in [`crosswalks/mitre-atlas.md`](../crosswalks/mitre-atlas.md)).

> **Status:** Draft. Control definitions, testing procedures, and evidence requirements are provisional and subject to community refinement.

---

## Control Families

| Family Code | Family Name | MITRE Tactic |
|---|---|---|
| ISVF-GOV | Governance and Accountability | TA0001 |
| ISVF-DOM | Domain Modeling and Boundary Claims | TA0002 |
| ISVF-DATA | Data Classification and Secret Handling | TA0003 |
| ISVF-CTX | Context and Memory Management | TA0004 |
| ISVF-EXF | Exfiltration Controls | TA0005 |
| ISVF-INF | Domain Reach Prevention | TA0006 |
| ISVF-CLD | Cloud Prompting Governance | TA0007 |
| ISVF-ADV | Adversary Testing | TA0008 |
| ISVF-MON | Monitoring, Telemetry, and Drift Detection | TA0009 |
| ISVF-ASR | Assurance and Reporting | TA0010 |

---

## ISVF-GOV: Governance and Accountability

### ISVF-GOV-01 — Idea Security Owner

**Mitigation:** ISVF.M0001  
**Summary:** Appoint a single accountable owner for idea security boundaries, risk acceptance, and evidence readiness.

**Control Statement:** The organization SHALL designate a Idea Security Owner (CSO) with defined authority to approve domain boundary decisions, accept residual risk, and maintain evidence readiness across all in-scope LLM-enabled systems.

**Evidence Requirements:**
- Named individual in an organizational chart or RACI matrix
- Written role definition including escalation authority
- Acknowledgment of ownership (signed or recorded)

<!-- TODO: Define minimum qualifications for the Idea Security Owner role -->
<!-- TODO: Specify escalation paths when the CSO is unavailable or in conflict -->

---

### ISVF-GOV-02 — Idea Security Risk Register

**Mitigation:** ISVF.M0002  
**Summary:** Maintain a risk register that explicitly includes semantic leakage, unauthorized domain reach, and reachability risk, mapped to NIST AI RMF functions.

**Control Statement:** The organization SHALL maintain a idea security risk register updated on at least a [quarterly / annually — TODO: define cadence] basis and within [30 days — TODO: define SLA] of any material system change. The register SHALL explicitly include risks associated with semantic leakage, cross-domain inference, and reachability drift.

**Evidence Requirements:**
- Current risk register document with date of last review
- Change log showing updates after system changes
- Mapping to NIST AI RMF functions (GOVERN, MAP, MEASURE, MANAGE)

<!-- TODO: Define minimum risk register fields (risk ID, description, likelihood, impact, owner, mitigation, residual) -->
<!-- TODO: Define "material system change" triggering an update -->

---

### ISVF-GOV-03 — Export Control and Deemed Export Coverage

**Mitigation:** ISVF.M0003  
**Summary:** Incorporate export-controlled technical data and deemed export scenarios into idea security design.

**Control Statement:** For systems that may process export-controlled technical data, the organization SHALL conduct a deemed-export analysis, identify affected information domains, and apply domain boundary controls sufficient to prevent unauthorized disclosure to foreign nationals or model pathways that constitute a controlled release.

**Evidence Requirements:**
- Deemed-export analysis document
- Identification of export-controlled domains in the domain inventory
- Controls applied and tested for those domains

<!-- TODO: Define which export control regimes are in scope (EAR, ITAR, etc.) -->
<!-- TODO: Specify engagement with legal/compliance for deemed export determinations -->

---

## ISVF-DOM: Domain Modeling and Boundary Claims

### ISVF-DOM-01 — Domain Inventory and Join Matrix

**Mitigation:** ISVF.M0004  
**Summary:** Maintain a structured inventory of information domains and an explicit join matrix.

**Control Statement:** The organization SHALL maintain a Domain Inventory and Join Matrix for each in-scope LLM-enabled system. The matrix SHALL classify each potential join between domains as: (1) explicitly allowed, (2) prohibited, or (3) conditionally allowed with defined approval requirements.

**Evidence Requirements:**
- Current domain inventory (version-controlled)
- Join matrix with classification for each domain pair
- Documented rationale for all prohibited and conditionally allowed joins
- Review history

See template: [`examples/join-matrix-template.md`](../examples/join-matrix-template.md)

<!-- TODO: Define minimum fields for the domain inventory (domain name, description, data types, sensitivity classification, owner) -->
<!-- TODO: Define the approval workflow for conditionally allowed joins -->
<!-- TODO: Define review cadence for the join matrix -->

---

### ISVF-DOM-02 — Unreachable Statement Classes

**Mitigation:** ISVF.M0005  
**Summary:** Define classes of conclusions that must not become reachable for each system.

**Control Statement:** The organization SHALL define a catalog of Unreachable Statement Classes (USCs) for each in-scope system. Each USC SHALL specify the semantic category of conclusions that must remain unreachable, the associated information domains, the test method used to evaluate reachability, and the release-gate threshold.

**Evidence Requirements:**
- USC catalog (version-controlled)
- Test procedures for each USC category
- Most recent test results per USC
- Release-gate pass/fail records

See template: [`examples/unreachable-statement-classes-template.md`](../examples/unreachable-statement-classes-template.md)

<!-- TODO: Define USC severity tiers and corresponding release-gate thresholds -->
<!-- TODO: Define minimum fields for a USC entry -->
<!-- TODO: Specify how USCs are updated after incidents -->

---

### ISVF-DOM-03 — Boundary Enforcement Map

**Mitigation:** ISVF.M0006  
**Summary:** Document where idea security boundaries are enforced in the system architecture.

**Control Statement:** The organization SHALL maintain a Boundary Enforcement Map that identifies enforcement points across retrieval, context assembly, tool invocation, memory, and output handling. The map SHALL include architecture diagrams, enforcement code locations, and named owners for each enforcement point.

**Evidence Requirements:**
- Architecture diagram annotated with enforcement points
- Inventory of enforcement components with code/config references
- Named owners per enforcement point
- Date of last review

<!-- TODO: Define minimum required enforcement points for RAG systems vs. agentic systems vs. copilots -->
<!-- TODO: Define what "enforcement" means at each layer (technical vs. policy) -->

---

## ISVF-DATA: Data Classification and Secret Handling

### ISVF-DATA-01 — Secret Taxonomy and Labeling Policy

**Mitigation:** ISVF.M0007  
**Summary:** Establish a secret taxonomy with consistent labels usable by retrieval, validators, telemetry, and incident response.

**Control Statement:** The organization SHALL establish and maintain a secret taxonomy covering at minimum: regulated, privileged, trade secret, and export-controlled categories. Labels SHALL be applied consistently to raw data, embeddings, caches, and memory artifacts.

**Evidence Requirements:**
- Published taxonomy document
- Labeling policy with scope and enforcement rules
- Evidence that labels propagate to derived artifacts (embeddings, caches)

<!-- TODO: Define minimum taxonomy categories and sub-categories -->
<!-- TODO: Define requirements for label propagation in vector stores -->

---

### ISVF-DATA-02 — Pre-ingestion Classification and Redaction

**Mitigation:** ISVF.M0008  
**Summary:** Classify and redact before ingestion into AI-adjacent stores.

**Control Statement:** The organization SHALL require classification and redaction of data before it is ingested into vector stores, prompt caches, memory layers, fine-tuning corpora, or other AI-adjacent stores. Redaction logs SHALL be maintained for audit and incident response.

**Evidence Requirements:**
- Ingestion pipeline documentation showing classification/redaction gate
- Redaction log samples
- Evidence of bypass path controls (no unreviewed direct ingest)

<!-- TODO: Define acceptable automated vs. human classification methods -->
<!-- TODO: Define redaction log retention period -->

---

### ISVF-DATA-03 — Data Provenance and Lineage for AI Outputs

**Mitigation:** ISVF.M0009  
**Summary:** Record provenance for retrieved and generated content to support incident investigation and purge targeting.

**Control Statement:** The organization SHALL record provenance metadata for retrieved and generated content sufficient to trace outputs to their source documents, transformations, and model versions. Provenance records SHALL be retained for [define period — TODO] and made available for incident investigation.

**Evidence Requirements:**
- Provenance metadata schema
- Sample provenance records
- Demonstrated ability to trace an output to its sources in a tabletop exercise

<!-- TODO: Define minimum provenance fields -->
<!-- TODO: Define retention period aligned with regulatory requirements -->

---

## ISVF-CTX: Context and Memory Management

### ISVF-CTX-01 — Least-Privilege Retrieval

**Mitigation:** ISVF.M0010  
**Summary:** Enforce ABAC/RBAC so model context never expands beyond user and task authorization.

**Control Statement:** The organization SHALL enforce least-privilege retrieval using Attribute-Based Access Control (ABAC) or Role-Based Access Control (RBAC) so that retrieved content is bounded by user identity and task authorization. Cross-domain retrieval attempts SHALL generate telemetry events.

**Evidence Requirements:**
- ABAC/RBAC configuration documentation
- Telemetry showing blocked cross-domain retrievals
- Test results demonstrating that a user cannot retrieve beyond their authorization

<!-- TODO: Define "retrieval by proxy" attack pattern and test cases -->
<!-- TODO: Specify telemetry schema for blocked retrieval events -->

---

### ISVF-CTX-02 — Session Information Budgets

**Mitigation:** ISVF.M0011  
**Summary:** Cap how much sensitive material can enter one session context.

**Control Statement:** The organization SHALL define and enforce per-session information budgets that cap how much sensitive material — and which domain combinations — may enter a single context window during a session. Budget limits SHALL be documented in the join matrix and enforced technically.

**Evidence Requirements:**
- Budget policy document with limits by domain and session type
- Technical enforcement configuration
- Test results showing that budget limits are enforced

<!-- TODO: Define how budgets are calculated (token count, sensitivity score, domain count) -->
<!-- TODO: Define behavior when a budget limit is reached (block, redact, alert, or abort) -->

---

### ISVF-CTX-03 — Memory Scoping, Retention Windows, and Deletion

**Mitigation:** ISVF.M0012  
**Summary:** Scope memory by domain and enforce retention windows with deletion capability.

**Control Statement:** The organization SHALL scope agent and session memory by information domain and enforce retention windows. The system SHALL support deletion, rekeying, and separation of user memory from organizational memory. Sensitive content SHALL NOT become durable substrate that expands reachability beyond its authorized window.

**Evidence Requirements:**
- Memory architecture documentation
- Retention window policy
- Demonstrated deletion capability (test records)
- Separation evidence for user vs. organizational memory

<!-- TODO: Define default retention windows by domain sensitivity tier -->
<!-- TODO: Define memory deletion verification procedure -->

---

## ISVF-EXF: Exfiltration Controls

### ISVF-EXF-01 — Output Validation and Semantic Leakage Guardrails

**Mitigation:** ISVF.M0013, ISVF.M0026  
**Summary:** Validate outputs to block or redact protected information and inferred protected conclusions.

**Control Statement:** The organization SHALL implement output validators aligned to the secret taxonomy, including pattern-based, structured, and semantic detectors. Output validators SHALL block or redact: (1) direct protected information, and (2) high-confidence inferred protected conclusions as defined in the USC catalog.

**Evidence Requirements:**
- Output validator configuration and coverage documentation
- Test results showing detection of direct and semantic leakage
- LER trend data

<!-- TODO: Define minimum validator types required by domain sensitivity tier -->
<!-- TODO: Define acceptable false-positive and false-negative thresholds -->

---

### ISVF-EXF-02 — Canary and Honeytoken Instrumentation

**Mitigation:** ISVF.M0014  
**Summary:** Deploy canaries and honeytokens in high-value domains to detect and attribute leakage.

**Control Statement:** The organization SHALL deploy canaries and/or honeytokens in high-value information domains. Alerts SHALL be routed to defined owners and integrated into incident response runbooks.

**Evidence Requirements:**
- Canary/honeytoken deployment records
- Alert routing configuration
- Incident response runbook reference
- Test of alert trigger (tabletop or live test record)

<!-- TODO: Define minimum density of canary/honeytoken deployment per domain tier -->
<!-- TODO: Define "honey idea" pattern for abstract semantic honeytokens -->

---

### ISVF-EXF-03 — Revocation and Downstream Purge Playbooks

**Mitigation:** ISVF.M0015  
**Summary:** Maintain playbooks to revoke access and purge downstream stores after a leak.

**Control Statement:** The organization SHALL maintain documented playbooks for revoking access and purging downstream stores (vector databases, caches, prompt logs, memory layers) following discovery of a leak. Playbooks SHALL be validated through tabletop exercises at least [annually — TODO: define cadence].

**Evidence Requirements:**
- Published purge playbook
- Tabletop exercise records
- Time-to-completion records for purge actions

<!-- TODO: Define maximum acceptable time-to-purge SLA by domain tier -->
<!-- TODO: Define escalation requirements for purges involving vendor-controlled stores -->

---

## ISVF-INF: Domain Reach Prevention

### ISVF-INF-01 — High-Sensitivity Join Approval Workflow

**Mitigation:** ISVF.M0016  
**Summary:** Require explicit approval and documented rationale for joins between high-sensitivity domains.

**Control Statement:** The organization SHALL require documented approval and written rationale before enabling joins between domains classified as high-sensitivity in the join matrix. Approvals SHALL be enforced through change control and technical policy gates.

**Evidence Requirements:**
- Change control records showing join approvals
- Technical policy gate configuration
- Written rationale on file for each approved high-sensitivity join

<!-- TODO: Define what constitutes "high-sensitivity" for join approval purposes -->
<!-- TODO: Define approval authority (who can approve a high-sensitivity join) -->

---

### ISVF-INF-02 — Cross-Domain Inference Testing and DIR Metric

**Mitigation:** ISVF.M0017, ISVF.M0028  
**Summary:** Run cross-domain inference tests for each USC and maintain a DIR metric.

**Control Statement:** The organization SHALL run cross-domain inference tests for each USC in the catalog and maintain a Domain Inference Risk (DIR) metric. DIR SHALL be tracked over time and segmented by system boundary, model version, and prompt template. DIR trends SHALL be reviewed at each release gate and after material system changes.

**Evidence Requirements:**
- DIR test procedures per USC
- DIR trend data
- Release-gate review records

See: [`metrics/domain-inference-risk.md`](../metrics/domain-inference-risk.md)

<!-- TODO: Define minimum test set size per USC for DIR calculation -->
<!-- TODO: Define DIR threshold for release-gate failure -->

---

### ISVF-INF-03 — Prohibit LLM Authorization Decisioning

**Mitigation:** ISVF.M0018  
**Summary:** Do not allow LLMs to make authorization decisions.

**Control Statement:** The organization SHALL NOT allow LLM outputs to serve as the final authority for access control, authorization, or permission decisions. Authorization SHALL be deterministic, auditable, and enforced by a control plane external to the model.

**Evidence Requirements:**
- Architecture diagram showing authorization enforcement outside the model
- Code review or configuration evidence confirming no model-gated authorization paths
- Test demonstrating that a model refusal can be bypassed only through the external control plane

<!-- TODO: Define acceptable patterns for model-assisted authorization vs. prohibited model-gated authorization -->

---

## ISVF-CLD: Cloud Prompting Governance

### ISVF-CLD-01 — Consumer LLM Submission Restrictions

**Mitigation:** ISVF.M0019  
**Summary:** Prohibit submission of sensitive data to consumer LLMs without approved configuration and contracts.

**Control Statement:** The organization SHALL prohibit submission of regulated, privileged, export-controlled, classified, or proprietary content to consumer LLMs unless: (1) an approved enterprise configuration is in place, (2) contractual protections meeting organizational requirements are confirmed, and (3) the pathway is listed in the approved cloud pathways inventory.

**Evidence Requirements:**
- Published policy document
- Approved cloud pathways inventory
- Contract evidence for approved pathways
- Technical controls blocking unapproved pathways (e.g., DLP, network controls)

<!-- TODO: Define minimum contractual terms required for a pathway to be approved -->
<!-- TODO: Define enforcement mechanism (DLP policy, network block, MDM policy, etc.) -->

---

### ISVF-CLD-02 — Approved Enterprise and API Pathways with Vendor Controls

**Mitigation:** ISVF.M0020  
**Summary:** Define approved vendors and configurations with documented retention and data control settings.

**Control Statement:** The organization SHALL maintain an inventory of approved cloud LLM vendors and API configurations, including documented vendor data retention settings, data processing agreements, and configuration baselines. Configuration baselines SHALL be reviewed [annually — TODO: define cadence] and after vendor policy changes.

**Evidence Requirements:**
- Approved vendor inventory
- Data processing agreements on file
- Configuration baseline documents with review dates

<!-- TODO: Define minimum vendor due diligence requirements -->
<!-- TODO: Define process for responding to vendor policy changes -->

---

### ISVF-CLD-03 — Cloud Prompt Logging and DLP Hooks

**Mitigation:** ISVF.M0021  
**Summary:** Implement logging and DLP integration for cloud prompting where feasible.

**Control Statement:** Where technically feasible, the organization SHALL implement logging and DLP integration for cloud LLM prompting. Logged events SHALL include at minimum: prompt metadata, policy hits, model version, and timestamp. Logs SHALL be retained for [define period — TODO] and available for incident investigation.

**Evidence Requirements:**
- Logging configuration documentation
- Sample log records
- DLP rule documentation

<!-- TODO: Define minimum logging schema fields -->
<!-- TODO: Define log retention period -->
<!-- TODO: Define escalation when logging is not technically feasible for a given pathway -->

---

## ISVF-ADV: Adversary Testing

### ISVF-ADV-01 — OWASP LLM Suite Release Gate and Red-Team Testing

**Mitigation:** ISVF.M0022, ISVF.M0027  
**Summary:** Run OWASP LLM test suite and adversarial tests at release gates and after material changes.

**Control Statement:** The organization SHALL run the OWASP LLM test suite at release gates and after material changes. In addition, the organization SHALL conduct adversarial testing including: paraphrase attacks, multi-step prompting, retrieval chaining, tool use, and cross-session memory effects, at a cadence of no less than [annually — TODO: define] for each in-scope system.

**Evidence Requirements:**
- OWASP LLM test suite results per release
- Red-team test records including scenarios, findings, and remediations
- CRS metric results

See: [`metrics/crawl-resilience-score.md`](../metrics/crawl-resilience-score.md)

<!-- TODO: Define minimum adversarial test scenarios per system type (RAG, agentic, copilot) -->
<!-- TODO: Define release-gate pass/fail criteria for adversarial testing -->

---

## ISVF-MON: Monitoring, Telemetry, and Drift Detection

### ISVF-MON-01 — Standard Idea Security Event Schema

**Mitigation:** ISVF.M0023, ISVF.M0026  
**Summary:** Emit a standard event schema for security monitoring and investigation.

**Control Statement:** The organization SHALL configure in-scope systems to emit a standard idea security event schema that includes at minimum: prompts (or prompt hashes), retrieved document identifiers, tool calls, policy hits and misses, model version, and timestamp. Events SHALL be retained for [define period — TODO] and available for investigation.

**Evidence Requirements:**
- Event schema documentation
- Sample events from each system
- Evidence of event integrity controls (e.g., tamper-evident log)

<!-- TODO: Define minimum schema fields -->
<!-- TODO: Define sampling strategy for high-volume systems -->
<!-- TODO: Define log integrity requirements -->

---

### ISVF-MON-02 — Drift and Boundary Regression Detection

**Mitigation:** ISVF.M0024  
**Summary:** Monitor for behavior drift and boundary regressions after system changes.

**Control Statement:** The organization SHALL monitor for reachability drift and boundary regressions after changes to model version, prompts, retrieval configuration, policies, or tools. Changes SHALL trigger regression test runs for affected USCs. DIR trends SHALL be updated and reviewed.

**Evidence Requirements:**
- Change-triggered regression test records
- DIR trend data showing pre- and post-change values
- Risk register updates after detected drift

<!-- TODO: Define what triggers a mandatory regression test run -->
<!-- TODO: Define drift alert thresholds -->

---

## ISVF-ASR: Assurance and Reporting

### ISVF-ASR-01 — Evidence Pack Maintenance

**Mitigation:** ISVF.M0025, ISVF.M0026  
**Summary:** Maintain an evidence pack demonstrating control design, implementation, and operating effectiveness.

**Control Statement:** The organization SHALL maintain a current evidence pack for each in-scope LLM-enabled system. The evidence pack SHALL be updated at least [annually — TODO: define cadence] and within [30 days — TODO: define SLA] of any material system change or incident.

**Evidence Requirements:**
- Current evidence pack meeting the requirements defined in the framework document
- Version history
- Date of last review

See template: [`examples/evidence-pack-template.md`](../examples/evidence-pack-template.md)

<!-- TODO: Define minimum review cycle for evidence packs -->
<!-- TODO: Define who is authorized to certify an evidence pack -->

---

### ISVF-ASR-02 — SOC-Style Crosswalk and Procurement Readiness

**Mitigation:** ISVF.M0025  
**Summary:** Optionally produce a SOC-style report mapped to AICPA Trust Services Criteria for procurement.

**Control Statement:** The organization SHOULD produce, or be able to produce on request, a summary of idea security control design and operating effectiveness mapped to AICPA Trust Services Criteria (Security, Availability, Confidentiality) with a ISVF control crosswalk. This document may be used in procurement, vendor assessment, or regulatory engagement.

**Evidence Requirements:**
- SOC-style mapping document (if produced)
- Evidence of controls mapped in the document
- Reviewer acknowledgment

<!-- TODO: Define the mapping between ISVF controls and AICPA Trust Services Criteria -->
<!-- TODO: Define whether an independent auditor attestation is required or optional -->

---

<!-- TODO: Add sector-specific control variants (healthcare/HIPAA, financial/GLBA, government/FedRAMP) -->
<!-- TODO: Define testing procedures for each control (unit test, integration test, red team, etc.) -->
<!-- TODO: Assign maturity levels (Level 1: policy, Level 2: implemented, Level 3: tested, Level 4: monitored) -->
<!-- TODO: Define control inheritance patterns for multi-system or platform environments -->

---

*See also:*
- *[`crosswalks/mitre-atlas.md`](../crosswalks/mitre-atlas.md) — ATLAS mitigation objects with ISVF crosswalk IDs*
- *[`crosswalks/nist-ai-rmf.md`](../crosswalks/nist-ai-rmf.md) — NIST RMF function mapping*
- *[`metrics/`](../metrics/) — Draft metric definitions*
- *[`examples/`](../examples/) — Templates for key artifacts*
