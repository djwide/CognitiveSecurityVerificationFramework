# CSVF × MITRE ATLAS Crosswalk

This document describes how CSVF concepts fit into the MITRE ATLAS (Adversarial Threat Landscape for Artificial-Intelligence Systems) knowledge base, and proposes concrete contributions in the form of new or expanded mitigation entries, tactic categories, and measurement objects.

---

## MITRE ATLAS Positioning

MITRE ATLAS is a globally accessible, living knowledge base of adversary tactics and techniques against AI-enabled systems, modeled in the spirit of ATT&CK but focused on AI and machine learning. It is based on real-world attack observations, case studies, and demonstrations from red teams and security researchers. Its purpose is to help defenders, researchers, and system owners understand how AI systems are attacked, organize those attacks into a usable threat model, and connect those threats to mitigations and case studies.

ATLAS is strongest as an **adversary-informed threat framework**. It is very good at answering questions like: what attack technique is being used, where in the AI lifecycle it appears, what related case studies exist, and what mitigations are commonly associated with it.

### How CSVF Fits

CSVF fits into ATLAS because the two projects solve adjacent but different problems:

- **ATLAS** tells you how attackers target AI systems.
- **CSVF** tells you what a defended organization should define, test, measure, and prove in response, especially for semantic leakage, cross-domain inference, and reachability risk.

In other words, ATLAS is **threat realism**; CSVF is the **boundary and assurance layer** that translates those threats into domain models, permitted joins, Unreachable Statement Classes, measurement requirements, and evidence artifacts.

CSVF would fit naturally into MITRE ATLAS as a mitigation and operationalization layer rather than as a competing framework. ATLAS already supports mitigations and case studies in its data model, so CSVF can be expressed in an ATLAS-native way as mitigation objects.

### Entry Forms

In practical terms, CSVF could enter the ATLAS ecosystem in four forms:

1. New or expanded **mitigation entries** for reachability-oriented defenses.
2. **Case study annotations** showing how particular ATLAS techniques resulted in semantic leakage or unauthorized domain reach.
3. **Navigator or matrix overlays** that map ATLAS techniques to CSVF control families and evidence expectations.
4. A **defender profile** that complements ATLAS's adversary view with a standard of care for LLM-enabled systems.

---

## CSVF Defender Tactic Categories

These are the proposed "columns" for a CSVF mitigation matrix, analogous to how ATLAS groups items for navigation.

| ID | Tactic (Defender Objective) |
|---|---|
| CSVF.TA0001 | Governance and Accountability |
| CSVF.TA0002 | Domain Modeling and Boundary Claims |
| CSVF.TA0003 | Data Classification and Secret Handling |
| CSVF.TA0004 | Context and Memory Management |
| CSVF.TA0005 | Exfiltration Controls |
| CSVF.TA0006 | Domain Reach Prevention |
| CSVF.TA0007 | Cloud Prompting Governance |
| CSVF.TA0008 | Adversary Testing |
| CSVF.TA0009 | Monitoring, Telemetry, and Drift Detection |
| CSVF.TA0010 | Assurance and Reporting |

---

## CSVF Mitigations (ATLAS-Style Objects)

### CSVF.M0001 — Cognitive Security Owner

**Tactic:** CSVF.TA0001 Governance and Accountability  
**CSVF Crosswalk:** CSVF-GOV-01

Appoint a single accountable owner for cognitive security boundaries, risk acceptance, and evidence readiness across LLM-enabled systems. This role coordinates security, product, legal, and compliance decisions and owns escalation paths for boundary violations.

---

### CSVF.M0002 — Cognitive Security Risk Register (AI RMF Mapped)

**Tactic:** CSVF.TA0001 Governance and Accountability  
**CSVF Crosswalk:** CSVF-GOV-02

Maintain a cognitive security risk register mapped to NIST AI RMF functions (Govern, Map, Measure, Manage) and updated on a defined cadence and after material system changes. Track risks tied to reachability, semantic leakage, and cross-domain inference.

---

### CSVF.M0003 — Export Control and Deemed Export Coverage

**Tactic:** CSVF.TA0001 Governance and Accountability  
**CSVF Crosswalk:** CSVF-GOV-03

Incorporate export-controlled technical data and deemed export scenarios into cognitive security design. Ensure domain boundaries and access controls prevent inappropriate disclosure to foreign nationals and prevent model exposure pathways that can constitute a controlled release.

---

### CSVF.M0004 — Domain Inventory and Join Matrix

