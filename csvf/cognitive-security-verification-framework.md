# Cognitive Security Verification Framework (CSVF)

> **Status:** Early public draft. This document is open for community review, critique, and contribution. It should not yet be treated as a finished consensus standard.

---

## Conceptual Overview

Organizations are deploying large language models into high-stakes workflows faster than they can define clear limits on what those systems are allowed to know, what domains they may combine, and what conclusions they may reach. The central problem within the data loss protection subfield of cybersecurity will no longer only be direct disclosure of secrets. New problems have emerged: **semantic leakage**, where a system reveals protected meaning without exposing the original source material, and **cross-domain inference**, where individually permitted fragments are combined into a result that policy would treat as prohibited. In this environment, organizations must move beyond controlling access to documents and begin controlling what conclusions their systems can derive, what joins they are permitted to perform, and what classes of statements must remain unreachable.

This framework is premised on an institutional argument about adoption. Standards need not emerge only through top-down coercion. They can also develop from the bottom up through practitioner communities and reputational legitimacy. The Cognitive Security Verification Framework (CSVF) is designed to function in that bottom-up mode. It aims to earn authority by being useful: by helping engineers build safer systems, helping Chief Information Security Officers (CISOs) define defensible boundaries, and helping compliance teams audit real controls.

The current standards ecosystem still lacks a practical, auditable framework for defining and testing inference boundaries. Existing institutions such as the National Institute of Standards and Technology (NIST), MITRE, and the Open Worldwide Application Security Project (OWASP) provide important building blocks, but none alone provides an end-to-end assurance model for semantic leakage, unauthorized domain reach, and the problem of sensitive conclusions becoming reachable in LLM-enabled systems.

The framework centers on four ideas:

1. **Domain modeling and permitted joins** — organizations must inventory their information domains and specify which combinations are allowed, prohibited, or subject to heightened approval.
2. **Unreachable Statement Classes** — organizations must define the categories of conclusions that must not become reachable, including derived conclusions rather than only verbatim secrets.
3. **Inference testing through Domain Inference Risk (DIR)** — organizations must repeatedly test whether prohibited conclusions are actually reachable in practice.
4. **Evidence packs for assurance and procurement** — organizations must be able to document and audit their boundary claims.

The framework further argues for several mitigation techniques. Prevention must begin upstream, before sensitive information is ingested into vector stores, memory layers, prompts, or fine-tuning corpora. No post-leak response is complete without downstream purge where possible, vendor engagement where a model owner exists, and strategic reassessment when leaked intellectual property may have affected enterprise value. For some of the most sensitive environments, the best solution may be architectural and hardware-focused rather than policy-focused: certain workflows should never enter a public or vendor-controlled model plane at all, and should instead run only on locally controlled models deployed on organization-owned hardware.

CSVF is therefore an attempt to make the expanding space of model-enabled inference legible, governable, testable, and auditable before ambient AI systems make that problem unmanageable.

---

## Executive Summary

Existing institutions such as OWASP, NIST, and MITRE provide important solutions for protecting classified, proprietary, and personal information in the AI era, but none yet offers a practical, auditable verification framework for defining inference boundaries, testing whether prohibited conclusions are reachable, and producing evidence that can support procurement, assurance, and governance.

The Cognitive Security Verification Framework is best understood as a **drop-in operational layer** for the current standards ecosystem. Its core contributions are:

- a domain inventory and join matrix
- Unreachable Statement Classes
- repeatable testing through draft metrics: Domain Inference Risk (DIR), Leakage Event Rate (LER), and Crawl-Resilience Score (CRS)
- evidence packs that make boundary claims legible to engineers, CISOs, auditors, and buyers

The broader argument is that organizations must govern not only access to files, but also what conclusions become reachable once retrieval, memory, tools, and synthesis are introduced into a single inferential system. The framework is an attempt to make model-enabled inference governable before ambient AI makes the problem too widespread and too normalized to control.

---

## Introduction

