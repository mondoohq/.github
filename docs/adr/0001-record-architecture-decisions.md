# 0001. Record architecture decisions

- **Status:** Accepted
- **Date:** 2026-09-10
- **Deciders:** Mondoo Engineering
- **Consulted / Informed:** All Mondoo project maintainers

## Context

Mondoo projects make architectural decisions that outlive the discussion that
produced them - a storage engine, a wire format, an authentication model.
Today that reasoning lives in pull request threads, chat, and people's heads.
New contributors cannot tell which constraints are deliberate and which are
accidents, so decisions get re-litigated or silently undone.

We need a lightweight, in-repo record that travels with the code and is
reviewed the same way code is.

## Decision

We will keep Architecture Decision Records in `docs/adr/` in each project,
using the org-wide template in the [mondoohq/.github](https://github.com/mondoohq/.github)
repository. Each significant architectural decision gets one numbered Markdown
file, proposed and discussed in a pull request, and the log is append-only:
superseded decisions are replaced by new ADRs rather than rewritten.

## Security implications

ADRs are public documents in public repositories, and they describe our
architecture in detail. That is the whole point, but it sets a rule: an ADR
never contains secrets, internal hostnames, customer names, or details of an
unpatched vulnerability. Describe the decision and the threat model; reference
the embargoed detail by ticket rather than restating it.

- **Threat model:** Unchanged. ADRs add documentation, not attack surface.
- **Data handling:** No sensitive data. See the rule above.
- **Residual risk:** Writing down architectural reasoning gives an attacker a
  clearer map of our systems. We accept this - the same information is already
  derivable from the open source code, and the benefit to contributors
  outweighs it.

## Performance implications

None - this decision adds Markdown files to repositories and does not affect
any runtime path.

## Consequences

### Positive

- The reasoning behind a decision lives next to the code it constrains and is
  versioned with it.
- Decisions get reviewed like code, with the same visibility and history.
- New contributors and future maintainers can read why, not just what.

### Negative

- Writing an ADR costs time up front, and the log goes stale if maintainers do
  not keep statuses current.
- Teams have to judge what counts as "significant"; some decisions will be
  recorded that did not need to be, and some will be missed.

### Follow-up

- Add `docs/adr/` to projects as they make their next architectural decision.
  There is no backfill effort for past decisions.

## Alternatives considered

### Option A - Keep decisions in pull request descriptions

No new process, but decisions are scattered across hundreds of PRs and are
effectively unsearchable once merged. Rejected.

### Option B - Keep an architecture wiki or shared docs

Easy to edit, but it drifts from the code, is not reviewed, and loses the
history of why something changed. Rejected.

## References

- [Michael Nygard, "Documenting Architecture Decisions"](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions)
- [ADR usage guide](README.md)
