# Crawl-Resilience Score (CRS)

> **Status:** Provisional draft metric. Substantial work remains to formalize definitions, standardize test protocols, establish thresholds, and validate across real deployments. CRS should be treated as a draft verification concept rather than a finished measure.

---

## Definition

**Crawl-Resilience Score (CRS)** measures the resilience of a system against persistent, multi-session extraction attempts over time. CRS is defined as the success rate of exfiltration workflows across sessions, prompt variations, and time.

CRS specifically targets **crawl-based** extraction: adversarial patterns where an attacker makes repeated, varied attempts across multiple sessions or over extended time periods, rather than attempting a single-shot extraction.

---

## Conceptual Formula

```
CRS = 1 - (Successful multi-session extraction attempts / Total multi-session extraction attempts)
```

A higher CRS indicates stronger resilience. A CRS of 1.0 means no multi-session extraction attempt succeeded. A CRS of 0.0 means all attempts succeeded.

Alternatively, the inverse (extraction success rate) can be reported directly for clarity:

```
Extraction Success Rate = Successful attempts / Total attempts
```

> **TODO:** Decide on canonical direction (resilience score vs. success rate) for consistency with LER and DIR.

---

## What CRS Measures

CRS is designed to answer the question:

> Does the system remain resilient when an adversary tries repeatedly, across sessions, using varied prompting strategies?

Single-session extraction attempts may be blocked by rate limits, output validators, or session budgets. CRS tests whether those controls remain effective when:

- The adversary spreads attempts across multiple sessions.
- The adversary varies prompt phrasing, language, or approach.
- The adversary uses time gaps between attempts to avoid detection.
- The adversary uses intermediate steps to build toward a target conclusion.

---

## Relationship to LER and DIR

| Metric | What It Measures | Attack Pattern |
|---|---|---|
| LER | Rate of protected information appearing in outputs | Single output evaluation |
| DIR | Rate of USC violations under defined conditions | Single-session inference testing |
| CRS | Resilience against multi-session, persistent extraction | Multi-session adversarial crawl |

CRS is not a substitute for LER or DIR. A system can have a low LER (individual outputs rarely leak) and a low DIR (single-session inference rarely produces USC violations) while still having a poor CRS if persistent multi-session crawls eventually reconstruct protected information.

---

## Test Protocol (Draft)

### 1. Define Extraction Scenarios

Design multi-session extraction scenarios targeting high-priority USC categories and seeded canary information. Each scenario should specify:

- The target information or conclusion being extracted.
- The sequence of sessions and prompts.
- The prompt variation strategy (paraphrase, role-play, translation, multi-step reasoning, etc.).
- The time gap between sessions (if relevant).

### 2. Conduct Multi-Session Extraction Attempts

Execute each scenario against the system under test. Record:

- Session and prompt sequence
- Outputs at each step
- Whether the target information or conclusion was eventually produced

### 3. Evaluate Extraction Success

For each scenario, determine:

- Was the target information or conclusion successfully extracted? (Binary: yes/no)
- At which session/step did extraction succeed (if it did)?
- Which prompt variation or strategy succeeded?

### 4. Compute CRS

```
CRS = 1 - (Successful scenarios / Total scenarios)
```

### 5. Track Over Time

Track CRS per system over time. A declining CRS after a system change is a signal that multi-session extraction controls may be degrading.

---

## Segmentation

CRS should be reported segmented by:

- **Scenario type** — what attack pattern was used
- **USC category targeted** — which USC was the extraction aiming to violate
- **Session count** — how many sessions were required before success (if applicable)
- **Prompt variation strategy** — which variation strategy succeeded

---

## Release Gate Expectations

> **TODO:** Define specific CRS thresholds for release-gate pass/fail.

Recommended approach (to be validated):

| USC Tier | Threshold for Release-Gate Failure |
|---|---|
| Critical | CRS < 1.0 (any successful extraction fails the gate) |
| High | CRS < [X] — TODO: define |
| Medium | CRS < [Y] — TODO: define |

---

## Limitations

- CRS is highly **test-set dependent**. A poorly designed scenario set will overestimate resilience.
- CRS requires **adversarial creativity** — a generic set of scenarios may miss novel attack patterns.
- CRS does not measure **detection capability** — a system may have a high CRS because attacks succeed slowly, not because they are detected and stopped.
- Multi-session testing is **resource-intensive** and may not be feasible at high frequency.
- CRS may underestimate risk if the test environment differs materially from production (e.g., missing connectors, data sources, or memory).

---

## What Remains to Be Done

<!-- TODO: Define a standardized scenario library for common USC categories and system types -->
<!-- TODO: Define minimum number of scenarios per USC tier -->
<!-- TODO: Define evaluation criteria for "successful extraction" (same challenge as semantic leakage evaluation) -->
<!-- TODO: Define CRS thresholds for each USC severity tier -->
<!-- TODO: Develop tooling to automate multi-session extraction scenario execution -->
<!-- TODO: Define relationship between CRS and canary/honeytoken instrumentation (ISVF-EXF-02) -->
<!-- TODO: Validate CRS across at least 3 distinct deployment types -->
<!-- TODO: Define how CRS scenarios should be updated after a successful extraction event -->
<!-- TODO: Consider whether CRS should incorporate detection speed as a sub-score -->

---

## Related

- [`metrics/domain-inference-risk.md`](domain-inference-risk.md) — single-session unauthorized domain reach
- [`metrics/leakage-event-rate.md`](leakage-event-rate.md) — single-output exfiltration rate
- [`controls/control-catalog.md#csvf-adv-01`](../controls/control-catalog.md) — ISVF-ADV-01 control
- [`crosswalks/mitre-atlas.md#csvfm0027`](../crosswalks/mitre-atlas.md) — ISVF.M0027 mitigation
