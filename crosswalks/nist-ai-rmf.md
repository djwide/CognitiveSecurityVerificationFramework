# CSVF × NIST AI RMF Crosswalk

This document describes how CSVF concepts can be inserted into the NIST Artificial Intelligence Risk Management Framework (AI RMF 1.0) and its companion resources. It is structured in "drop-in" format: the point is to show not only that CSVF conceptually aligns with the AI RMF, but exactly where NIST language, categories, profiles, and companion resources could absorb cognitive security concepts.

---

## NIST Positioning

The National Institute of Standards and Technology Artificial Intelligence Risk Management Framework is designed as a voluntary, rights-preserving, non-sector-specific, and use-case agnostic resource for organizations that design, develop, deploy, or use AI systems. Its purpose is to help those organizations manage AI risks while promoting trustworthy and responsible development and use. The framework is organized around four core functions: **GOVERN**, **MAP**, **MEASURE**, and **MANAGE**.

NIST is an appropriate institutional home for CSVF. The AI RMF already provides the governance vocabulary, lifecycle framing, trustworthiness language, and measurement mindset that CSVF needs. NIST also explicitly recognizes that AI risk is hard to measure, that new metrics may need to be developed, that risks can emerge over time, and that profiles can be built for specific technologies such as large language models, cloud-based services, or acquisition contexts.

What CSVF contributes is not a replacement for that structure, but a more specific operational layer for one class of AI risk that the RMF currently leaves at a higher level of abstraction: **semantic leakage, unauthorized domain reach, and the reachability of prohibited conclusions in LLM-enabled systems**.

---

## Proposed Drop-ins: AI RMF 1.0

### Executive Summary / Part 1: Framing Risk

**Proposed addition:** Explain that in LLM-enabled systems, one class of AI risk is not merely direct disclosure, but the ability of a system to combine individually permitted fragments into conclusions that policy would treat as prohibited. A concise insertion would introduce **semantic leakage**, **cross-domain inference**, and **reachability** as examples of AI risks that differ from traditional software risks.

---

### Section 1.2.1 — Risk Measurement

NIST already states that many AI risks are hard to measure, that the lack of consensus on robust measurement methods is a challenge, and that organizations should identify and track emergent risks.

**Proposed addition:** A specific measurement layer:
- **Leakage Event Rate (LER)** — for protected meaning appearing in outputs
- **Crawl-Resilience Score (CRS)** — for multi-session exfiltration attempts
- **Domain Inference Risk (DIR)** — for the percentage of test runs in which an out-of-domain conclusion becomes reachable from in-domain inputs

In NIST terms, CSVF would be contributing exactly the sort of new measurement approach the RMF says may still need to be developed.

---

### Section 3: AI Risks and Trustworthiness

NIST's trustworthiness section identifies systems as secure and resilient, accountable and transparent, explainable and interpretable, and privacy-enhanced, among other characteristics.

**Proposed addition:** A bridging paragraph clarifying that these characteristics are strained in LLM systems when protected meaning becomes reachable through inference even where no single restricted file is directly disclosed. This could be positioned as a cross-cutting clarification that cognitive security is a condition of secure and resilient AI, privacy-enhanced AI, and accountable and transparent AI in systems built on retrieval, long context, memory, and tool use.

---

### Section 3.3 — Secure and Resilient

NIST already notes concerns such as adversarial examples, data poisoning, and exfiltration of models or training data.

**Proposed addition:** Secure and resilient LLM systems must also defend against **semantic exfiltration** and **unauthorized domain reach**, where the relevant failure is not theft of a source artifact but derivation of a protected conclusion. This extends NIST's security discussion to encompass inference-path risks alongside more familiar endpoint or dataset risks.

---

### Section 3.4 — Accountable and Transparent

**Proposed addition:** For LLM-enabled systems, meaningful transparency should include documentation of:
- where semantic boundaries are enforced (retrieval, context assembly, tool invocation, memory, or output gates)
- what classes of conclusions are intended to remain unreachable

This extends NIST's accountability logic into a concrete boundary model.

---

### Section 3.6 — Privacy-Enhanced

NIST already notes that AI can create privacy risks by allowing inference to identify individuals or previously private information.

**Proposed addition:** A more operational answer to that observation: privacy risk in LLM systems includes **inferential reconstruction from permitted fragments**, and privacy-enhancing practice should therefore include domain-aware retrieval, context limits, and testing of whether protected conclusions about individuals become reachable through synthesis.

---

## Proposed Drop-ins: AI RMF Core Functions

### GOVERN

The GOVERN function is meant to build a culture of risk management, align risk practices to organizational priorities, document roles and responsibilities, monitor outcomes, and handle third-party issues.

**Proposed additions** (fitting GOVERN 1, GOVERN 2, and GOVERN 6):

1. Require a **Cognitive Security Owner** concept under accountability structures.
2. Require a **cognitive security risk register** that explicitly includes semantic leakage and unauthorized domain reach.
3. Require policies for **cloud prompting governance**, including approved and prohibited model pathways.
4. Require **evidence-pack maintenance** for LLM systems whose trustworthiness depends on boundary testing and telemetry.

