# CSVF × OWASP Crosswalk

This document describes how CSVF concepts, controls, and measurement constructs can be inserted into existing OWASP projects. It is structured in "drop-in" format: the point is not just to show conceptual alignment, but to identify exactly where CSVF language could be added.

---

## OWASP Positioning

Within OWASP, Top 10 projects are best understood as community-driven risk-prioritization artifacts rather than full assurance standards. Their function is to identify the most important classes of vulnerability, create a shared vocabulary for practitioners, and provide developers and defenders with a practical starting point for what to test and mitigate first.

CSVF is not best presented as a rival Top 10. It is better understood as an **operational layer** that can be inserted into existing OWASP projects to address a gap they already expose but do not fully resolve.

**The gap:** Both the OWASP LLM Top 10 and the OWASP GenAI Data Security document lack a practical, auditable framework for:
- defining information domains
- specifying which joins are allowed
- defining prohibited semantic outcomes
- measuring whether those outcomes become reachable through inference

That is the contribution CSVF is intended to make.

---

## Proposed Drop-ins: OWASP Top 10 for LLM Applications 2025

### Front Matter or "Moving Forward" Section

**Proposed addition:** Some GenAI failures depend not only on direct disclosure, but on whether a system can combine permitted information into conclusions that policy would treat as prohibited. CSVF concepts would be introduced here as a complementary boundary and assurance layer that defines domains, permitted joins, prohibited semantic outcomes, and measurable inference tests.

---

### Appendix 1: LLM Application Architecture and Threat Modeling

**Proposed addition:** A "Cognitive Security Boundary Questions" subsection asking:

1. What are the information domains in this system?
2. Which joins between those domains are explicitly permitted?
3. Where is the boundary enforced: retrieval, context assembly, tools, or output?
4. What classes of conclusions must not become reachable?
5. What telemetry and test methods detect when previously unreachable conclusions become reachable?

---

### LLM02:2025 — Sensitive Information Disclosure

**Proposed addition:** Clarify that disclosure may be **semantic**, not only verbatim. The new text would recommend:
- defining Unreachable Statement Classes for sensitive conclusions
- enforcing domain-aware retrieval and context boundaries
- measuring leakage not only through direct string exposure but also through semantic leakage and inference

This connects the existing disclosure category to CSVF concepts: USCs, LER, and DIR.

---

### LLM06:2025 — Excessive Agency

**Proposed addition:** Explain that agents should be constrained not only by action permissions but also by **join permissions**. Agents should have defined limits on which domains they may combine through retrieval, tool use, and memory, and high-sensitivity joins should require explicit approval. This brings CSVF's treatment of tools and agent workflows into OWASP's existing agency category.

---

### LLM08:2025 — Vector and Embedding Weaknesses

**Proposed addition:** Clarify that retrieval systems should enforce a **domain-aware join matrix**, not just basic access control. Embeddings and retrieved passages should be treated as cross-domain combination surfaces, and systems should test whether vector retrieval makes prohibited conclusions reachable even when no single retrieved document is prohibited on its own.

---

### LLM07:2025 — System Prompt Leakage and LLM05:2025 — Improper Output Handling

**Proposed addition:** Cross-reference to the CSVF **Boundary Enforcement Map**, which requires organizations to document whether meaningful enforcement occurs at retrieval, context assembly, tools, or output handling. This gives OWASP entries a more explicit architectural model of where the security boundary actually resides.

---

## Proposed Drop-ins: OWASP GenAI Data Security Risks and Mitigations 2026

### Document Scope and Objectives

**Proposed addition:** Explain that the document identifies GenAI data risk classes and tiered mitigations, but that some organizations also need an inference-boundary layer. CSVF would be presented as a complementary control-oriented framework for defining domains, authorized joins, prohibited semantic outcomes, and measurable inference tests.

---

### What Is Data Security in the GenAI Context?

**Proposed addition:** Extend the current discussion of context fusion by stating that once prompts, RAG results, tool outputs, and conversation history are merged into a single flat namespace, the relevant security question is not only whether sensitive data appears verbatim, but whether **protected conclusions become reachable through synthesis or inference**.

---

### AI-Data Security Posture Management (DSPM) Section

**Proposed addition:** Require a new capability category titled **"Cognitive Boundary Modeling and Reachability Testing"** that includes:
- domain inventories
- permitted-join matrices
- Unreachable Statement Class catalogs
- session information budgets
- Domain Inference Risk testing

This is likely the cleanest place to position CSVF as an extension of OWASP's already sophisticated governance and posture-management approach.

---

### DSGAI01: Sensitive Data Leakage

