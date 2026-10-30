---
name: zmx-worktree
description: "Create a git worktree and persistent zmx session using local sessionizer conventions, optionally launching a requested coding client such as Claude Code or Codex. Use for zmx-backed worktrees and session setup, not ordinary git operations or unrelated agent delegation."
---

# zmx worktree and session

Create one worktree and one flat persistent session for the requested repository.
Claude Code, Codex, or another shell-capable agent can use these instructions.
Launch a coding client inside the session only when requested, using the user's
chosen client and settings.

## Establish conventions

Read `~/.local/bin/zmx-sessionizer` if present: it is the source of truth for
local naming, worktree roots, and picker behavior. Do not install or modify it
or invoke its interactive picker merely to use this skill.

Inspect `git worktree list`, `git status`, `zmx version`, and `zmx help`. Respect
the requested source checkout and base ref. Require git and zmx, plus the requested
coding client only if launching one. Report missing dependencies rather than
installing them implicitly.

Preserve PATH; add Homebrew or `~/.local/bin` prefixes only where needed. Never
name a zsh variable `path`, which is tied to PATH.

Read [references/compatibility.md](references/compatibility.md) for directory-field
changes, legacy command caveats, and the nested-Claude transcript workaround.

## Create the worktree

When the user or local sessionizer has not specified different conventions:

- Root: `~/Work/worktrees`, respecting `WT_DIR` and `WORK` overrides.
- Branch: `wt-YYYYMMDD-<petname>`, falling back to a timestamp suffix.
- Directory: `<root>/<repo>--<branch>`.
- Session: the directory basename with `.` and `:` replaced by `_`.
- One flat session per directory; the terminal or tmux owns layout.

Derive names from the intended source checkout:

```sh
repo_root=$(git rev-parse --show-toplevel)
repo=$(basename "$repo_root")
worktree_root=${WT_DIR:-"${WORK:-$HOME/Work}/worktrees"}
branch="wt-$(date +%Y%m%d)-$(petname 2>/dev/null || date +%H%M%S)"
wt="$worktree_root/${repo}--${branch}"
session=$(basename "$wt" | tr '.:' '__')
```

Check the branch, directory, and normalized session name for collisions. Retry
with a fresh suffix; stop after 20 failed candidates and report the collision.
Never reuse an unrelated session merely because its name matches.

Create the parent directory, then run `git worktree add -b "$branch" "$wt"` from
the intended checkout, including the user's base ref if specified. Diagnose a
failure before creating a session. If mise is in use, follow the repository's
trust policy for its configuration; a mise failure does not remove the worktree.

## Create and optionally launch

Change the invoking shell's directory before creating the session. Prefer detached
`run` from automation so setup does not switch the user's attached terminal.

For a session without a requested coding client:

```sh
cd "$wt"
zmx run "$session" -d cd "$wt"
```

Inspect its actual state. A completed detached task does not by itself prove
the session shell is live or dead. Older setups may show an ended session that
a later attach revives.

If a coding client was requested, start with a persistent login shell instead of
creating an ended session and killing it to replace it:

```sh
cd "$wt"
zmx run "$session" -d /bin/zsh -l
```

Use an installed login shell appropriate to the platform if zsh is unavailable.
Verify shell readiness with a fresh session listing, process tree, and bounded
history read. Then send only the requested launch command:

```sh
# Claude Code
printf 'claude\r' | zmx send "$session"

# Codex
printf 'codex\r' | zmx send "$session"
```

These are alternatives, not commands to run together. For another requested
client, verify its installed command and use the same shell-first pattern.
Do not infer permission-bypass flags from this skill. Preserve settings already
authorized by the user. For nested Claude persistence, read the reference.

## Protect existing work

- Before sending input to or killing an existing session, inspect a fresh
  `zmx list`, process tree, and recent `zmx history`. A `claude` or `codex`
  descendant, another client TUI, a generation indicator, or partially typed
  human input means the PTY is not an available idle shell.
- An idle coding-agent TUI is still not a shell. If ownership or readiness is
  unclear, report the blocker instead of sending commands or killing it.
- Do not kill sessions with attached clients without the user's authorization.
  A completed task or stale snapshot is not evidence that a session is unused.
- For requested cleanup, verify worktree changes and session ownership. Remove
  only the authorized, unused pair; do not force-remove dirty worktrees or kill
  unrelated sessions to tidy the picker.

## Verify and hand back

Verify the worktree path and branch, the session's directory metadata for the
installed zmx version, and the requested client's boot if launched. Read bounded
history rather than attaching or waiting indefinitely. Distinguish successful
setup from a client awaiting login, trust, or other user input.

Return the worktree path, branch, session name, and `zmx attach <session>` command.
The default detach shortcut is `ctrl+\`; `ZMX_NO_DETACH_KEY` can disable it.
