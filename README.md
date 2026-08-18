# skills

Claude Code skills I use, kept in one place so they travel between machines.

Drop a skill folder into `.claude/skills/` in a project, or into `~/.claude/skills/`
to make it available everywhere. Claude Code picks it up by the `name` and
`description` in the SKILL.md frontmatter.

## Skills

### [zmx-worktree](.claude/skills/zmx-worktree/SKILL.md)

Creates a git worktree plus a persistent zmx session
in one step, following the conventions my `zmx-sessionizer` script expects, and
optionally launches Claude Code inside it.

Worth reading even if you do not use zmx — it documents a few things that cost me
real time to work out:

- `zmx attach <name> <command>` silently drops the trailing command (as of 0.6.0),
  so the session lands in the daemon's cwd instead of yours.
- `start_dir` is recorded from the *client* cwd, which is how the sessionizer maps
  directories to sessions. Create a session from the wrong directory and the picker
  stops finding it.
- In zsh, naming a shell variable `path` destroys `$PATH` mid-script — it is a
  special array tied to it.
- Launching Claude from inside another Claude session needs
  `CLAUDE_CODE_FORCE_SESSION_PERSISTENCE=1`, or the child inherits the child-session
  marker and quietly stops saving transcripts.

It also carries safety rules for not clobbering a session someone (or some agent) is
working in.

## Notes

These are written for my setup: worktrees under `~/Work/worktrees`, Homebrew paths,
zsh. Adjust the paths before using them on yours.
