---
name: workforest
description: Use when starting parallel/isolated coding-agent work in git worktrees, or when asked to create, reuse, list, release, or clean up worktrees. Also use this to install the `workforest`/`wf` CLI itself if it isn't on PATH yet — the end user should never need to install it manually.
---

# Workforest (wf)

`workforest` (alias `wf`) is a CLI tool designed for managing git worktrees during parallel/isolated coding-agent sessions. It prevents multiple agents from colliding on the same worktree by tracking idle vs. in-use states and enables safe reuse and cleanup of worktree environments.

## Setup (do this first, every time this skill is used)

Check whether the CLI is already on `PATH`:

```bash
command -v workforest || command -v wf
```

If neither is found, install it yourself — do not ask the end user to do this:

```bash
npm install -g @mnliv/workforest
```

(Requires Node.js >= 18. If the global install fails with an `EACCES`/permission
error, it means npm's global prefix isn't user-writable — retry with `sudo`
on macOS/Linux, or re-run the terminal as Administrator on Windows. Don't
silently give up and fall back to manual `git worktree` commands just
because the first install attempt failed.)

Verify it worked before proceeding:

```bash
workforest --version
```

Once installed, it stays installed — this setup check is a no-op on future
invocations in the same environment.

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
- `3`: Lock-acquisition timeout (another workforest process is holding the state lock — retry after a short delay)

## Agent Lifecycle Example

1. **Acquire**: `path=$(workforest acquire --task my-feature --owner <session_id>)`
2. **Work**: `cd $path && <do work>`
3. **Release**: `workforest release $path --owner <session_id> --reset`

## Guidance Notes

- **Ownership**: Always pass a stable `--owner` ID (e.g., your session ID) to ensure you only release worktrees you currently hold.
- **Task Slugs**: Use short kebab-case slugs for the `--task` flag.
- **Reacquiring the same branch**: `--base` only takes effect when a new branch is being created, or an idle worktree is being switched onto a *different* branch than it's currently on. Reacquiring a worktree already on the requested branch never resets it, even with `--base` passed — so it's always safe to pass `--base` defensively without risk of discarding prior commits.
- **Fallback**: Only fall back to manual `git worktree add`/`remove` if installing the CLI (see Setup above) genuinely isn't possible in this environment (e.g. no npm/network access at all) — not just because the first install attempt hit a fixable error.
- **Safety**: Never execute `workforest clean --all --force` without explicit instruction, as it can destroy active worktrees from other sessions.
- **Locking**: Exit code `3` indicates a lock is held by another process; retry after a short delay.
