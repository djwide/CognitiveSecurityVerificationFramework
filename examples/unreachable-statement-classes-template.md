# Unreachable Statement Classes (USC) Template

This template provides a starting structure for defining Unreachable Statement Classes (USCs) for an LLM-enabled system. Complete this document as part of ISVF control **ISVF-DOM-02**.

An **Unreachable Statement Class (USC)** is a category of conclusions, claims, syntheses, or inferences that policy requires the system not to make reachable — including derived conclusions, not only verbatim secrets.

> USCs are used to drive tests, policy gates, and assurance claims. They must be defined at the level of **semantic outcome**, not exact string match.

---

## System Identification

| Field | Value |
|---|---|
| System Name | `[System name]` |
| System Owner | `[Name / role]` |
| Idea Security Owner | `[Name / role]` |
| Document Version | `[e.g., 1.0]` |
| Last Reviewed | `[YYYY-MM-DD]` |
| Next Review Due | `[YYYY-MM-DD]` |

---

## USC Severity Tiers

<!-- TODO: Validate and finalize severity tier definitions with community input. -->

| Tier | Label | Description | Release Gate Implication |
|---|---|---|---|
| 1 | Critical | Classified-adjacent, export-controlled, or catastrophic harm potential | Any single reachable result fails release gate |
| 2 | High | Trade secret, legally privileged, regulated PII/PHI, or significant competitive harm | Low-tolerance threshold — TODO: define |
| 3 | Medium | Confidential business information, sensitive internal strategy | Moderate tolerance threshold — TODO: define |
| 4 | Low | Internal-only information, embarrassing but not harmful | Higher tolerance threshold — TODO: define |

---

## USC Entry Format

Each USC entry should specify:

| Field | Description |
|---|---|
| USC ID | Unique identifier (e.g., USC-001) |
| USC Name | Short descriptive name |
| Severity Tier | 1 (Critical) through 4 (Low) |
| Description | Plain-language description of the **semantic outcome** that must not be reachable. Describe the *class of meaning*, not an exact string. |
| Example Prohibited Output | One or more examples of outputs that would constitute a reachability violation (not exhaustive — illustrative only) |
| Related Domains | Domain IDs from the domain inventory whose combination could produce this USC |
| Related Prohibited Joins | Join matrix entries that govern the relevant domain combinations |
| Test Method | How reachability will be tested (see test method options below) |
| Test Frequency | How often DIR testing is run for this USC |
| DIR Threshold | Maximum acceptable DIR before release-gate failure |
| Owner | Named individual responsible for this USC |
| Notes | Any additional context |

### Test Method Options

| Code | Method | Description |
|---|---|---|
| TM-01 | Direct prompt testing | Direct prompts requesting the prohibited information |
| TM-02 | Synthesis testing | Prompts designed to induce the system to synthesize the prohibited conclusion from permitted fragments |
| TM-03 | Paraphrase testing | Reformulated prompts testing whether semantic equivalents of the prohibited output appear |
| TM-04 | Multi-step reasoning | Multi-turn prompts that build toward the prohibited conclusion step by step |
| TM-05 | Tool-assisted inference | Prompts that use available tools to approach the prohibited conclusion |
| TM-06 | Memory-assisted inference | Tests that exploit cross-session memory to build toward the prohibited conclusion |
| TM-07 | Cross-language testing | Prompts in multiple languages to test whether language-switching evades controls |

---

## USC Register

<!-- TODO: Replace with actual USCs for your system. Add rows as needed. -->

| USC ID | USC Name | Tier | Description | Example Prohibited Output | Related Domains | Related Joins | Test Method | Test Frequency | DIR Threshold | Owner |
|---|---|---|---|---|---|---|---|---|---|---|
| USC-001 | `[Name]` | `[1–4]` | `[Semantic description]` | `[Example output]` | `[DOM-IDs]` | `[Join pairs]` | `[TM codes]` | `[e.g., quarterly]` | `[e.g., 0%]` | `[Owner]` |
| USC-002 | | | | | | | | | | |

---

## Test Records Summary

For each USC, maintain a record of the most recent test run.

<!-- TODO: Populate after each DIR test cycle. -->

| USC ID | Test Date | Model Version | Boundary Conditions | Runs | Reachable Results | DIR | Pass/Fail | Notes |
|---|---|---|---|---|---|---|---|---|
| USC-001 | `[YYYY-MM-DD]` | `[Version]` | `[Summary]` | `[N]` | `[n]` | `[n/N]` | `[Pass/Fail]` | |

---

## Change Log

| Version | Date | Author | Summary of Changes |
|---|---|---|---|
| 1.0 | `[YYYY-MM-DD]` | `[Author]` | Initial draft |

---

<!-- TODO: Define the process for adding a new USC (who can propose, who approves) -->
<!-- TODO: Define the process for retiring a USC when it is no longer applicable -->
<!-- TODO: Define how USCs are updated after an incident where a USC was violated -->
<!-- TODO: Define the relationship between USC severity tier and the DIR threshold for that tier -->
<!-- TODO: Develop a library of example USCs for common sector use cases (healthcare, financial, government, technology) -->
<!-- TODO: Define whether USC test results constitute part of the evidence pack or a separate artifact -->

---

*See also: [`examples/domain-inventory-template.md`](domain-inventory-template.md) | [`examples/join-matrix-template.md`](join-matrix-template.md) | [`examples/evidence-pack-template.md`](evidence-pack-template.md)*  
*Related metrics: [`metrics/domain-inference-risk.md`](../metrics/domain-inference-risk.md)*  
*Related control: [`controls/control-catalog.md#csvf-dom-02`](../controls/control-catalog.md)*
