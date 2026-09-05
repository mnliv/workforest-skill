# workforest-skill

Agent skill package for [`workforest`](https://github.com/mnliv/workforest), the CLI that manages reusable git worktrees for parallel coding-agent sessions.

## What this does

Add this skill to your coding agent (e.g. Claude Code) and it handles everything:

- Checks whether the `workforest`/`wf` CLI is already installed.
- Installs it itself (`npm install -g @mnliv/workforest`) if it isn't — you never need to install the CLI by hand.
- Uses it to acquire, reuse, release, and clean up git worktrees as it works, instead of colliding with other sessions on the same worktree.

You just add the skill; the agent does the rest.

See [`skills/workforest/SKILL.md`](skills/workforest/SKILL.md) for the full instructions given to the agent.