Early drafts of this project referred to it as "Cognitive Security Standards" because the initial aim was to identify what a practical standard of care for semantic leakage and cross-domain inference in LLM-enabled systems would require. As the research developed, however, that label became too strong. This document does not present a standalone consensus standard. It presents a **Cognitive Security Verification Framework (CSVF)**: a verification-oriented set of control concepts, testing methods, metrics, and evidence expectations designed to strengthen existing institutions rather than replace them. OWASP, NIST, and MITRE already provide much of the surrounding ecosystem. This framework should be understood as a living document.

OWASP's LLM Top 10 and OWASP's GenAI Data Security Risks and Mitigations Guide identify major risk classes and practical mitigations, but they are not themselves full procurement-grade verification regimes. NIST AI RMF 1.0 provides voluntary governance structure and risk-management vocabulary. MITRE ATLAS grounds AI security work in adversary tactics, techniques, mitigations, and case studies, but it is primarily a threat-informed knowledge base rather than a normative assurance framework. CSVF is therefore best understood as a **contribution layer**: it proposes concrete verification artifacts, measurement ideas, and evidence expectations that can be inserted into those existing frameworks.

The central claim is that existing frameworks do not yet fully operationalize one particular problem: whether an LLM-enabled system can combine individually permitted fragments into a conclusion that policy would treat as prohibited. CSVF brings those pieces together as a verification problem centered on boundary claims, permitted joins, statement-level prohibitions, repeatable testing, and evidence for assurance.

CSVF is also intentionally provisional. The proposed metrics — Domain Inference Risk (DIR), Leakage Event Rate (LER), and Crawl-Resilience Score (CRS) — should be treated as draft verification concepts rather than finished measures. They are included because a verification framework without metrics would remain abstract, but substantial work remains to formalize definitions, standardize test protocols, establish thresholds, and validate these measures across real deployments.

### Primary Research Question

> What would a practical, adoptable standard look like for controlling semantic leakage and cross-domain inference in LLM-enabled systems?

### Sub-Questions

1. How should organizations define and document "boundaries" for LLM systems, including where joins across domains are allowed or prohibited?
2. How can organizations specify what must not become reachable, including derived conclusions, not only verbatim secrets?
3. What technical and governance controls are required across retrieval, context assembly, tool use, memory, and output validation?
4. What measurement and testing regime is credible, repeatable, and suitable for procurement and assurance?
5. How can a new standard gain legitimacy and adoption in an ecosystem already shaped by NIST, MITRE, and OWASP?

---

## Background

### Semantic Leakage and Reachability

Traditional information security assumes separation of data works: data sits in compartments (HIPAA vs. non-HIPAA, export-controlled vs. public, proprietary vs. customer-facing), and access control at the boundaries largely determines what a user can know. LLM-enabled systems weaken that assumption because they convert scattered text into a single medium that can be searched, summarized, and recombined. The exposure is therefore not limited to verbatim leakage; we must now also consider **semantic leakage**, where the system reconstructs protected meaning from individually permissible fragments.

That phenomenon is not entirely new. Humans have always been able to piece together sensitive conclusions from scattered clues. Intelligence analysis and investigative journalism, for instance, have always rested on the idea that separate harmless facts can become sensitive when combined. A skilled analyst could infer a military deployment window from shipping records, weather patterns, maintenance notices, and public statements, even if none of those items was classified by itself. The difference is that this traditionally happened at human speed and required time, expertise, and usually a fairly narrow set of actors.

Existing safeguards partly reflect that older world. Organizations try to address the problem through classification rules, need-to-know restrictions, compartmentation, and insider threat programs. In practice, they also rely on friction — that friction slows down synthesis and reduces the number of people who can realistically perform it.

LLMs make the problem worse because they dramatically reduce the cost of synthesis. They can search, summarize, and recombine across large volumes of text in seconds. They also do this in workflows that feel normal and low-friction to users. A person no longer has to manually read hundreds of emails, logs, and reports to infer a sensitive conclusion. The model can do so at scale. As context windows grow, retrieval improves, and tools connect models to more repositories, the practical barriers that once limited inference are weakened further. What used to require a skilled human analyst now becomes accessible to ordinary users through routine prompting. That is why CSVF treats the problem as one of **reachability**.

### Defining "Reachable"

