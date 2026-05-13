## Guiding Questions 
### Primary Research Question

> What would a practical, adoptable standard look like for controlling semantic leakage and cross-domain inference in LLM-enabled systems?

### Sub-Questions

1. How should organizations define and document "boundaries" for LLM systems, including where joins across domains are allowed or prohibited?
2. How can organizations specify what must not become reachable, including derived conclusions, not only verbatim secrets?
3. What technical and governance controls are required across retrieval, context assembly, tool use, memory, and output validation?
4. What measurement and testing regime is credible, repeatable, and suitable for procurement and assurance?
5. How can a new standard gain legitimacy and adoption in an ecosystem already shaped by NIST, MITRE, and OWASP?

--------

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