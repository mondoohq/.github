# Architecture Decision Records (ADRs)

An ADR is a short document that captures one significant architectural
decision, the context that forced it, and the consequences we accepted. ADRs
are how Mondoo projects answer "why is it built this way?" months or years
after the people in the room have moved on.

This directory holds the org-wide template. Every Mondoo project uses it.

## When to write one

Write an ADR when a decision is expensive to reverse or hard to infer from the
code:

- Choosing or replacing a datastore, queue, protocol, or major dependency
- Public API, schema, or wire format changes
- Authentication, authorization, tenancy, or key handling
- Service boundaries, deployment topology, or build/release architecture
- Deliberately accepting technical debt or a known limitation

Skip the ADR for reversible, local choices - a library swap behind an
interface, naming, formatting. Those belong in the PR description.

## How to use the template in your project

1. Create `docs/adr/` in your repository.
2. Copy [`adr-template.md`](adr-template.md) from this repo into it as
   `NNNN-short-title.md`, where `NNNN` is the next free number, zero padded
   (`0001`, `0002`, ...). Numbers are never reused, even for rejected ADRs.
3. Copy [`0001-record-architecture-decisions.md`](0001-record-architecture-decisions.md)
   as your project's first ADR if you are starting a new log.
4. Fill in the template, open a pull request with status `Proposed`, and let
   the discussion happen in review.
5. When the PR merges, set the status to `Accepted` and update the date.

## Changing a decision

ADRs are an append-only log, not a wiki. Do not rewrite an accepted ADR when
the world changes. Instead:

- Write a new ADR that supersedes it.
- Set the old ADR's status to `Superseded by [ADR-NNNN](NNNN-new-title.md)`.
- Link back to the old ADR from the new one.

Editing an accepted ADR is fine only for typos, broken links, and status
updates.

## Statuses

| Status | Meaning |
| --- | --- |
| `Proposed` | Under discussion in an open pull request. |
| `Accepted` | Decided. This is what we do today. |
| `Rejected` | Considered and turned down. Kept so we do not revisit it blindly. |
| `Deprecated` | No longer relevant, and nothing replaced it. |
| `Superseded by [ADR-NNNN](...)` | Replaced by a later decision. |

## Further reading

- [Michael Nygard, "Documenting Architecture Decisions"](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions)
- [MADR - Markdown Any Decision Records](https://adr.github.io/madr/)
