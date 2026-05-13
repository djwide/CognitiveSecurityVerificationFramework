# Cognitive Security Verification Framework (CSVF)

The Cognitive Security Verification Framework (CSVF) is an open framework for defining, testing, and auditing inference boundaries in LLM-enabled systems.

CSVF addresses a growing security problem: organizations are no longer only trying to prevent direct disclosure of secrets. They must also govern what conclusions AI systems can derive, what domains those systems may combine, and what sensitive meanings can become reachable through retrieval, memory, tools, prompts, and synthesis.

This repository contains the public draft of CSVF and is open for community review, critique, and contribution.

---

## Core Idea

Traditional security asks:

> Who can access which file?

CSVF asks an additional question:

> What conclusions can this system derive once prompts, retrieved documents, tools, memory, and outputs are combined into one inferential workflow?

The framework is designed to help organizations make cognitive security boundaries explicit, testable, and auditable.

---

## Key Concepts

CSVF is built around several core ideas:

- **Semantic Leakage** — disclosure of protected meaning through paraphrase, translation, summarization, abstraction, or inference, even when the original secret text is not exposed.
- **Cross-Domain Inference** — production of a prohibited conclusion by combining fragments from multiple domains that are individually permitted but jointly sensitive.
- **Reachability** — the set of conclusions an AI system can reliably produce under defined operating conditions.
- **Unreachable Statement Classes (USCs)** — categories of conclusions that policy requires the system not to make reachable.
- **Domain Inventory and Join Matrix** — a structured record of information domains and which combinations among them are allowed, prohibited, or approval-gated.
- **Boundary Enforcement Map** — documentation of where boundaries are actually enforced, including retrieval, context assembly, tools, memory, and output handling.
- **Evidence Packs** — audit-ready artifacts showing that cognitive boundaries are defined, enforced, tested, and monitored.

---

## Proposed Metrics

CSVF introduces draft verification metrics for community refinement:

- **Domain Inference Risk (DIR)** — how often a system derives an out-of-domain conclusion using only in-domain inputs under defined boundary conditions.
- **Leakage Event Rate (LER)** — how often protected information or protected meaning appears in outputs, weighted by materiality.
- **Crawl-Resilience Score (CRS)** — how well a system resists repeated or multi-session extraction attempts over time.

These metrics are provisional. Contributions that refine definitions, testing protocols, scoring methods, and implementation patterns are especially welcome.

---

## Relationship to Existing Frameworks

CSVF is not intended to replace OWASP, NIST, MITRE ATLAS, or other AI security efforts.

It is designed as a contribution layer that can strengthen existing frameworks by adding:

- inference-boundary modeling
- permitted-join analysis
- semantic leakage testing
- reachability measurement
- evidence-pack requirements
- procurement-ready assurance artifacts

CSVF is especially relevant to LLM applications involving RAG, copilots, agentic workflows, long-context systems, memory, tool use, and cloud prompting governance.

---

## Repository Structure

```
├── README.md
├── LICENSE
├── works-cited.md
├── CONTRIBUTING.md
├── csvf/
│   └── cognitive-security-verification-framework.md
├── glossary/
│   └── glossary.md
├── controls/
│   └── control-catalog.md
├── metrics/
│   ├── domain-inference-risk.md
│   ├── leakage-event-rate.md
│   └── crawl-resilience-score.md
├── crosswalks/
│   ├── owasp.md
│   ├── nist-ai-rmf.md
│   └── mitre-atlas.md
└── examples/
    ├── domain-inventory-template.md
    ├── join-matrix-template.md
    ├── unreachable-statement-classes-template.md
    └── evidence-pack-template.md
```

This structure is provisional and may change as the framework matures.

---

## How to Contribute

Community contributions are welcome.

Useful contributions include:

- edits to improve clarity
- new glossary terms
- proposed controls
- test cases
- implementation examples
- sector-specific examples
- OWASP, NIST, MITRE, ISO, SOC 2, or regulatory crosswalks
- metric refinements
- red-team scenarios
- critiques of weak or unrealistic assumptions

Before submitting a pull request, please open an issue describing the proposed change. See [CONTRIBUTING.md](CONTRIBUTING.md) for full guidelines.

---

## Contribution Principles

CSVF should remain:

- **Practical** — useful to engineers, CISOs, auditors, procurement teams, and policy owners.
- **Auditable** — focused on evidence, controls, tests, and repeatable verification.
- **Framework-aligned** — complementary to OWASP, NIST, MITRE, and other existing bodies.
- **Precise** — careful about distinguishing direct disclosure, semantic leakage, and unauthorized domain reach.
- **Provisional where appropriate** — draft metrics and concepts should be labeled honestly until validated.

---

## Status

CSVF is an early public draft. It should not yet be treated as a finished consensus standard. The goal of open sourcing the framework is to invite community review and develop it into a more rigorous, adoptable, and testable standard over time.

---

## License

This document is licensed under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](LICENSE).

You are free to share and adapt the material for any purpose, including commercial use, provided appropriate credit is given.

If this repository later includes software, test harnesses, or machine-readable tooling, those components may be licensed separately under Apache-2.0.

---

## Maintainer

**David J. Weidman**  
Founder, SenteGuard