In CSVF, **reachable** refers to the set of conclusions an AI system can reliably produce from its underlying information substrate under real operating conditions. The substrate includes what the system can draw on through prompts, logs, connected repositories, and so forth. The central question has moved from "who can open which file" to "what conclusions become realistically derivable once the system can traverse and synthesize across the organization's information substrate."

### Exfiltration vs. Unauthorized Domain Reach

CSVF treats semantic leakage as two distinct failure modes:

- **Exfiltration risk** — the unauthorized movement of protected information from the user's domain into an LLM system or LLM-enabled workflow, especially third-party or otherwise ungoverned model planes. The typical failure mode is a user pasting sensitive material into a cloud assistant because it is the fastest way to draft, summarize, translate, or debug.
- **Unauthorized domain reach risk** — when an LLM-enabled system crosses an epistemic boundary by outputting conclusions functionally equivalent to higher-domain information even without explicit authorized access to higher-domain files.

### Relevant Regulatory Frameworks

CSVF is not a proposed privacy statute. It is a security and assurance standard meant to create a legible standard of care for information leakage and boundary erosion in LLM-enabled systems. The standard is designed to be usable inside existing legal frameworks that already care about disclosure and confidentiality, including:

- HIPAA
- 42 CFR Part 2
- GLBA
- FERPA
- GDPR principles
- CCPA / CPRA
- FTC Section 5 enforcement posture
- Trade secret law
- Export control regimes including deemed exports and ITAR technical data

CSVF's thesis is that these regimes already impose confidentiality duties, but they do not define what it means to prevent semantic leakage and unauthorized domain reach risk. CSVF supplies that missing layer.

### Relevant Organizations

**OWASP (primary client).** The Open Worldwide Application Security Project is the primary client for CSVF because it influences what engineers actually test and ship. The OWASP Top 10 for LLM Applications and the OWASP Data Security Risks and Mitigations explicitly include categories like Sensitive Information Disclosure and Insecure Plugin Design, which map cleanly onto CSVF's exfiltration and tool/agent control families.

**NIST.** The National Institute of Standards and Technology provides the governance vocabulary and lifecycle framing already used across industry and government. The NIST AI Risk Management Framework (AI RMF 1.0) supplies widely adopted risk functions and outcomes that CSVF can align to while adding a more control-oriented, inference-specific operational layer.

**MITRE.** MITRE ATLAS (Adversarial Threat Landscape for Artificial-Intelligence Systems) provides adversary-grounded tactics and techniques against AI-enabled systems. CSVF is designed to complement ATLAS by translating threat realism into boundary claims, test methods, and evidence requirements that can be assessed.

### Methodology

This framework was developed using qualitative methods aimed at producing an implementable standard. Development included direct engagements with:

- 12 academics in cybersecurity, AI, and policy
- 15 industry practitioners ranging from engineers to Chief Information Security Officers (CISOs)
- 6 leading contributors to major standards bodies

These engagements informed the control choices in CSVF, especially where legacy controls break down under inference and cross-domain joins.

In parallel, comparative case studies of existing standards and best practice regimes were conducted, focusing on how they achieve legitimacy and adoption and how they translate high-level principles into concrete controls, tests, and evidence requirements.

Practitioner context was built through direct participation in the security community, including Tel Aviv Cyber Week and AI Week, Stanford DEF CON, Lonestar Application Security Conference (LASCON), MITRE ATT&CKcon, and discussions with security engineers, red teamers, standards contributors, and platform owners at these events.

---

## Findings

### Primary Question: What Would a Practical, Adoptable Standard Look Like?

The central finding is that a practical standard for LLM-enabled systems must govern two distinct but related failure modes:

1. **Exfiltration** — where protected data or protected meaning leaves the boundary.
2. **Unauthorized domain reach** — where a system reaches a prohibited out-of-domain conclusion by combining otherwise permitted material.

Existing frameworks identify important parts of this problem, but none provides a full assurance model for:

- defining domain boundaries
- specifying prohibited joins
- identifying conclusions that must remain unreachable
- testing whether those conclusions become reachable in practice
- documenting the results in a form usable for audit, procurement, and risk acceptance