**Tactic:** CSVF.TA0002 Domain Modeling and Boundary Claims  
**CSVF Crosswalk:** CSVF-DOM-01

Maintain an inventory of information domains and an explicit permitted-joins matrix for each in-scope system. A "join" includes retrieval, context assembly, tool invocation, memory writes, and outputs that combine information across domains.

---

### CSVF.M0005 — Unreachable Statement Classes (USCs)

**Tactic:** CSVF.TA0002 Domain Modeling and Boundary Claims  
**CSVF Crosswalk:** CSVF-DOM-02

Define classes of statements that must not become reachable for each system boundary. USCs are used to drive tests, policy gates, and assurance claims.

---

### CSVF.M0006 — Boundary Enforcement Map (CSB Map)

**Tactic:** CSVF.TA0002 Domain Modeling and Boundary Claims  
**CSVF Crosswalk:** CSVF-DOM-03

Document the cognitive security boundary (CSB) enforcement points: retrieval gates, context filters, tool authorization, memory policies, and output validators. Include architecture diagrams and identify enforcement code locations and owners.

---

### CSVF.M0007 — Secret Taxonomy and Labeling Policy

**Tactic:** CSVF.TA0003 Data Classification and Secret Handling  
**CSVF Crosswalk:** CSVF-DATA-01

Establish a secret taxonomy (regulated, privileged, trade secret, export-controlled, classified) and require consistent labels that can be used by retrieval policy, validators, telemetry, and incident response.

---

### CSVF.M0008 — Pre-ingestion Classification and Redaction

**Tactic:** CSVF.TA0003 Data Classification and Secret Handling  
**CSVF Crosswalk:** CSVF-DATA-02

Require classification and redaction before data is ingested into vector stores, prompt caches, memory, fine-tunes, or other AI-adjacent stores. Prevent bypass paths and maintain redaction logs for audit and incident response.

---

### CSVF.M0009 — Data Provenance and Lineage for AI Outputs

**Tactic:** CSVF.TA0003 Data Classification and Secret Handling  
**CSVF Crosswalk:** CSVF-DATA-03

Record provenance for retrieved and generated content sufficient to trace outputs to sources and transformations. Enable incident investigation, purge targeting, and assurance evidence.

---

### CSVF.M0010 — Least-Privilege Retrieval (ABAC/RBAC Enforced)

**Tactic:** CSVF.TA0004 Context and Memory Management  
**CSVF Crosswalk:** CSVF-CTX-01

Enforce least-privilege retrieval using ABAC/RBAC so the model context never expands beyond user and task authorization. Prevent "retrieval by proxy" and block cross-domain retrieval attempts with explicit telemetry.

---

### CSVF.M0011 — Session Information Budgets (Sensitivity Caps)

**Tactic:** CSVF.TA0004 Context and Memory Management  
**CSVF Crosswalk:** CSVF-CTX-02

Enforce per-session information budgets that cap how much sensitive material can enter context. Budgets constrain risk from long-context assembly and iterative extraction workflows even when each single retrieval appears permissible.

---

### CSVF.M0012 — Memory Scoping, Retention Windows, and Deletion

**Tactic:** CSVF.TA0004 Context and Memory Management  
**CSVF Crosswalk:** CSVF-CTX-03

Scope memory by domain and enforce retention windows. Support deletion, rekeying, and separation of user memory from organizational memory. Prevent sensitive content from becoming durable substrate that expands reachability.

---

### CSVF.M0013 — Output Validation and Semantic Leakage Guardrails

**Tactic:** CSVF.TA0005 Exfiltration Controls  
**CSVF Crosswalk:** CSVF-EXF-01

Implement output validators aligned to the secret taxonomy, including pattern-based, structured, and semantic detectors. Block or redact protected information and high-confidence inferred protected conclusions.

---

### CSVF.M0014 — Canary and Honeytoken Instrumentation

**Tactic:** CSVF.TA0005 Exfiltration Controls  
**CSVF Crosswalk:** CSVF-EXF-02

Deploy canaries and honeytokens in high-value domains to detect leakage and misuse. Route alerts to defined owners and integrate into incident response runbooks and post-incident evidence.

---

### CSVF.M0015 — Revocation and Downstream Purge Playbooks

**Tactic:** CSVF.TA0005 Exfiltration Controls  
**CSVF Crosswalk:** CSVF-EXF-03