**Proposed addition:** Under both the attack description and mitigations, note that leakage may be **semantic rather than verbatim**. Mitigation additions would recommend:
- defining USCs for sensitive conclusions
- adding semantic-output testing alongside string-based testing
- measuring Domain Inference Risk where sensitive meaning can be reconstructed from individually permissible fragments

---

### DSGAI03: Shadow AI & Unsanctioned Data Flows

**Proposed addition:** Require cloud prompting governance language that:
- classifies which data domains may never be sent to consumer or unsanctioned external models
- requires approved pathways for sensitive workflows
- recognizes ambient copilots and browser assistants as connectors that can create unauthorized joins across domains

---

### DSGAI06: Tool, Plugin & Agent Data Exchange Risks

**Proposed addition:** State that tools and agents are not just action surfaces but **domain-combination surfaces**. Mitigations should:
- define which tools may combine which domains
- require approval for high-sensitivity joins
- ensure that agent memory and tool outputs do not silently widen the reachable conclusion space

---

### DSGAI07: Data Governance, Lifecycle & Classification for AI Systems

**Proposed addition:** Cover:
- named ownership for inference-boundary decisions
- risk registers that include semantic leakage and unauthorized domain reach
- evidence-pack requirements
- propagation of classification labels not only to raw data but also to embeddings, caches, and memory artifacts

---

### DSGAI15: Over-Broad Context Windows & Prompt Over-Sharing

**Proposed addition:** Require **session information budgets** as a specific mitigation. The idea is not merely minimization in the abstract, but explicit caps on how much sensitive material — and which domain combinations — may enter one context window during a session.

---

### DSGAI18: Inference & Data Reconstruction

**Proposed addition:** Require three core CSVF insertions:
1. **Domain Inference Risk** as a measurable, repeatable metric
2. **Unreachable Statement Classes** as semantic regions that must remain unreachable
3. **Regression testing** after changes to models, retrieval, prompts, tools, or memory

This is the strongest substantive fit between the existing OWASP taxonomy and the CSVF measurement layer.

---

## CSVF ↔ OWASP LLM Top 10 Mapping Summary

| OWASP LLM Entry | CSVF Concepts | CSVF Controls |
|---|---|---|
| LLM02 Sensitive Information Disclosure | Semantic leakage, USCs, LER | CSVF-EXF-01, CSVF-DOM-02 |
| LLM05 Improper Output Handling | Boundary Enforcement Map | CSVF-DOM-03 |
| LLM06 Excessive Agency | Join permissions, join matrix | CSVF-INF-01, CSVF-DOM-01 |
| LLM07 System Prompt Leakage | Boundary Enforcement Map | CSVF-DOM-03 |
| LLM08 Vector and Embedding Weaknesses | Domain-aware join matrix, DIR | CSVF-DOM-01, CSVF-INF-02 |

---

## CSVF ↔ OWASP GenAI Data Security Mapping Summary

| OWASP DSGAI Entry | CSVF Concepts | CSVF Controls |
|---|---|---|
| DSGAI01 Sensitive Data Leakage | USCs, LER, DIR, semantic leakage | CSVF-EXF-01, CSVF-DOM-02, CSVF-INF-02 |
| DSGAI03 Shadow AI | Cloud prompting governance | CSVF-CLD-01, CSVF-CLD-02 |
| DSGAI06 Tool/Agent Data Exchange | Join permissions, domain-combination surfaces | CSVF-INF-01, CSVF-DOM-01 |
| DSGAI07 Data Governance / Classification | Evidence packs, classification propagation | CSVF-ASR-01, CSVF-DATA-01 |
| DSGAI15 Over-Broad Context Windows | Session information budgets | CSVF-CTX-02 |
| DSGAI18 Inference & Reconstruction | DIR, USCs, regression testing | CSVF-INF-02, CSVF-DOM-02, CSVF-MON-02 |

---

<!-- TODO: Expand each OWASP drop-in with exact proposed text suitable for submission to OWASP working groups -->
<!-- TODO: Add issue/PR references once OWASP contributions are submitted -->
<!-- TODO: Update mappings as OWASP LLM Top 10 and GenAI Data Security documents are revised -->
<!-- TODO: Add crosswalk to OWASP SAMM (Software Assurance Maturity Model) -->

---

*Related: [`crosswalks/nist-ai-rmf.md`](nist-ai-rmf.md) | [`crosswalks/mitre-atlas.md`](mitre-atlas.md)*  
*Back to framework: [`csvf/cognitive-security-verification-framework.md`](../csvf/cognitive-security-verification-framework.md)*
