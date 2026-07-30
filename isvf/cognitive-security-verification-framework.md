# Idea Security Verification Framework (ISVF)

> **Status:** Early public draft. This document is open for community review, critique, and contribution. It should not yet be treated as a finished consensus standard.

---

## Conceptual Overview

Organizations are deploying large language models into high-stakes workflows faster than they can define clear limits on what those systems are allowed to know, what domains they may combine, and what conclusions they may reach. The central problem within the data loss protection subfield of cybersecurity will no longer only be direct disclosure of secrets. New problems have emerged: **semantic leakage**, where a system reveals protected meaning without exposing the original source material, and **cross-domain inference**, where individually permitted fragments are combined into a result that policy would treat as prohibited. In this environment, organizations must move beyond controlling access to documents and begin controlling what conclusions their systems can derive, what joins they are permitted to perform, and what classes of statements must remain unreachable.

This framework is premised on an institutional argument about adoption. Standards need not emerge only through top-down coercion. They can also develop from the bottom up through practitioner communities and reputational legitimacy. The Idea Security Verification Framework (ISVF) is designed to function in that bottom-up mode. It aims to earn authority by being useful: by helping engineers build safer systems, helping Chief Information Security Officers (CISOs) define defensible boundaries, and helping compliance teams audit real controls.

The current standards ecosystem still lacks a practical, auditable framework for defining and testing inference boundaries. Existing institutions such as the National Institute of Standards and Technology (NIST), MITRE, and the Open Worldwide Application Security Project (OWASP) provide important building blocks, but none alone provides an end-to-end assurance model for semantic leakage, unauthorized domain reach, and the problem of sensitive conclusions becoming reachable in LLM-enabled systems.

The framework centers on four ideas:

1. **Domain modeling and permitted joins** — organizations must inventory their information domains and specify which combinations are allowed, prohibited, or subject to heightened approval.
2. **Unreachable Statement Classes** — organizations must define the categories of conclusions that must not become reachable, including derived conclusions rather than only verbatim secrets.
3. **Inference testing through Domain Inference Risk (DIR)** — organizations must repeatedly test whether prohibited conclusions are actually reachable in practice.
4. **Evidence packs for assurance and procurement** — organizations must be able to document and audit their boundary claims.

The framework further argues for several mitigation techniques. Prevention must begin upstream, before sensitive information is ingested into vector stores, memory layers, prompts, or fine-tuning corpora. No post-leak response is complete without downstream purge where possible, vendor engagement where a model owner exists, and strategic reassessment when leaked intellectual property may have affected enterprise value. For some of the most sensitive environments, the best solution may be architectural and hardware-focused rather than policy-focused: certain workflows should never enter a public or vendor-controlled model plane at all, and should instead run only on locally controlled models deployed on organization-owned hardware.

ISVF is therefore an attempt to make the expanding space of model-enabled inference legible, governable, testable, and auditable before ambient AI systems make that problem unmanageable.

---

## Executive Summary

Existing institutions such as OWASP, NIST, and MITRE provide important solutions for protecting classified, proprietary, and personal information in the AI era, but none yet offers a practical, auditable verification framework for defining inference boundaries, testing whether prohibited conclusions are reachable, and producing evidence that can support procurement, assurance, and governance.

The Idea Security Verification Framework is best understood as a **drop-in operational layer** for the current standards ecosystem. Its core contributions are:

- a domain inventory and join matrix
- Unreachable Statement Classes
- repeatable testing through draft metrics: Domain Inference Risk (DIR), Leakage Event Rate (LER), and Crawl-Resilience Score (CRS)
- evidence packs that make boundary claims legible to engineers, CISOs, auditors, and buyers

The broader argument is that organizations must govern not only access to files, but also what conclusions become reachable once retrieval, memory, tools, and synthesis are introduced into a single inferential system. The framework is an attempt to make model-enabled inference governable before ambient AI makes the problem too widespread and too normalized to control.

---

## Introduction