Maintain playbooks to revoke access and purge downstream stores (vector DBs, caches, prompt logs where feasible). Conduct tabletop exercises and verify purge actions and time-to-completion.

---

### CSVF.M0016 — High-Sensitivity Join Approval Workflow

**Tactic:** CSVF.TA0006 Domain Reach Prevention  
**CSVF Crosswalk:** CSVF-INF-01

Require explicit approval and documented rationale for joins between high-sensitivity domains. Enforce approvals through change control and technical policy gates, not by policy text alone.

---

### CSVF.M0017 — Cross-Domain Inference Testing and DIR Metric

**Tactic:** CSVF.TA0006 Domain Reach Prevention  
**CSVF Crosswalk:** CSVF-INF-02

Run cross-domain inference tests for each USC and boundary and maintain a Domain Inference Risk (DIR) metric.

**Reachable definition (operational):** A statement class is reachable if, under defined system conditions (identity, allowed tools, retrieval policy, prompts, context limits), the system can reliably produce outputs that satisfy the USC description at a repeatable success rate. DIR tracks how often the system reaches those outcomes and how that risk changes over time.

---

### CSVF.M0018 — Prohibit LLM Authorization Decisioning

**Tactic:** CSVF.TA0006 Domain Reach Prevention  
**CSVF Crosswalk:** CSVF-INF-03

Do not allow LLMs to make authorization decisions. Authorization must be deterministic, auditable, and enforced outside the model, with the model operating only within the permissions granted by that layer.

---

### CSVF.M0019 — Consumer LLM Submission Restrictions

**Tactic:** CSVF.TA0007 Cloud Prompting Governance  
**CSVF Crosswalk:** CSVF-CLD-01

Prohibit submission of regulated, privileged, export-controlled, classified, or proprietary content to consumer LLMs unless an approved configuration and contractual protections are in place.

---

### CSVF.M0020 — Approved Enterprise and API Pathways with Vendor Controls

**Tactic:** CSVF.TA0007 Cloud Prompting Governance  
**CSVF Crosswalk:** CSVF-CLD-02

Define approved vendors and configurations aligned to vendor data controls and retention settings. Maintain configuration baselines and contract evidence suitable for procurement and assurance review.

---

### CSVF.M0021 — Cloud Prompt Logging and DLP Hooks

**Tactic:** CSVF.TA0007 Cloud Prompting Governance  
**CSVF Crosswalk:** CSVF-CLD-03, CSVF-MON-01

Where feasible, implement logging and DLP integration for cloud prompting. ATLAS lists "AI Telemetry Logging" as a mitigation; CSVF specifies the event schema and evidence expectations for audits and DIR trend analysis.

---

### CSVF.M0022 — OWASP LLM Suite Release Gate

**Tactic:** CSVF.TA0008 Adversary Testing  
**CSVF Crosswalk:** CSVF-ADV-01

Run the required OWASP LLM test suite at release gates and after material changes. Track failures, remediation, and regression outcomes as evidence of operating effectiveness.

---

### CSVF.M0023 — Standard Cognitive Security Event Schema

**Tactic:** CSVF.TA0009 Monitoring, Telemetry, and Drift Detection  
**CSVF Crosswalk:** CSVF-MON-01

Emit a standard event schema including prompts, retrievals, tool calls, policy hits and misses, and model or version changes. Support sampling, integrity controls, and investigation workflows.

---

### CSVF.M0024 — Drift and Boundary Regression Detection

**Tactic:** CSVF.TA0009 Monitoring, Telemetry, and Drift Detection  
**CSVF Crosswalk:** CSVF-MON-02

Monitor for behavior drift and boundary regressions after changes to model, prompts, retrieval configuration, policies, or tools. Trigger regression test runs and update DIR trends and risk register entries.

---

### CSVF.M0025 — Evidence Pack and SOC-Style Crosswalk

**Tactic:** CSVF.TA0010 Assurance and Reporting  
**CSVF Crosswalk:** CSVF-ASR-01, CSVF-ASR-02

Maintain an evidence pack that demonstrates control design, implementation, and operating effectiveness. Optionally produce a SOC-style report mapped to AICPA Trust Services Criteria with a CSVF crosswalk for procurement readiness.

---

### CSVF.M0026 — Leakage Event Rate (LER) Instrumentation

**Tactic:** CSVF.TA0005 Exfiltration Controls / CSVF.TA0009 Monitoring  
**CSVF Crosswalk:** CSVF-EXF-01, CSVF-MON-01, CSVF-ASR-01