The Cognitive Security Verification Framework is best understood as a **drop-in operational layer** for the current standards ecosystem. In OWASP terms, it provides a boundary and assurance layer for disclosure, retrieval, agency, tool use, and reconstruction risks. In NIST terms, it clarifies how organizations can govern, map, measure, and manage inference risk. In MITRE-style terms, it turns attack realism into concrete mitigations, metrics, and evidence artifacts.

### Sub-Question 1: How Should Organizations Define and Document Boundaries?

Organizations should define boundaries through three core artifacts:

1. **Domain Inventory and Join Matrix** — identifies the major information domains in a system and specifies which combinations are allowed, prohibited, or approval-gated.
2. **Unreachable Statement Classes (USCs)** — catalogs the conclusions that must not become reachable.
3. **Boundary Enforcement Map** — states where enforcement actually occurs across retrieval, context assembly, tool use, memory, and output validation.

This shifts the organization away from vague references to "sensitive information" and toward a concrete model of what the system may and may not derive.

### Sub-Question 2: How Can Organizations Specify What Must Not Become Reachable?

Organizations should specify this through **Unreachable Statement Classes** rather than through keyword blocking or document labels alone.

In LLM systems, the protected outcome should not be thought of as an exact string but as a class of strings that are semantically similar to a protected idea. If an organization defines "secret" only as an exact text match, it is governing token space while the real harm occurs in meaning space. Unreachable Statement Classes force the organization to identify the conclusions that must remain unreachable even when the model has access to fragments that are individually permissible. That makes the standard responsive to both semantic exfiltration and unauthorized domain reach.

### Sub-Question 3: What Technical and Governance Controls Are Required?

CSVF answers this with layered controls and with a clear separation between those that primarily reduce exfiltration and those that primarily reduce unauthorized domain reach.

On the exfiltration side, the core controls are:
- output validation and semantic leakage guardrails
- canary and honeytoken instrumentation

The broader governance principle is that organizations must act **upstream**, not only downstream. Once sensitive material has entered vector stores or fine-tuning corpora without proper boundary control, the organization is already operating from a weaker position. The model should operate only within permissions granted by an outside control plane — never as the final authority on who may know what. An agent should not be given administrator privileges.

### Sub-Question 4: What Measurement and Testing Regime Is Credible?

For exfiltration, the key metrics are:
- **Leakage Event Rate (LER)** — the rate at which seeded protected secrets or protected meaning appear in outputs, weighted by materiality.
- **Crawl-Resilience Score (CRS)** — the resilience of the system against persistent multi-session extraction attempts.

For unauthorized domain reach, the key metric is:
- **Domain Inference Risk (DIR)** — how often a system derives an out-of-domain conclusion using only in-domain inputs under defined boundary conditions.

The proposed metrics — DIR, LER, and CRS — should be understood as draft verification measures rather than finalized industry metrics. More work is required to finalize definitions, normalize scoring, establish adversary protocols, determine acceptable thresholds, and validate the measures across multiple deployment types and sectors. At present, they should be treated as provisional.

### Sub-Question 5: How Can CSVF Gain Legitimacy and Adoption?

Achieving organizational buy-in requires fitting CSVF into the existing standards ecosystem rather than pretending it can supersede that ecosystem overnight. A deliberate choice was made to frame contributions as drop-ins to current standards and workflows so that the project could earn credibility by being useful to institutions in the language they already use.

CSVF's immediate value is as an operational layer that can be incorporated into existing governance structures:

- For **CISOs**: converts a vague fear about "AI leakage" into a structured control and testing regime.
- For **engineers**: clarifies what to build and where the boundary is enforced.
- For **compliance and legal teams**: produces artifacts that make reasonable-measures arguments more legible.
- For **procurement teams**: creates concrete vendor requirements rather than vague appeals to trust.
- For **executives**: makes clear that the alternative to a usable standard is not freedom, but uncontrolled exposure.

---

## Evidence Packs

At minimum, the assurance pack should function as a single evidentiary record that the system's cognitive boundaries are defined, enforced, and monitored over time.

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