Early drafts of this project referred to it as "Idea Security Standards" because the initial aim was to identify what a practical standard of care for semantic leakage and cross-domain inference in LLM-enabled systems would require. As the research developed, however, that label became too strong. This document does not present a standalone consensus standard. It presents a **Idea Security Verification Framework (ISVF)**: a verification-oriented set of control concepts, testing methods, metrics, and evidence expectations designed to strengthen existing institutions rather than replace them. OWASP, NIST, and MITRE already provide much of the surrounding ecosystem. This framework should be understood as a living document.

OWASP's LLM Top 10 and OWASP's GenAI Data Security Risks and Mitigations Guide identify major risk classes and practical mitigations, but they are not themselves full procurement-grade verification regimes. NIST AI RMF 1.0 provides voluntary governance structure and risk-management vocabulary. MITRE ATLAS grounds AI security work in adversary tactics, techniques, mitigations, and case studies, but it is primarily a threat-informed knowledge base rather than a normative assurance framework. ISVF is therefore best understood as a **contribution layer**: it proposes concrete verification artifacts, measurement ideas, and evidence expectations that can be inserted into those existing frameworks.

The central claim is that existing frameworks do not yet fully operationalize one particular problem: whether an LLM-enabled system can combine individually permitted fragments into a conclusion that policy would treat as prohibited. ISVF brings those pieces together as a verification problem centered on boundary claims, permitted joins, statement-level prohibitions, repeatable testing, and evidence for assurance.

ISVF is also intentionally provisional. The proposed metrics — Domain Inference Risk (DIR), Leakage Event Rate (LER), and Crawl-Resilience Score (CRS) — should be treated as draft verification concepts rather than finished measures. They are included because a verification framework without metrics would remain abstract, but substantial work remains to formalize definitions, standardize test protocols, establish thresholds, and validate these measures across real deployments.

---

## Background

### Semantic Leakage and Reachability

Traditional information security assumes separation of data works: data sits in compartments (HIPAA vs. non-HIPAA, export-controlled vs. public, proprietary vs. customer-facing), and access control at the boundaries largely determines what a user can know. LLM-enabled systems weaken that assumption because they convert scattered text into a single medium that can be searched, summarized, and recombined. The exposure is therefore not limited to verbatim leakage; we must now also consider **semantic leakage**, where the system reconstructs protected meaning from individually permissible fragments.

That phenomenon is not entirely new. Humans have always been able to piece together sensitive conclusions from scattered clues. Intelligence analysis and investigative journalism, for instance, have always rested on the idea that separate harmless facts can become sensitive when combined. A skilled analyst could infer a military deployment window from shipping records, weather patterns, maintenance notices, and public statements, even if none of those items was classified by itself. The difference is that this traditionally happened at human speed and required time, expertise, and usually a fairly narrow set of actors.

Existing safeguards partly reflect that older world. Organizations try to address the problem through classification rules, need-to-know restrictions, compartmentation, and insider threat programs. In practice, they also rely on friction — that friction slows down synthesis and reduces the number of people who can realistically perform it.

LLMs make the problem worse because they dramatically reduce the cost of synthesis. They can search, summarize, and recombine across large volumes of text in seconds. They also do this in workflows that feel normal and low-friction to users. A person no longer has to manually read hundreds of emails, logs, and reports to infer a sensitive conclusion. The model can do so at scale. As context windows grow, retrieval improves, and tools connect models to more repositories, the practical barriers that once limited inference are weakened further. What used to require a skilled human analyst now becomes accessible to ordinary users through routine prompting. That is why ISVF treats the problem as one of **reachability**.

### Defining "Reachable"

In ISVF, **reachable** refers to the set of conclusions an AI system can reliably produce from its underlying information substrate under real operating conditions. The substrate includes what the system can draw on through prompts, logs, connected repositories, and so forth. The central question has moved from "who can open which file" to "what conclusions become realistically derivable once the system can traverse and synthesize across the organization's information substrate."

### Exfiltration vs. Unauthorized Domain Reach

ISVF treats semantic leakage as two distinct failure modes:

- **Exfiltration risk** — the unauthorized movement of protected information from the user's domain into an LLM system or LLM-enabled workflow, especially third-party or otherwise ungoverned model planes. The typical failure mode is a user pasting sensitive material into a cloud assistant because it is the fastest way to draft, summarize, translate, or debug.
- **Unauthorized domain reach risk** — when an LLM-enabled system crosses an epistemic boundary by outputting conclusions functionally equivalent to higher-domain information even without explicit authorized access to higher-domain files.

### Relevant Regulatory Frameworks

ISVF is not a proposed privacy statute. It is a security and assurance standard meant to create a legible standard of care for information leakage and boundary erosion in LLM-enabled systems. The standard is designed to be usable inside existing legal frameworks that already care about disclosure and confidentiality, including:

- HIPAA
- 42 CFR Part 2
- GLBA
- FERPA
- GDPR principles
- CCPA / CPRA
- FTC Section 5 enforcement posture
- Trade secret law
- Export control regimes including deemed exports and ITAR technical data

ISVF's thesis is that these regimes already impose confidentiality duties, but they do not define what it means to prevent semantic leakage and unauthorized domain reach risk. ISVF supplies that missing layer.

### Relevant Organizations

**OWASP (primary client).** The Open Worldwide Application Security Project is the primary client for ISVF because it influences what engineers actually test and ship. The OWASP Top 10 for LLM Applications and the OWASP Data Security Risks and Mitigations explicitly include categories like Sensitive Information Disclosure and Insecure Plugin Design, which map cleanly onto ISVF's exfiltration and tool/agent control families.

**NIST.** The National Institute of Standards and Technology provides the governance vocabulary and lifecycle framing already used across industry and government. The NIST AI Risk Management Framework (AI RMF 1.0) supplies widely adopted risk functions and outcomes that ISVF can align to while adding a more control-oriented, inference-specific operational layer.

**MITRE.** MITRE ATLAS (Adversarial Threat Landscape for Artificial-Intelligence Systems) provides adversary-grounded tactics and techniques against AI-enabled systems. ISVF is designed to complement ATLAS by translating threat realism into boundary claims, test methods, and evidence requirements that can be assessed.

---

## Control Families

ISVF organizes its controls into eight families. Each family addresses a distinct layer of idea security risk. Together they span the full lifecycle of an LLM-enabled system — from governance and data ingestion through inference testing and procurement-ready assurance.

### Family A — Governance and Accountability

- Appoint a idea security owner with defined authority over boundary decisions, risk acceptance, and evidence readiness.
- Maintain a idea security risk register that explicitly includes semantic leakage, unauthorized domain reach, and reachability drift.
- Define ownership for domain boundaries, join approvals, and residual-risk acceptance decisions.
- Include export-controlled technical data, legal privilege, regulated data, trade secrets, and other high-consequence categories where relevant.

### Family B — Domain Modeling and Boundary Claims

- Define the information domains in scope for each LLM-enabled system.
- Specify allowed, prohibited, and approval-gated joins between domains.
- Create Unreachable Statement Classes that describe, at the level of semantic outcome, the conclusions the system must not make reachable.
- Build a Boundary Enforcement Map showing where controls operate across retrieval, context assembly, tools, memory, and output handling.

### Family C — Data Classification and Secret Handling

- Classify and label sensitive material before it is ingested into any AI-adjacent store.
- Propagate labels to chunks, embeddings, caches, memory layers, prompts, and fine-tuning corpora so downstream systems can enforce policy consistently.
- Quarantine ambiguous or unlabeled material rather than defaulting it into general-purpose AI workflows.

### Family D — Context, Retrieval, and Memory Controls

- Enforce least-privilege retrieval through RBAC or ABAC so the model context never expands beyond user and task authorization.
- Use session information budgets to cap cumulative sensitivity in a single context window.
- Scope memory by user, domain, purpose, and retention window.
- Prevent agents from silently widening the system's reachable conclusion space through tool use, memory writes, or connector access.

### Family E — Exfiltration Controls

- Use semantic output validation, not only keyword scanning, to detect protected meaning leaving the system.
- Instrument canaries, honeytokens, and honey ideas in high-value domains to detect and attribute leakage.
- Maintain revocation and downstream purge playbooks for vector stores, caches, prompt logs, and memory layers, and validate them through tabletop exercises.