Instrument and compute Leakage Event Rate (LER), defined as the rate at which seeded protected secrets or protected meaning appears in outputs, weighted by materiality. Use LER for release-gate regression checks and trending.

---

### CSVF.M0027 — Crawl-Resilience Score (CRS) Multi-Session Extraction Testing

**Tactic:** CSVF.TA0008 Adversary Testing / CSVF.TA0009 Monitoring  
**CSVF Crosswalk:** CSVF-ADV-01, CSVF-MON-02

Measure resilience to multi-session extraction attempts by computing Crawl-Resilience Score (CRS), defined as the success rate of exfiltration workflows across sessions, prompt variations, and time. Use CRS to validate that controls remain effective under persistent probing.

---

### CSVF.M0028 — Domain Inference Risk (DIR) Reachability Measurement

**Tactic:** CSVF.TA0006 Domain Reach Prevention / CSVF.TA0009 Monitoring  
**CSVF Crosswalk:** CSVF-INF-02, CSVF-DOM-02, CSVF-DOM-01

Compute Domain Inference Risk (DIR), defined as the percentage of test runs in which the system derives an out-of-domain conclusion using only in-domain inputs under defined boundary conditions. DIR operationalizes reachability by measuring what has become reachable as data sources, tools, and model capability evolve.

---

## CSVF Mitigation Matrix Summary

| ID | Mitigation | Tactic |
|---|---|---|
| M0001 | Cognitive Security Owner | TA0001 |
| M0002 | Cognitive Security Risk Register | TA0001 |
| M0003 | Export Control and Deemed Export Coverage | TA0001 |
| M0004 | Domain Inventory and Join Matrix | TA0002 |
| M0005 | Unreachable Statement Classes | TA0002 |
| M0006 | Boundary Enforcement Map | TA0002 |
| M0007 | Secret Taxonomy and Labeling Policy | TA0003 |
| M0008 | Pre-ingestion Classification and Redaction | TA0003 |
| M0009 | Data Provenance and Lineage | TA0003 |
| M0010 | Least-Privilege Retrieval | TA0004 |
| M0011 | Session Information Budgets | TA0004 |
| M0012 | Memory Scoping and Retention | TA0004 |
| M0013 | Output Validation and Semantic Leakage Guardrails | TA0005 |
| M0014 | Canary and Honeytoken Instrumentation | TA0005 |
| M0015 | Revocation and Downstream Purge Playbooks | TA0005 |
| M0016 | High-Sensitivity Join Approval Workflow | TA0006 |
| M0017 | Cross-Domain Inference Testing and DIR Metric | TA0006 |
| M0018 | Prohibit LLM Authorization Decisioning | TA0006 |
| M0019 | Consumer LLM Submission Restrictions | TA0007 |
| M0020 | Approved Enterprise and API Pathways | TA0007 |
| M0021 | Cloud Prompt Logging and DLP Hooks | TA0007 |
| M0022 | OWASP LLM Suite Release Gate | TA0008 |
| M0023 | Standard Cognitive Security Event Schema | TA0009 |
| M0024 | Drift and Boundary Regression Detection | TA0009 |
| M0025 | Evidence Pack and SOC-Style Crosswalk | TA0010 |
| M0026 | LER Instrumentation | TA0005 / TA0009 |
| M0027 | CRS Multi-Session Extraction Testing | TA0008 / TA0009 |
| M0028 | DIR Reachability Measurement | TA0006 / TA0009 |

---

<!-- TODO: Format mitigation objects as ATLAS-compatible YAML/JSON for submission to the MITRE ATLAS data repository -->
<!-- TODO: Identify specific ATLAS techniques that CSVF mitigations address and create explicit mappings -->
<!-- TODO: Develop case study annotations showing how ATLAS techniques produce semantic leakage or unauthorized domain reach -->
<!-- TODO: Create ATLAS Navigator layer file (JSON) mapping CSVF mitigations to ATLAS technique IDs -->
<!-- TODO: Engage MITRE ATLAS team with proposed contributions -->
<!-- TODO: Add ATLAS technique references once technique IDs are confirmed against the public ATLAS data repo -->

---

*Related: [`crosswalks/owasp.md`](owasp.md) | [`crosswalks/nist-ai-rmf.md`](nist-ai-rmf.md)*  
*Back to framework: [`csvf/cognitive-security-verification-framework.md`](../csvf/cognitive-security-verification-framework.md)*
