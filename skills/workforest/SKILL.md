---
name: workforest
description: Use when starting parallel/isolated coding-agent work in git worktrees, or when asked to create, reuse, list, release, or clean up worktrees.
---

# Workforest (wf)

`workforest` (alias `wf`) is a CLI tool designed for managing git worktrees during parallel/isolated coding-agent sessions. It prevents multiple agents from colliding on the same worktree by tracking idle vs. in-use states and enables safe reuse and cleanup of worktree environments.

## Command Reference

| Command | Description |
| :--- | :--- |
| `workforest init [--base-dir <dir>] [--default-base <branch>] [--force]` | One-time setup per-repo; writes `workforest.config.json`. |
| `workforest acquire [--task <slug>] [--branch <name>] [--base <branch>] [--owner <id>] [--json]` | Reuses an idle worktree (preferring the requested branch, else LRU) or creates a new one. Prints the absolute path to stdout. |
| `workforest release <path-or-id> [--reset] [--owner <id>]` | Marks worktree as idle. `--reset` cleans untracked files; `--owner` guards release. |
| `workforest list [--json] [--status idle\|in-use]` | Lists managed worktrees. |
| `workforest status <path-or-id> [--json]` | Shows full detail for a specific worktree. |
| `workforest clean [--older-than <duration>] [--all] [--force] [--dry-run]` | Removes idle worktrees. `--older-than` (e.g., `2h`) and `--all` (requires `--force`). |
| `workforest prune` | Reconciles tracked state with `git worktree list`. |

**Exit Codes:**
- `0`: Success
- `1`: General/runtime error
- `2`: Usage error
- `3`: Lock-acquisition timeout

## Agent Lifecycle Example

1. **Acquire**: `path=$(workforest acquire --task my-feature --owner <session_id>)`
2. **Work**: `cd $path && <do work>`
3. **Release**: `workforest release $path --owner <session_id> --reset`

## Guidance Notes

- **Ownership**: Always pass a stable `--owner` ID (e.g., your session ID) to ensure you only release worktrees you currently hold.
- **Task Slugs**: Use short kebab-case slugs for the `--task` flag.
- **Fallback**: Only use manual `git worktree add`/`remove` if the `workforest` or `wf` command is not found on your `PATH`.
- **Safety**: Never execute `workforest clean --all --force` without explicit instruction, as it can destroy active worktrees from other sessions.
- **Locking**: Exit code `3` indicates a lock is held by another process; retry after a short delay.