---

### MAP

The MAP function establishes context, documents intended purposes and limits, characterizes system methods and knowledge limits, and maps risks, impacts, and third-party components.

**Proposed additions:**

1. **Domain inventories** for the information spaces the system touches.
2. **Permitted join matrices** for which domains may be combined.
3. **Unreachable Statement Classes** describing semantic regions that must not become reachable.
4. Explicit documentation of **where the boundary is enforced** across retrieval, context, tools, memory, and outputs.

These are direct elaborations of NIST's existing calls to document context, knowledge limits, system requirements, and component risks.

---

### MEASURE

The MEASURE function already calls for quantitative, qualitative, or mixed methods, rigorous testing, TEVV, monitoring in production, new types of measurement where needed, and tracking emergent risks over time.

**Proposed additions** — a specialized LLM measurement package:

- LER, CRS, and DIR metrics
- Repeatable inference tests against Unreachable Statement Classes
- Regression detection after model, retrieval, prompt, or tool changes
- Evidence requirements for release gates

CSVF does not ask NIST to rethink the MEASURE function. It gives NIST a ready-made way to instantiate it for semantic leakage and reachability risk.

---

### MANAGE

The MANAGE function is about prioritizing and responding to risks, documenting residual risk, handling third-party dependencies, and maintaining incident response and continuous improvement.

**Proposed additions** (fitting MANAGE 1 through MANAGE 4):

1. **Purge and revocation playbooks** for embeddings, caches, and memory.
2. **Post-deployment monitoring** for reachability drift.
3. **Explicit response options** when prohibited conclusions become reachable.
4. **Documentation of residual cognitive security risk** to downstream users.
5. A **high-sensitivity architecture option** in which certain data domains are barred from cloud models and restricted to locally controlled inference environments.

---

## Proposed Drop-ins: AI RMF Profiles and Companion Resources

### Section 6: AI RMF Profiles

NIST explicitly says that profiles may be built for specific settings or applications, and that cross-sectoral profiles can cover technologies or business processes common across sectors, including large language models, cloud-based services, and acquisition.

**Proposed contribution:** A **Cross-Sectoral Cognitive Security Profile for LLM-Enabled Systems**, with variants for:
- RAG systems
- Copilots
- Agentic tools
- Cloud prompting governance

A profile could map the existing RMF functions to concrete CSVF outcomes, such as domain inventories in MAP, DIR metrics in MEASURE, and purge playbooks in MANAGE.

---

### AI RMF Playbook

Because the RMF says the Playbook offers tactical actions and tailored guidance that organizations can adapt to their own contexts, CSVF fits naturally as a set of Playbook inserts.

**Proposed tactical guidance:**
- How to build a join matrix
- How to define an Unreachable Statement Class
- How to test DIR
- How to set session information budgets
- How to document evidence for LLM release gates

---

### Roadmap / Future Measurement Work

The AI RMF notes that additional guidance and resources will be captured in an associated roadmap. CSVF's metrics layer is especially well suited to a future NIST measurement workstream.

**Proposed addition:** A proposal for NIST to treat semantic leakage and reachability as a candidate domain for new AI risk metrics, with DIR as the main unauthorized domain reach-oriented addition.

---

## CSVF ↔ NIST AI RMF Function Mapping

| NIST AI RMF Function | CSVF Contribution |
|---|---|
| GOVERN | Cognitive Security Owner, cognitive security risk register, cloud prompting governance policy, evidence-pack maintenance |
| MAP | Domain inventories, permitted join matrices, USCs, boundary enforcement documentation |
| MEASURE | LER, CRS, DIR metrics; USC-driven regression tests; release-gate evidence requirements |
| MANAGE | Purge and revocation playbooks, reachability drift monitoring, residual risk documentation, high-sensitivity architecture option |

---

## CSVF ↔ NIST Trustworthiness Characteristics Mapping

| NIST Characteristic | CSVF Relevance |
|---|---|
| Secure and Resilient | Semantic exfiltration and unauthorized domain reach defenses |
| Accountable and Transparent | Boundary enforcement documentation, USC declarations |
| Privacy-Enhanced | Inferential reconstruction prevention, domain-aware retrieval |

---

<!-- TODO: Draft formal profile document for Cross-Sectoral Cognitive Security Profile -->
<!-- TODO: Map CSVF controls to specific NIST AI RMF Playbook action items -->
<!-- TODO: Align CSVF evidence pack format to NIST documentation expectations -->
<!-- TODO: Engage NIST AI RMF working groups with proposed contributions -->
<!-- TODO: Map to NIST AI 600-1 (Generative AI Profile) specifically -->

---

*Related: [`crosswalks/owasp.md`](owasp.md) | [`crosswalks/mitre-atlas.md`](mitre-atlas.md)*  
*Back to framework: [`csvf/cognitive-security-verification-framework.md`](../csvf/cognitive-security-verification-framework.md)*
