# Leakage Event Rate (LER)

> **Status:** Provisional draft metric. Substantial work remains to formalize definitions, standardize test protocols, establish thresholds, and validate across real deployments. LER should be treated as a draft verification concept rather than a finished measure.

---

## Definition

**Leakage Event Rate (LER)** is the rate at which seeded protected secrets or protected meaning appears in system outputs, weighted by the materiality of the leaked information.

LER measures whether exfiltration controls are working — not only in ideal conditions, but under repeated and varied prompting.

---

## Conceptual Formula

```
LER = Σ (Leakage events weighted by materiality)
      ─────────────────────────────────────────
      Total output events evaluated
```

A higher LER indicates that protected information or meaning is appearing in outputs more frequently or with higher materiality.

---

## What LER Measures

LER is designed to answer the question:

> Is protected information or protected meaning escaping through outputs?

LER covers **two types of leakage**:

1. **Direct leakage** — the exact protected string (or a recognizable variant) appears in an output.
2. **Semantic leakage** — protected meaning appears through paraphrase, translation, summarization, abstraction, or inference, even when no verbatim secret is present.

LER does not measure unauthorized domain reach (where a system synthesizes a prohibited conclusion from permitted fragments). That is measured by [Domain Inference Risk](domain-inference-risk.md).

---

## Materiality Weighting

LER is weighted by **materiality** — not all leakage events are equal. A materiality weighting should reflect:

- The sensitivity tier of the leaked information (regulated, privileged, trade secret, export-controlled, classified).
- The potential harm if the information is disclosed (legal exposure, competitive harm, safety risk).
- Whether the leakage is direct or semantic (semantic leakage of high-confidence inferred conclusions may warrant equal or higher weight).

> **TODO:** Define a formal materiality weighting scheme. Options include:
> - Ordinal tiers (1–5) by sensitivity category
> - Continuous scoring based on regulatory risk
> - Binary critical/non-critical classification

---

## Test Protocol (Draft)

### 1. Seed Protected Information

Plant known canary values or honeytoken strings in the information substrate (vector stores, documents, memory, system prompts). Include:

- Direct canary strings (verbatim secrets)
- Semantic honeytoken "ideas" (paraphraseable concepts that are uniquely identifiable)

### 2. Run Evaluation Prompts

Execute a set of prompts designed to elicit the seeded information. Include:

- Direct retrieval prompts
- Paraphrase and reformulation prompts
- Summarization and synthesis prompts
- Multi-turn prompts that build toward the seeded information

### 3. Evaluate Outputs

For each output, determine:

- Did a direct canary string appear? (Direct leakage)
- Did semantic equivalence to a seeded honeytoken concept appear? (Semantic leakage)
- What is the materiality weight of the leaked information?

### 4. Compute LER

```
LER = Σ (materiality weight × leakage indicator) / total outputs evaluated
```

### 5. Track Over Time

Track LER per system over time. Use LER at release gates for regression checks.

---

## Segmentation

LER should be reported segmented by:

- **Leakage type** — direct vs. semantic
- **Information domain** — which domains are producing leakage
- **Output channel** — chat, API, document generation, tool output, etc.
- **Materiality tier** — high/medium/low

---

## Release Gate Expectations

> **TODO:** Define specific LER thresholds for release-gate pass/fail.

Recommended approach (to be validated):

| Leakage Type | Materiality Tier | Threshold for Release-Gate Failure |
|---|---|---|
| Direct | Critical | Any single event |
| Direct | High | LER > [X%] — TODO: define |
| Semantic | Critical | Any single event |
| Semantic | High | LER > [Y%] — TODO: define |

---

## Limitations

- LER depends entirely on the **quality and coverage of seeded canaries and honeytokens**. Unseeded information cannot be measured.
- **Semantic leakage evaluation** requires human judgment or a trained evaluator and is inherently less precise than direct string matching.
- LER measures **outputs**, not all possible leakage paths (e.g., model weight extraction, inference-time side channels).
- LER does not capture **authorized-but-risky** outputs that approach but do not reach a leakage threshold.
- High LER for semantic leakage may be a signal of USC violations that should also be tracked via [DIR](domain-inference-risk.md).

---

## What Remains to Be Done

<!-- TODO: Define a formal materiality weighting scheme -->
<!-- TODO: Define minimum canary/honeytoken density and variety for LER evaluation -->
<!-- TODO: Define standardized schema for leakage event records -->
<!-- TODO: Define evaluation criteria for semantic leakage (what counts as a leakage event?) -->
<!-- TODO: Define LER thresholds for each materiality tier and leakage type -->
<!-- TODO: Develop an automated semantic leakage evaluator or define criteria for human evaluation -->
<!-- TODO: Validate LER across at least 3 distinct deployment types -->
<!-- TODO: Define relationship between LER and DIR — when does a high LER imply a USC violation? -->

---

## Related

- [`metrics/domain-inference-risk.md`](domain-inference-risk.md) — unauthorized domain reach metric
- [`metrics/crawl-resilience-score.md`](crawl-resilience-score.md) — multi-session extraction testing
- [`controls/control-catalog.md#isvf-exf-01`](../controls/control-catalog.md) — ISVF-EXF-01 control
- [`crosswalks/mitre-atlas.md#isvfm0026`](../crosswalks/mitre-atlas.md) — ISVF.M0026 mitigation
