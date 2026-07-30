# ISVF Glossary

This glossary defines terms used throughout the Idea Security Verification Framework (ISVF). Definitions are provisional and subject to community refinement.

---

## Terms

**Ambient AI**
The condition in which LLMs are embedded across ordinary organizational tools and workflows rather than confined to a single visible chat interface. Ambient AI degrades governance policies that rely on intentional user behavior because the interfaces multiply and the boundaries dissolve. Content can be retained, logged, reviewed, routed through vendors, and later reintroduced elsewhere in a "leakage cascade." *(See Appendix B of the framework document.)*

**Boundary Enforcement Map**
A diagram showing the system's information domains, the boundaries between them, and the permitted or prohibited joins. It visualizes how data and meaning can flow across domains and where combinations occur. The map marks enforcement points such as retrieval, context assembly, tools, memory, and outputs. It makes the system's idea boundaries explicit and auditable rather than implicit.

**Cloud Prompting Governance**
Policies and controls governing when organizational data may be sent to vendor-controlled or third-party model environments. Includes classification of which data domains may never be sent to consumer or unsanctioned external models, requirements for approved pathways for sensitive workflows, and recognition of ambient copilots and browser assistants as connectors that can create unauthorized joins across domains.

**Crawl-Resilience Score (CRS)**
A draft metric for how well a system resists repeated or multi-session extraction attempts over time. CRS measures the success rate of exfiltration workflows across sessions, prompt variations, and time, and is used to validate that controls remain effective under persistent probing. *(Provisional — see [`metrics/crawl-resilience-score.md`](../metrics/crawl-resilience-score.md).)*

**Cross-Domain Inference**
The production of a prohibited conclusion by combining fragments from multiple domains that are individually permitted but jointly sensitive.

**Domain**
A logically distinct information space, such as HR, legal, R&D, export-controlled engineering, customer support, or finance.

**Domain Inference Risk (DIR)**
A draft metric measuring how often a system derives an out-of-domain conclusion using only in-domain inputs under defined boundary conditions. DIR operationalizes the idea of reachability by testing whether prohibited conclusions become available as prompts, tools, sources, and system capabilities evolve. *(Provisional — see [`metrics/domain-inference-risk.md`](../metrics/domain-inference-risk.md).)*

**Domain Inventory and Join Matrix**
A structured record of domains and the combinations among them that are allowed, prohibited, or approval-gated. A "join" includes retrieval, context assembly, tool invocation, memory writes, and outputs that combine information across domains.

**Evidence Pack**
A compact set of documents, test results, approvals, and telemetry demonstrating control design, implementation, and operating effectiveness for an LLM-enabled system. At minimum, an evidence pack should show that idea boundaries are defined, enforced, tested, and monitored over time.

**Exfiltration**
The unauthorized movement of protected data or protected meaning out of the intended boundary. In ISVF, exfiltration is treated as distinct from unauthorized domain reach: exfiltration is about data leaving the boundary, while unauthorized domain reach is about prohibited conclusions being derivable within it.

**Information Substrate**
The total body of prompts, retrieved data, logs, memory, tool outputs, and connected repositories an LLM-enabled system can draw upon.

**Join**
Any combination of information across domains, whether through retrieval, tool use, context assembly, memory, or output synthesis. Join governance is a core concern of ISVF because individually permitted fragments can produce prohibited conclusions when combined.

**Leakage Event Rate (LER)**
A draft metric for how often protected information or protected meaning appears in outputs, weighted by materiality. LER is used for release-gate regression checks and trending. *(Provisional — see [`metrics/leakage-event-rate.md`](../metrics/leakage-event-rate.md).)*

**Model Plane**
The model execution environment in which prompts, context, and outputs are processed, whether local or vendor-controlled.

**Reachability**
The set of conclusions a system can reliably produce under defined operational conditions. In ISVF, a statement class is considered reachable if, under defined system conditions (identity, allowed tools, retrieval policy, prompts, context limits), the system can reliably produce outputs that satisfy a USC description at a repeatable success rate. *(See Appendix B of the framework document for theoretical background.)*

**Reachability Drift**
Change over time in what conclusions become reachable due to model updates, retrieval changes, tool changes, connector growth, or policy shifts. Reachability drift is why monitoring and regression testing are required after any system change.

**Semantic Leakage**
Disclosure of protected meaning through paraphrase, translation, summarization, or abstraction without necessarily disclosing the original text verbatim. Semantic leakage is one of the two primary failure modes ISVF addresses, alongside unauthorized domain reach.

**Session Information Budget**
A cap on how much sensitive material — or which combinations of domains — may enter a single session context. Session information budgets are a specific mitigation against long-context assembly risks and iterative extraction workflows even when each single retrieval appears permissible.

**Unauthorized Domain Reach**
When an LLM-enabled system crosses an epistemic boundary by outputting conclusions functionally equivalent to higher-domain information even without explicit authorized access to higher-domain files.

**Unreachable Statement Class (USC)**
A category of conclusions, claims, syntheses, or inferences defined by their closeness to protected information that policy requires the system not to make reachable. USCs are used to drive tests, policy gates, and assurance claims. They differ from keyword blocks or document labels in that they define the *semantic outcome* that must remain prohibited rather than an exact string.

---

<!-- TODO: Add additional terms as the framework matures, particularly around: -->
<!-- TODO: - USC severity tiers or classification levels -->
<!-- TODO: - Specific retrieval control vocabulary (e.g., retrieval-by-proxy) -->
<!-- TODO: - Agent-specific terms (e.g., tool scope, connector governance) -->
<!-- TODO: - Audit and evidence terminology aligned with SOC 2 / AICPA Trust Services Criteria -->

---

*Back to framework: [`isvf/idea-security-verification-framework.md`](../isvf/idea-security-verification-framework.md)*
