# Contributing to ISVF

Thank you for your interest in contributing to the Idea Security Verification Framework. This is an early public draft, and community input is essential for developing it into a rigorous, adoptable, and testable standard.

---

## What Kinds of Contributions Are Useful

Valuable contributions include:

- **Clarity edits** — improve the precision or readability of existing text without changing meaning.
- **Glossary terms** — propose new terms or refine existing definitions in [`glossary/glossary.md`](glossary/glossary.md).
- **New or refined controls** — propose additions or modifications to the [`controls/control-catalog.md`](controls/control-catalog.md).
- **Metric refinements** — improve the definitions, test protocols, or thresholds in any of the [`metrics/`](metrics/) documents.
- **Test cases** — contribute concrete USC test cases, DIR test scenarios, LER canary patterns, or CRS extraction scenarios.
- **Implementation examples** — provide worked examples of domain inventories, join matrices, USC catalogs, or evidence packs.
- **Sector-specific examples** — healthcare, financial, government, defense, technology, and other sectors all have distinct control and compliance contexts.
- **Crosswalk improvements** — refine or expand the OWASP, NIST, and MITRE ATLAS crosswalks in [`crosswalks/`](crosswalks/).
- **Framework critiques** — identify weak or unrealistic assumptions, missing threat patterns, or gaps in coverage.
- **Red-team scenarios** — contribute adversarial test patterns, including paraphrase attacks, multi-step inference, retrieval chaining, cross-session memory exploitation, and tool-assisted inference.
- **Regulatory alignment** — align ISVF controls to HIPAA, FERPA, GLBA, ITAR, GDPR, CCPA, FedRAMP, or other regulatory frameworks.

---

## Contribution Principles

Before contributing, please read these principles:

**Practical.** Contributions should be useful to engineers, CISOs, auditors, procurement teams, and policy owners. Avoid purely theoretical additions without implementation guidance.

**Auditable.** Contributions should focus on evidence, controls, tests, and repeatable verification. Vague best-practice language should be made concrete wherever possible.

**Framework-aligned.** Contributions should remain complementary to OWASP, NIST, MITRE, and other existing bodies. ISVF is designed as a contribution layer, not a replacement.

**Precise.** Be careful about distinguishing direct disclosure, semantic leakage, and unauthorized domain reach. Imprecision in these distinctions weakens the framework.

**Provisional where appropriate.** Draft metrics and concepts should be labeled honestly until validated. Do not overstate the confidence level of new additions.

---

## How to Submit a Contribution

1. **Open an issue first.** Before submitting a pull request, open an issue describing the proposed change. This allows discussion before implementation effort is invested.

2. **Fork the repository** and create a branch for your change.

3. **Follow the existing document structure.** Use the existing heading hierarchy, formatting conventions, and cross-reference patterns.

4. **Mark TODOs explicitly.** If your contribution leaves open questions or unfinished sections, mark them with `<!-- TODO: ... -->` comments so they are visible and actionable.

5. **Update cross-references.** If your change affects glossary terms, control IDs, metric definitions, or crosswalk entries, update the affected cross-references.

6. **Submit a pull request** with a clear description of:
   - What you changed and why
   - What remains to be done (if anything)
   - Whether any existing TODOs are addressed by your change

---

## Editorial Standards

- Write in clear, direct prose. Avoid jargon that is not defined in the glossary.
- Use the active voice where possible.
- Define terms the first time they appear in a new document.
- Use consistent control ID formats: `ISVF-GOV-01`, `ISVF-DOM-02`, etc.
- Use consistent mitigation ID formats: `ISVF.M0001`, `ISVF.M0002`, etc.
- Use consistent USC ID formats: `USC-001`, `USC-002`, etc.
- Use consistent domain ID formats: `DOM-001`, `DOM-002`, etc.

---

## What Contributions Are NOT Appropriate

- Changes that claim finality for metrics or controls that are still provisional.
- Contributions that weaken the standard's precision (e.g., replacing specific control requirements with vague best-practice language).
- Content that advocates for specific vendor products without generic alternatives.
- Contributions that are incompatible with the CC BY 4.0 license (e.g., content with restrictive copyright).

---

## License

By contributing to this repository, you agree that your contributions will be licensed under the same [Creative Commons Attribution 4.0 International License (CC BY 4.0)](LICENSE) that covers the project.

---

## Contact

For questions about the framework or the contribution process, open an issue or contact the maintainer:

**David J. Weidman**  
Founder, SenteGuard

---

<!-- TODO: Add a link to the OWASP working group or Slack channel once community infrastructure is established -->
<!-- TODO: Add a code of conduct document -->
<!-- TODO: Define a governance process for resolving disputes about proposed changes -->
<!-- TODO: Define a versioning scheme for the framework (e.g., ISVF 0.1, 1.0, etc.) -->
