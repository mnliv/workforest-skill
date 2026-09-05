# workforest-skill

Agent skill package for [`workforest`](https://github.com/mnliv/workforest), the CLI that manages reusable git worktrees for parallel coding-agent sessions.

## What this does

Add this skill to your coding agent (e.g. Claude Code) and it handles everything:

- Checks whether the `workforest`/`wf` CLI is already installed.
- Installs it itself (`npm install -g @mnliv/workforest`) if it isn't — you never need to install the CLI by hand.
- Uses it to acquire, reuse, release, and clean up git worktrees as it works, instead of colliding with other sessions on the same worktree.

You just add the skill; the agent does the rest — including installing the
`workforest` CLI itself, so there's no separate `npm install` step for you.

## Installing the skill (Claude Code)

Claude Code loads skills from `~/.claude/skills/<name>/` (available in every
project) or `<project>/.claude/skills/<name>/` (that project only). Clone
this repo and link the skill folder into whichever location you want:

```bash
git clone https://github.com/mnliv/workforest-skill.git
mkdir -p ~/.claude/skills
ln -s "$(pwd)/workforest-skill/skills/workforest" ~/.claude/skills/workforest
```

(Use `cp -r` instead of `ln -s` if you'd rather have an independent copy
than one that tracks this clone via `git pull`. For a project-only install,
target `<project>/.claude/skills/workforest` instead of the `~/.claude` path.)

Skills are loaded at session start, so start a new Claude Code session
after linking it in.

See [`skills/workforest/SKILL.md`](skills/workforest/SKILL.md) for the full instructions given to the agent.
