<!--
Copy this file to docs/adr/NNNN-short-title.md in your project, replace NNNN
with the next number in the sequence, and delete the comments as you fill it in.
-->

# NNNN. Short title of the decision

- **Status:** Proposed <!-- Proposed | Accepted | Rejected | Deprecated | Superseded by [ADR-0002](0002-example.md) -->
- **Date:** YYYY-MM-DD <!-- date the status last changed -->
- **Deciders:** @github-handle, @github-handle
- **Consulted / Informed:** teams or people who gave input or need to know

## Context

What is the situation that forces a decision? Describe the problem, the
constraints (technical, security, compliance, cost, timeline), and anything a
future reader needs in order to understand why this came up. State facts, not
opinions. Link to issues, designs, or incidents.

## Decision

What we decided, in the active voice: "We will ...". Be specific enough that
someone can tell whether an implementation follows this decision or not.

## Security implications

Required. If a decision genuinely does not change our security posture, write
"None - this decision does not change the threat model" and say why. Do not
delete the section.

- **Threat model:** What new trust boundaries, attack surface, or entry points
  does this create? What does an attacker gain if this component is
  compromised?
- **Data handling:** What sensitive data (credentials, tokens, keys, customer
  data) flows through, is stored by, or is logged by this? Where does it live
  and for how long?
- **Authentication and authorization:** Who or what can call this, and how is
  that enforced?
- **Supply chain:** New dependencies or vendors, their provenance, maintenance
  status, and what they can reach.
- **Residual risk:** Risk we are knowingly accepting, and who accepted it.

## Performance implications

Required, on the same terms: "None - no measurable impact expected" plus the
reason is a valid answer, but the section stays.

- **Expected impact:** Latency, throughput, memory, storage, scan time, cost.
  Give numbers or a magnitude, not "should be fine".
- **Evidence:** Benchmark, load test, production measurement, or a stated
  estimate. Say which it is and link it.
- **Scale assumptions:** The input sizes, request rates, or asset counts this
  holds for - and where it stops holding.
- **Regression budget:** Any SLO, timeout, or budget this consumes or puts at
  risk.

## Consequences

### Positive

- What gets better, easier, or safer as a result.

### Negative

- What gets harder or more expensive as a result - operational burden, added
  complexity, lock-in, migration pain. Every real decision has a cost; write it
  down honestly. Security and performance costs belong in their own sections
  above, not here.

### Follow-up

- Work this decision creates: migrations, deprecations, docs, follow-up ADRs.

## Alternatives considered

### Option A - name

Short description, and why it was not chosen.

### Option B - name

Short description, and why it was not chosen.

## References

- Links to issues, PRs, RFCs, benchmarks, vendor docs, prior ADRs.