## Mitigations

No mitigation can fully reverse a leak once protected information has entered a model or has been widely exposed through prompts, outputs, or derivative artifacts. After a leak has been discovered, the practical objectives are: (1) reduce the probability of repeated leakage going forward, and (2) limit the operational, legal, and economic damage from the current incident.

### Upstream Classification to Prevent Future Similar Leaks

The most important mitigation is to move protection earlier in the workflow. If sensitive information is only recognized at the moment of output, the organization is already acting too late. Future prevention requires upstream classification before information enters vector stores, prompt contexts, or fine-tuning corpora. Sensitive data should be tagged according to a clear taxonomy that includes regulated, privileged, trade-secret, export-controlled, and classified categories where applicable.

Upstream classification presents the challenge of **join-anticipation**. Organizations usually know that some single domains are sensitive. What is difficult to anticipate is which combinations of otherwise permitted material will become dangerous once a model can synthesize across them. A support-ticket domain may seem harmless, and an infrastructure-logging domain may seem harmless, but the join between them may reveal a system weakness.

Several practical strategies follow:

1. Ambiguous or unlabeled material should default toward quarantine or restricted ingestion rather than inclusion in general-purpose AI workflows.
2. High-risk domains should pass through ingestion gates that can require human review or automated redaction.
3. Organizations should run simulation exercises and adversarial tests specifically designed to discover unexpected joins.
4. They should maintain narrower pilot environments for new connectors and data sources before allowing them into broad production workflows.
5. They should use provenance metadata so that when an impermissible conclusion does become reachable, the organization can trace which domains and joins contributed to that failure.

### Revaluation of the Organization if Part of Its Value Was Based on Leaked Intellectual Property

If the leaked information includes valuable intellectual property, the organization may need to reassess its economic and strategic position. Some firms derive a meaningful share of their value from proprietary methods, designs, source code, or business processes that are difficult to replicate precisely because they are secret. If those assets become reachable through an LLM, the organization should consider whether part of its valuation depended on exclusivity that no longer exists in the same form.

Management should revisit assumptions about defensibility, licensing value, and competitive moat. In some cases, the appropriate response may include revised revenue forecasts, changes to go-to-market strategy, or legal action to reinforce trade secret claims where possible.

### Requesting Removal from the LLM Owner

Where the leak involves a hosted model owned and controlled by a vendor, one possible mitigation is to request removal, suppression, or isolation of the protected information — including removal from fine-tuning datasets or purge actions in connected vector stores and caches. The organization should make a formal request supported by evidence, identify the affected content as precisely as possible, and seek written confirmation of what actions the vendor can and cannot perform.

**Important limits:**

- Not every vendor can fully remove information once it has influenced model behavior.
- Some artifacts may persist in logs, backups, or derivative tuning layers.
- The organization may not be able to verify complete removal independently.
- For open models, once weights are distributed and mirrored, the possibility of centralized removal is dramatically weaker.

### Hardware and Deployment Segmentation

Another mitigation option is architectural: block access to cloud-based models for certain classes of data and require that sensitive workflows run only on locally controlled models deployed on organization-owned hardware. This reduces the risk that protected information enters a third-party processing environment at all.

Where this mitigation is adopted, it should be implemented as a controlled deployment architecture:

- network controls that block unapproved cloud LLM endpoints
- approved local inference infrastructure
- strict connector governance
- logging and telemetry for internal use
- clear rules about which domains must remain on local systems only

### Additional Mitigations

**Revocation and purge across downstream stores.** If protected information entered a vector database, cache, prompt log, memory layer, or internal fine-tuning pipeline, the organization should identify and purge those downstream copies as quickly as possible.

**Credential and architecture changes.** If leaked information includes API keys, credentials, system prompts, internal architecture details, or sensitive technical parameters, the organization should rotate credentials, rebuild exposed trust relationships, and assume the leaked information may be used in follow-on attacks.

**Honeytokens and seeded detection.** Organizations should seed high-value domains with canaries, honeytokens, or honey ideas so that leaks become easier to detect and attribute.

