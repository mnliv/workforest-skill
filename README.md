# workforest-skill

Agent skill package for [`workforest`](https://github.com/mnliv/workforest), the CLI that manages reusable git worktrees for parallel coding-agent sessions.

This repo is the **source** the skill content is developed and reviewed in —
it's also vendored into the `workforest` npm package itself, so day-to-day
users install and update it via the CLI, not by cloning this repo directly
(see below).

## What this does

Add this skill to your coding agent (e.g. Claude Code) and it handles everything:

- Checks whether the `workforest`/`wf` CLI is already installed, and installs it itself if not — you never need to install the CLI by hand.
- Keeps its own installed copy in sync with whatever CLI version is present, so it never silently goes stale after you update the CLI.
- Uses it to acquire, reuse, release, and clean up git worktrees as it works, instead of colliding with other sessions on the same worktree.

You just add the skill; the agent does the rest.

## Installing the skill (Claude Code)

**Recommended:** install the CLI, then let it install the skill for you —
this is what keeps the two in sync going forward, since the skill content
ships bundled inside the CLI package and updates whenever the CLI does.

```bash
npm install -g @mnliv/workforest
workforest skill install
```

By default this installs into `~/.claude/skills/workforest` (available in
every project); pass `--target <dir>` to install into a specific project's
`.claude/skills/workforest` instead. Skills are loaded at session start, so
start a new Claude Code session afterward.

Re-running `workforest skill install` at any point (e.g. after `npm update
-g @mnliv/workforest`) resyncs it — it's a no-op if already current. The
skill's own Setup step does this automatically every time it's used, so in
practice you'll rarely need to run it by hand after the first time.

**Alternative (tracking this repo directly):** useful if you're
contributing to the skill content itself and want to test changes before
they're vendored into a release. Clone this repo and link the skill folder
in yourself:

```bash
git clone https://github.com/mnliv/workforest-skill.git
mkdir -p ~/.claude/skills
ln -s "$(pwd)/workforest-skill/skills/workforest" ~/.claude/skills/workforest
```

With this method you're responsible for keeping it current yourself
(`git pull` in the clone) — there's no automatic sync, since it's no longer
going through `workforest skill install`.

See [`skills/workforest/SKILL.md`](skills/workforest/SKILL.md) for the full instructions given to the agent.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). `main` is protected — changes land
through a reviewed pull request, and only the maintainer can merge one.
