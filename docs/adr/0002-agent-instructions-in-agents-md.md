# 0002. Keep agent instructions in AGENTS.md

- **Status:** Accepted
- **Date:** 2026-09-13
- **Deciders:** @chris-rock
- **Consulted / Informed:** All Mondoo project maintainers

## Context

Our repositories carry written instructions for coding agents: where things
live, how to build and test, which idioms are deliberate, and which mistakes
reviewers keep seeing. In cnspec that file is 200 lines and is the difference
between an agent that follows house conventions and one that reinvents them.

Those instructions were written as `CLAUDE.md`, a name specific to one tool.
The content is not specific to one tool. Every agent working in the repository
needs the same facts, and a per-tool filename leads to the outcome we already
see elsewhere: a second copy for the second tool, and the two drift.

[agents.md](https://agents.md/) is a cross-tool convention for exactly this
file. It specifies `AGENTS.md` at the repository root, and allows nested files
in subdirectories, where the closest file to the code being edited takes
precedence. That nesting matters for our larger repositories: cnspec keeps
separate instructions for the policy engine and for policy content, because the
rules genuinely differ.

The constraint is that tools already read `CLAUDE.md`, and a rename alone would
silently drop the instructions for anyone on a version that has not adopted
`AGENTS.md`. Silently, because an agent with no instructions does not fail --
it just stops following conventions it never learned.

## Decision

We will keep agent instructions in `AGENTS.md`, at the repository root, with
nested `AGENTS.md` files in subprojects where the guidance genuinely differs.

`CLAUDE.md` becomes a relative symlink to the `AGENTS.md` beside it, so tools
that look for the old name keep working and there is exactly one copy of the
content. New instruction files are created as `AGENTS.md`; the symlink exists
for compatibility, not as a second place to write.

## Security implications

Agent instruction files are read by tools that then write code and run
commands, so their contents influence privileged behaviour. That is not new --
`CLAUDE.md` had the same property -- and the control is the same: these files
live in the repository and change only through reviewed pull requests. Treat an
edit to `AGENTS.md` with the same scrutiny as an edit to CI configuration,
because both direct something that executes.

- **Threat model:** Unchanged. The file moves and gains a symlink; neither adds
  an entry point. An attacker who can modify `AGENTS.md` can already modify the
  code it describes.
- **Data handling:** No sensitive data. The same rule as ADR 0001 applies: no
  secrets, internal hostnames, or customer names. Instruction files are a
  tempting place to put "the test token is ..." and must not be.
- **Authentication and authorization:** Unchanged; repository write access
  governs both files.
- **Supply chain:** No new dependencies. Worth noting the symlink must be
  **relative and in-tree**. A symlink pointing outside the repository, or an
  absolute path, would let a checkout read content the repository does not
  contain and does not review. Reviewers should reject one.
- **Residual risk:** None beyond what `CLAUDE.md` already carried.

## Performance implications

None - this decision renames Markdown files and adds symlinks. It touches no
runtime path, no build step, and no scan.

## Consequences

### Positive

- One file serves every agent, so per-tool copies cannot drift apart.
- The convention is external and documented, so contributors do not have to
  learn a Mondoo-specific filename.
- Nested files are part of the spec rather than a local invention, and they
  already match how our larger repositories are organised.

### Negative

- **Git for Windows does not create symlinks by default.** Without
  `core.symlinks=true` and Developer Mode (or an elevated shell), `CLAUDE.md`
  checks out as a plain text file containing the string `AGENTS.md`. A tool
  reading it then gets one useless line instead of the instructions. This is
  the main cost of the symlink approach, and it is invisible: nothing errors.
  Contributors on Windows whose agent still relies on `CLAUDE.md` should either
  enable symlink support or point their tool at `AGENTS.md`.
- Two names for one file is a question every new contributor asks once.
- The symlink is a compatibility shim with no removal date. It should be
  deleted when the tools we use all read `AGENTS.md`, and nothing currently
  tracks that.

### Follow-up

- Rename the instruction files in each project as it is next touched. There is
  no coordinated migration; a repository with no `AGENTS.md` is not broken.
- Revisit the `CLAUDE.md` symlink once the tools in use read `AGENTS.md`
  directly, and delete it rather than carrying it indefinitely.

## Alternatives considered

### Option A - Rename to AGENTS.md with no symlink

The clean end state, and where we want to be. Rejected for now because the
failure mode is silent: a tool that only looks for `CLAUDE.md` finds nothing,
reports nothing, and simply stops following the repository's conventions. The
symlink costs one inode and removes that failure.

### Option B - Keep CLAUDE.md and add AGENTS.md as a second real file

Both names work everywhere, including on Windows without symlink support.
Rejected because two real files is the drift problem we are trying to avoid;
the copies would diverge within weeks and nothing would detect it.

### Option C - Keep CLAUDE.md only

No work, and it works today. Rejected because it entrenches a tool-specific
name for tool-agnostic content, and the second tool that needs the same facts
gets a second file.

## References

- [agents.md - the AGENTS.md convention](https://agents.md/)
- [ADR 0001 - Record architecture decisions](0001-record-architecture-decisions.md)
- [ADR usage guide](README.md)
