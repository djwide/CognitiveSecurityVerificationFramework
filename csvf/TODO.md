## What Is Left to Do

### Harden and Test Metrics

The next stage of ISVF development should focus on hardening the framework through measurable testing rather than only expanding its conceptual vocabulary.

**Metric Family 1: Reachability.** For each high-priority Unreachable Statement Class, organizations should measure Domain Inference Risk over repeated test runs and trend it over time, segmented by system boundary, model version, and prompt template.

**Metric Family 2: Control performance.** Organizations should track:
- join-policy violations prevented
- ambiguous ingestions quarantined
- percentage of in-scope repositories carrying valid classification labels
- number of high-sensitivity joins requiring exception approval
- time to revoke or purge downstream copies after discovery of leaked material

**Metric Family 3: Test realism.** ISVF should encourage regular red-team and adversary-emulation exercises that include paraphrase, multi-step prompting, retrieval chaining, tool use, and cross-session memory effects.

ISVF should also include **release-gate expectations** for high-scrutiny USC categories. A system that makes a classified-adjacent or strategically critical USC reachable should fail its release gate outright.

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