### Family F — Unauthorized Domain Reach Controls

- Require documented approval and written rationale for high-sensitivity joins before they are enabled.
- Test whether restricted conclusions can be derived from permitted inputs, and track Domain Inference Risk over time.
- Prohibit LLMs from making authorization decisions; authorization must be deterministic, auditable, and enforced outside the model.
- Monitor for reachability drift after changes to models, prompts, retrieval settings, tools, connectors, or memory.

### Family G — Cloud Prompting Governance

- Define what data may never be submitted to consumer or unapproved cloud LLMs and enforce those restrictions technically, not by policy text alone.
- Require approved enterprise or API pathways — with confirmed contractual protections — wherever sensitive data is involved.
- Use logging and DLP integration for prompt flows where feasible.
- For the most sensitive workflows, require locally controlled models on organization-owned or organization-controlled hardware.

### Family H — Assurance and Reporting

- Maintain evidence packs that demonstrate control design, implementation, and operating effectiveness for each in-scope system.
- Run standardized test harnesses at release gates and after material system changes.
- Produce SOC-style management assertions or auditor-facing reports where appropriate.
- Make boundary claims legible for procurement, compliance, and board oversight.

---

## Evidence Packs

At minimum, the assurance pack should function as a single evidentiary record that the system's idea boundaries are defined, enforced, and monitored over time.

### Required Contents

- System boundary description and architecture diagram
- Domain inventory and join matrix
- Unreachable Statement Class catalog
- Boundary enforcement map
- Pre-ingestion classification and redaction evidence
- Output-validation, canary, and purge controls
- Join approvals and cross-domain inference test results
- Proof that authorization is enforced outside the model
- Release-gate adversarial testing records
- Telemetry and drift-monitoring records
- Change logs for models, prompts, retrieval settings, policies, and tools
- Incident records and remediation status
- Vendor-control artifacts for approved cloud pathways
- Explicit risk acceptances for any high-sensitivity joins or residual exposures

### Exfiltration Evidence

Should show that the system can detect, block, and respond to outbound leakage of protected meaning through:

- output-validator configurations
- canary and honeytoken deployment records
- LER trends
- CRS results
- purge playbooks
- tabletop exercises
- post-incident containment records

### Unauthorized Domain Reach Evidence

Should show that the system prevents impermissible cross-domain access through:

- the join matrix
- USC-driven test cases
- DIR trend data
- blocked cross-domain retrieval events
- session-budget settings
- memory-scoping controls

---

## What Existing Frameworks Already Cover, and What ISVF Adds

| Area | What Frameworks Already Cover | ISVF's Delta |
|---|---|---|
| Governance and risk framing | OWASP 2026 covers governance, lifecycle, and classification. NIST covers governance, mapping, measurement, management, and profiles. ATLAS provides an adversary-informed structure. | Requires organizations to make explicit boundary claims about what cross-domain combinations are and are not allowed, assign ownership for those claims, and verify them with repeatable evidence. |
| Insufficiency of access control | OWASP recognizes sensitive disclosure and vector/embedding weaknesses. ABAC and RBAC answer who may access which resource. | The Domain Inventory and Join Matrix. The question shifts from "may this principal read this object?" to "may this system combine domain A, domain B, tool C, and memory D in one inferential workflow?" |
| Gaps in Data Loss Prevention | OWASP covers sensitive information disclosure and improper output handling. Existing DLP aims to stop direct leakage or unsafe outputs. | The Unreachable Statement Class concept. Instead of only asking whether a forbidden string leaves the system, ISVF asks whether the system can produce a prohibited conclusion, ranking, synthesis, or forecast even when no exact secret string appears. |
| Assurance and procurement | OWASP is helpful for builders and defenders. ATLAS is helpful for threat-informed operations. NIST provides broad structure. | The evidence pack: a compact record of boundary claims, controls, and tests. |

---

## Conclusion

LLM-era security requires standards that treat meaning, joins, and inference as first-class security objects. ISVF provides a structure for managing that shift across chat, retrieval-augmented generation, agentic tool use, and memory. It does so by:

- requiring explicit boundary claims through domain inventories, permitted joins, and enforcement maps
- defining what must remain unreachable through statement-level prohibitions that include derived conclusions
- testing and quantifying reachability risk through DIR-based inference testing
- packaging the resulting evidence in forms that support assurance, procurement, and audit

The paper's contribution is broader than those four pillars alone. It argues that idea security must begin upstream, before sensitive information is ingested into vector stores, prompt contexts, or memory layers. It argues that organizations must pay far more attention to joins, especially the difficult problem of anticipating which combinations of individually permissible domains will later produce prohibited inferences. It argues that Unreachable Statement Classes are especially useful because they force policy to name the outcomes that truly matter. It argues that post-leak response must extend beyond simple disclosure handling to include purge, containment, vendor engagement where possible, architectural revision, and in some cases strategic reassessment of the value lost when sensitive knowledge is no longer exclusive. And it argues that for the most sensitive workflows, the correct boundary may be architectural: some information should never enter a vendor-controlled model plane at all.

Finally, ISVF is best understood not as a finished answer, but as a practical starting point for making inference boundaries legible and enforceable before those boundaries disappear into the background of ordinary organizational life.

---

## Appendix A — Glossary

See [`glossary/glossary.md`](../glossary/glossary.md) for the full ISVF glossary.

---

## Appendix B — Related Writings by Author

This appendix collects related conceptual writings by the author that informed the vocabulary and framing of this framework. These writings are not offered as independent validation and do not substitute for primary sources. Where cited in the body, they are used to clarify concepts introduced here.

### Why "Ambient" LLMs Negate Policy Boundaries

*Living With LLMs Everywhere* argues that LLMs are becoming ambient: they appear inside the everyday stack, often outside the single "chat box" mental model that most governance policies still assume. The implication is that privacy and security controls that rely on intentional user behavior ("I won't paste sensitive things into ChatGPT") degrade as interfaces multiply and boundaries dissolve. It also emphasizes that risk is not limited to whether prompts are used for training; content can be retained, logged, reviewed, routed through vendors, and later reintroduced elsewhere in a "leakage cascade."

### Mapping Inference Risk

*PageRank for Inference: Mapping Reachability in LLM Systems* argues that every major computing era creates a new kind of disorder before someone builds the framework that makes it legible and usable. In the early web, PageRank helped bring order to a chaotic internet by turning an unstructured mass of pages and links into something navigable, measurable, and trustworthy enough for people to use with confidence. The same problem is now emerging in LLM systems, where the challenge is no longer just locating information but understanding what becomes reachable when models connect scattered fragments into new conclusions. ISVF aims to do for reachable information space what PageRank did for the web: impose structure on a domain that is currently powerful but poorly understood.

### Theoretical Bounds on LLM Reachability

*The Limits of LLM-Reachable Intelligence* provides a useful caution: even if we formalize "reachable" as "claims produced with justifications accepted by a verifier," the reachable set remains bounded by the verifier and is incomplete relative to the full space of truths in the domain. The practical contribution to ISVF is that it sets theoretical bounds on the limits of inference as a medium and points toward room for human contribution to information systems.

### Gaps in Meaning

*Large Language Models and Gaps in Meaning* argues that discrete token sequences live in a countable combinatorial space, while human meanings are better modeled as points on a continuous "meaning manifold." It proposes a three-space framework (token space, meaning manifold, and an "information space") and defines the information space as the subset of meanings that are both reachable by a generator and endorsed by a verifier.

For ISVF, the relevance is:

1. A system can block verbatim secrets yet still allow the meaning (the protected conclusion) to become reachable via paraphrase.
2. If "unreachable" is defined only as "no exact match," the system is governing token space while the harm occurs in meaning space.
3. Discretization and embedding approximations introduce blind spots and distortions in meaning representation, which can allow unintended inferences to slip through controls.
4. The gap between token space and the meaning manifold implies that perfect control at the string level cannot guarantee control at the semantic level.
5. Because the "information space" is defined by what a generator can produce and a verifier will accept, reachability is system-dependent.

---

*See [`works-cited.md`](../works-cited.md) for full references.*
