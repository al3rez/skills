---
name: zmx-worktree
description: Create a git worktree and persistent zmx session following the machine's zmx-sessionizer conventions (~/.local/bin/zmx-sessionizer), optionally launching Claude Code inside it. Use when asked to "open a worktree", "spin up a session", "launch claude in a worktree", or anything zmx-session related.
---

# zmx worktree + session, the sessionizer way

`~/.local/bin/zmx-sessionizer` is the source of truth for these conventions.
If this skill and that script ever disagree, re-read the script.

## Conventions (must match exactly)

- Worktree root: `~/Work/worktrees`
- Branch name: `wt-$(date +%Y%m%d)-$(petname)` — petname is at
  `/usr/local/bin/petname`; fall back to `$(date +%H%M%S)` if missing.
  Retry with a fresh petname if the branch or path already exists.
- Worktree path: `~/Work/worktrees/<repo>--<branch>`
- Session name: `basename` of the directory with `.` and `:` replaced by `_`
  — i.e. `<repo>--<branch>`, NOT the bare branch name.
- One flat session per directory. Ghostty/tmux owns layout; zmx owns
  persistence.

## Procedure

PATH first: the Bash tool's shell may lack Homebrew paths. Start every zmx
command block with `export PATH="/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:$PATH"`
(petname lives in /usr/local/bin).

zsh landmine: NEVER name a shell variable `path` — in zsh it is a special
array tied to `$PATH`, so `path="$HOME/..."` silently destroys the search
path mid-script. Use `wt` or `wt_path`.

1. **Worktree** (from the main repo checkout):
   ```sh
   repo=$(basename "$(git rev-parse --show-toplevel)")
   branch="wt-$(date +%Y%m%d)-$(petname)"
   wt="$HOME/Work/worktrees/${repo}--${branch}"
   git worktree add -b "$branch" "$wt"      # from HEAD, like the sessionizer
   ```
2. **mise trust** any of `mise.toml`, `.mise.toml`, `.tool-versions` present
   in the new worktree (`mise trust <file>`), errors ignored.
3. **Session** — `cd` into the worktree FIRST, then:
   ```sh
   cd "$wt"
   zmx run "${repo}--${branch}" -d cd "$wt"
   ```
   The session will show `ended / exit_code=0` in `zmx list` — that is
   correct: `run -d` records `start_dir`, and a later `zmx attach` revives
   it as a login shell there. Only a session meant to run an agent
   detached needs a live process (step 4).
   `start_dir` is recorded from the client cwd and is how the sessionizer
   maps directories to sessions — creating the session from the wrong cwd
   breaks the picker. NEVER use `zmx attach <name> <command>` to create:
   as of zmx 0.6.0 the trailing command is silently dropped and the session
   lands in the daemon's cwd ($HOME).
4. **Launching Claude inside** (only if asked): the step-3 session is
   `ended` — its PTY is dead, so `zmx send` to it does nothing. Recreate it
   with a persistent login shell as the command, THEN type claude into it
   (the shell survives Claude exiting):
   ```sh
   zmx kill "${repo}--${branch}" --force 2>/dev/null; sleep 1
   cd "$wt"
   zmx run "${repo}--${branch}" -d /bin/zsh -l
   sleep 2
   zmx send "${repo}--${branch}" $'CLAUDE_CODE_FORCE_SESSION_PERSISTENCE=1 claude --dangerously-skip-permissions\n'
   ```
   (Killing here is safe only because the step-3 session is known-ended and
   empty; the safety rules below still apply to any other session.)
   `CLAUDE_CODE_FORCE_SESSION_PERSISTENCE=1` matters when launching from
   inside another Claude session: the child otherwise inherits the
   child-session marker and silently stops saving transcripts.
   Only pass `--dangerously-skip-permissions` when the user asked for it.
5. **Verify** boot with `zmx history <name> | tail`, then tell the user:
   `zmx attach <name>` (detach: `ctrl+\`).

## Safety rules

- Before sending text to or killing ANY session, check whether an agent owns
  it: a `claude`/`codex` process under the session pid, "esc to interrupt"
  in `zmx history <name> | tail -c 8000` means it is mid-generation, and any
  `❯ <text>` in recent history means a human is typing there. Text sent to
  an agent-owned PTY lands inside its TUI; killing it destroys their work.
- Never kill sessions with `clients=1+` in `zmx list` without asking.
- Cleanup pairs: a worktree removed with `git worktree remove` should have
  its session killed too, and vice versa — orphans confuse the picker.