**Narrowing tool permissions and reducing ambient connectors.** If agents or copilots contributed to the leak, the organization should review tool scopes, connector approvals, and memory defaults.

**Legal and contractual response.** Trade secret, confidentiality, export control, and sector-specific duties may require legal review, notification decisions, or contract enforcement.

**USC and DIR retesting.** After any incident, the organization should update its Unreachable Statement Classes and re-run inference testing. A leak is evidence that either the prohibited outcome was never defined clearly enough or the boundary did not hold under realistic conditions.

**Re-segmentation of domains.** Some leaks reveal that the organization grouped too much information into a single searchable or retrievable substrate. A longer-term mitigation may require restructuring repositories, narrowing join permissions, or separating high-sensitivity material into more tightly governed systems.

---

## What Existing Frameworks Already Cover, and What CSVF Adds

| Area | What Frameworks Already Cover | CSVF's Delta |
|---|---|---|
| Governance and risk framing | OWASP 2026 covers governance, lifecycle, and classification. NIST covers governance, mapping, measurement, management, and profiles. ATLAS provides an adversary-informed structure. | Requires organizations to make explicit boundary claims about what cross-domain combinations are and are not allowed, assign ownership for those claims, and verify them with repeatable evidence. |
| Insufficiency of access control | OWASP recognizes sensitive disclosure and vector/embedding weaknesses. ABAC and RBAC answer who may access which resource. | The Domain Inventory and Join Matrix. The question shifts from "may this principal read this object?" to "may this system combine domain A, domain B, tool C, and memory D in one inferential workflow?" |
| Gaps in Data Loss Prevention | OWASP covers sensitive information disclosure and improper output handling. Existing DLP aims to stop direct leakage or unsafe outputs. | The Unreachable Statement Class concept. Instead of only asking whether a forbidden string leaves the system, CSVF asks whether the system can produce a prohibited conclusion, ranking, synthesis, or forecast even when no exact secret string appears. |
| Assurance and procurement | OWASP is helpful for builders and defenders. ATLAS is helpful for threat-informed operations. NIST provides broad structure. | The evidence pack: a compact record of boundary claims, controls, and tests. |

---

## What Is Left to Do

### Harden and Test Metrics

The next stage of CSVF development should focus on hardening the framework through measurable testing rather than only expanding its conceptual vocabulary.

**Metric Family 1: Reachability.** For each high-priority Unreachable Statement Class, organizations should measure Domain Inference Risk over repeated test runs and trend it over time, segmented by system boundary, model version, and prompt template.

**Metric Family 2: Control performance.** Organizations should track:
- join-policy violations prevented
- ambiguous ingestions quarantined
- percentage of in-scope repositories carrying valid classification labels
- number of high-sensitivity joins requiring exception approval
- time to revoke or purge downstream copies after discovery of leaked material

**Metric Family 3: Test realism.** CSVF should encourage regular red-team and adversary-emulation exercises that include paraphrase, multi-step prompting, retrieval chaining, tool use, and cross-session memory effects.

CSVF should also include **release-gate expectations** for high-scrutiny USC categories. A system that makes a classified-adjacent or strategically critical USC reachable should fail its release gate outright.

### Open Sourcing for Community Input

**Advantages:**
- Community input accelerates refinement from practitioners across red teams, platform engineering, GRC, procurement, and sector-specific environments.
- Open development improves defensibility — a framework criticized and revised in public is often stronger than one developed behind closed doors.
- Community review could expose blind spots in join modeling, weaknesses in USC definitions, and unrealistic assumptions about telemetry, red-teaming, or procurement.

**Disadvantages:**
- Quality dilution — community input is valuable only if the project maintains editorial rigor.
- Premature ossification — draft language can harden too early into something treated as canonical.
- Security sensitivity — some examples, test cases, and implementation details may be too revealing if published in full.
- Governance burden — open-source legitimacy requires maintainers, versioning, issue triage, and publication discipline.

---

## Conclusion

LLM-era security requires standards that treat meaning, joins, and inference as first-class security objects. CSVF provides a structure for managing that shift across chat, retrieval-augmented generation, agentic tool use, and memory. It does so by:

- requiring explicit boundary claims through domain inventories, permitted joins, and enforcement maps
- defining what must remain unreachable through statement-level prohibitions that include derived conclusions
- testing and quantifying reachability risk through DIR-based inference testing
- packaging the resulting evidence in forms that support assurance, procurement, and audit

The paper's contribution is broader than those four pillars alone. It argues that cognitive security must begin upstream, before sensitive information is ingested into vector stores, prompt contexts, or memory layers. It argues that organizations must pay far more attention to joins, especially the difficult problem of anticipating which combinations of individually permissible domains will later produce prohibited inferences. It argues that Unreachable Statement Classes are especially useful because they force policy to name the outcomes that truly matter. It argues that post-leak response must extend beyond simple disclosure handling to include purge, containment, vendor engagement where possible, architectural revision, and in some cases strategic reassessment of the value lost when sensitive knowledge is no longer exclusive. And it argues that for the most sensitive workflows, the correct boundary may be architectural: some information should never enter a vendor-controlled model plane at all.

Finally, CSVF is best understood not as a finished answer, but as a practical starting point for making inference boundaries legible and enforceable before those boundaries disappear into the background of ordinary organizational life.

---

## Appendix A — Glossary

See [`glossary/glossary.md`](../glossary/glossary.md) for the full CSVF glossary.

---

## Appendix B — Related Writings by Author

This appendix collects related conceptual writings by the author that informed the vocabulary and framing of this framework. These writings are not offered as independent validation and do not substitute for primary sources. Where cited in the body, they are used to clarify concepts introduced here.

### Why "Ambient" LLMs Negate Policy Boundaries

*Living With LLMs Everywhere* argues that LLMs are becoming ambient: they appear inside the everyday stack, often outside the single "chat box" mental model that most governance policies still assume. The implication is that privacy and security controls that rely on intentional user behavior ("I won't paste sensitive things into ChatGPT") degrade as interfaces multiply and boundaries dissolve. It also emphasizes that risk is not limited to whether prompts are used for training; content can be retained, logged, reviewed, routed through vendors, and later reintroduced elsewhere in a "leakage cascade."

### Mapping Inference Risk

*PageRank for Inference: Mapping Reachability in LLM Systems* argues that every major computing era creates a new kind of disorder before someone builds the framework that makes it legible and usable. In the early web, PageRank helped bring order to a chaotic internet by turning an unstructured mass of pages and links into something navigable, measurable, and trustworthy enough for people to use with confidence. The same problem is now emerging in LLM systems, where the challenge is no longer just locating information but understanding what becomes reachable when models connect scattered fragments into new conclusions. CSVF aims to do for reachable information space what PageRank did for the web: impose structure on a domain that is currently powerful but poorly understood.

### Theoretical Bounds on LLM Reachability

*The Limits of LLM-Reachable Intelligence* provides a useful caution: even if we formalize "reachable" as "claims produced with justifications accepted by a verifier," the reachable set remains bounded by the verifier and is incomplete relative to the full space of truths in the domain. The practical contribution to CSVF is that it sets theoretical bounds on the limits of inference as a medium and points toward room for human contribution to information systems.

### Gaps in Meaning

*Large Language Models and Gaps in Meaning* argues that discrete token sequences live in a countable combinatorial space, while human meanings are better modeled as points on a continuous "meaning manifold." It proposes a three-space framework (token space, meaning manifold, and an "information space") and defines the information space as the subset of meanings that are both reachable by a generator and endorsed by a verifier.

For CSVF, the relevance is:

1. A system can block verbatim secrets yet still allow the meaning (the protected conclusion) to become reachable via paraphrase.
2. If "unreachable" is defined only as "no exact match," the system is governing token space while the harm occurs in meaning space.
3. Discretization and embedding approximations introduce blind spots and distortions in meaning representation, which can allow unintended inferences to slip through controls.
4. The gap between token space and the meaning manifold implies that perfect control at the string level cannot guarantee control at the semantic level.
5. Because the "information space" is defined by what a generator can produce and a verifier will accept, reachability is system-dependent.

---

*See [`works-cited.md`](../works-cited.md) for full references.*
