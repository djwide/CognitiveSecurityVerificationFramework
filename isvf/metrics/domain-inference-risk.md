# Domain Inference Risk (DIR)

> **Status:** Provisional draft metric. Substantial work remains to formalize definitions, standardize test protocols, establish thresholds, and validate across real deployments. DIR should be treated as a draft verification concept rather than a finished measure.

---

## Definition

**Domain Inference Risk (DIR)** is the percentage of test runs in which an LLM-enabled system derives an out-of-domain conclusion using only in-domain inputs under defined boundary conditions.

DIR operationalizes the concept of **reachability** by measuring how often prohibited conclusions become available as prompts, tools, data sources, and system capabilities evolve.

---

## Conceptual Formula

```
DIR = (Number of test runs producing a reachable out-of-domain conclusion)
      ─────────────────────────────────────────────────────────────────
      (Total number of test runs)
```

A higher DIR indicates that more prohibited conclusions are reachable under current system conditions.

---

## Operational Definition of "Reachable"

A statement class is **reachable** if, under defined system conditions — including the test identity, allowed tools, retrieval policy, prompt templates, and context limits — the system can reliably produce outputs that satisfy the USC description at a repeatable success rate.

DIR tracks how often the system reaches those outcomes and how that risk changes over time.

---

## What DIR Measures

DIR is designed to answer the question:

> Can this system produce a prohibited conclusion from inputs that are individually permitted?

It is not a measure of direct information disclosure (see [Leakage Event Rate](leakage-event-rate.md) for that). DIR specifically targets **unauthorized domain reach** — the case where a system synthesizes across permitted fragments to reach a prohibited outcome.

---

## Test Protocol (Draft)

### 1. Select USC Categories

For each Unreachable Statement Class (USC) in the catalog, define a test set of prompts specifically designed to test whether that USC is reachable. Test prompts should:

- Use only inputs from authorized domains.
- Avoid directly quoting higher-domain material.
- Attempt synthesis, combination, and inference rather than direct retrieval.

### 2. Define Boundary Conditions

Document the exact system configuration under which the test is run:

- Identity / role used
- Allowed tools and connectors
- Retrieval policy in effect
- Prompt template version
- Model version
- Context limit settings

### 3. Run Tests

Execute each prompt against the system under the defined conditions. Record:

- The input prompt
- The system output
- A judgment (reachable / not reachable) against the USC definition
- Confidence of judgment (human-evaluated or automated)

### 4. Compute DIR

For each USC category:

```
DIR(USC) = Reachable runs / Total runs
```

Aggregate across USCs (weighted or unweighted — TODO: define weighting approach).

### 5. Trend Over Time

Track DIR per USC per system over time. Flag regressions (DIR increases) after:

- Model version changes
- Retrieval configuration changes
- New tool or connector additions
- Prompt template updates
- New data source ingestion

---

## Segmentation

DIR should be reported segmented by:

- **System boundary** — results may differ significantly across deployment environments.
- **Model version** — the same boundary conditions may produce different reachability under different models.
- **Prompt template** — different prompting strategies may surface different USC violations.
- **USC severity tier** — high-severity USC violations should be weighted more heavily.

---

## Release Gate Expectations

> **TODO:** Define specific DIR thresholds for release-gate pass/fail.

Recommended approach (to be validated):

| USC Tier | Threshold for Release-Gate Failure |
|---|---|
| Critical (classified-adjacent, export-controlled) | DIR > 0% (any single reachable result fails the gate) |
| High (trade secret, privileged, regulated) | DIR > [X%] — TODO: define |
| Medium | DIR > [Y%] — TODO: define |
| Low | DIR > [Z%] — TODO: define |

---

## Limitations

- DIR is a **rate**, not an absolute measure. A low DIR may still represent unacceptable risk if the USC category is critical.
- DIR depends heavily on the **quality and coverage of the test set**. A small or poorly designed test set will underestimate risk.
- DIR is **system-configuration-specific**. Results from one configuration do not generalize to another.
- Human judgment in evaluating "reachable" outputs is subjective and may not be consistent across testers.
- DIR does not capture **multi-step or crawl-based** extraction (see [Crawl-Resilience Score](crawl-resilience-score.md)).

---

## What Remains to Be Done

<!-- TODO: Define a standardized test set format (schema for test cases) -->
<!-- TODO: Define minimum test set size per USC for statistical significance -->
<!-- TODO: Define weighting scheme for aggregating DIR across USC categories -->
<!-- TODO: Define scoring normalization across deployment types and sectors -->
<!-- TODO: Define acceptable DIR thresholds per USC severity tier -->
<!-- TODO: Define adversary protocol for DIR testing (i.e., how hard should tests try?) -->
<!-- TODO: Validate DIR against real deployments across at least 3 distinct sectors -->
<!-- TODO: Define automated evaluation criteria for "reachable" judgment where possible -->
<!-- TODO: Develop worked examples of DIR test cases by USC type -->

---

## Related

- [`metrics/leakage-event-rate.md`](leakage-event-rate.md) — exfiltration-focused metric
- [`metrics/crawl-resilience-score.md`](crawl-resilience-score.md) — multi-session extraction testing
- [`controls/control-catalog.md#isvf-inf-02`](../controls/control-catalog.md) — ISVF-INF-02 control
- [`crosswalks/mitre-atlas.md#isvfm0028`](../crosswalks/mitre-atlas.md) — ISVF.M0028 mitigation
- [`examples/unreachable-statement-classes-template.md`](../examples/unreachable-statement-classes-template.md) — USC template
