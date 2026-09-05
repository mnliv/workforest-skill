# Contributing

Thanks for considering a contribution to `workforest-skill`.

## How to propose a change

1. Fork the repo (or, if you've been added as a collaborator, branch directly).
2. Make your change on a branch, not `main`.
3. Open a pull request against `main`.

`main` is protected: every change lands through a reviewed pull request, and
only the maintainer ([@mnliv](https://github.com/mnliv)) can approve and
merge one — see [CODEOWNERS](.github/CODEOWNERS). You're welcome to open a
PR from anywhere; you don't need write access to this repo to do that.

## What lives here

This repo is just the agent-facing documentation for the
[`workforest`](https://github.com/mnliv/workforest) CLI — `SKILL.md` and its
`references/`. There's no code to build or test, but accuracy matters more
here than in most doc repos: an agent acts on this file directly, so a wrong
claim in it produces wrong actions, not just a wrong reading.

Before proposing a change:

- **Verify against the actual CLI**, not against memory of how it used to
  work — install the version referenced in `references/install.md` and run
  the command yourself. Several past corrections here existed only because
  someone tested a claim in `SKILL.md` against the real CLI and found it
  didn't hold anymore.
- If you're documenting new or changed behavior, it should already be
  released in `workforest` — this repo is meant to describe what an agent
  can actually rely on right now, not an upcoming change.
- Keep additions to `SKILL.md` itself short; anything that's only needed
  occasionally (a one-time setup step, a detailed troubleshooting path)
  belongs in `references/` instead, since `SKILL.md` is loaded into context
  on every single invocation of the skill.

## Reporting an inaccuracy

Open an issue naming the exact line in `SKILL.md` or a `references/` file,
what it claims, and what you observed the CLI actually do instead.
