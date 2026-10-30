# Local and version-dependent behavior

## Directory metadata and the picker

The original `~/.local/bin/zmx-sessionizer` matches directories against `start_dir`
in `zmx list` and normalizes session names from directory basenames. Inspect the
actual script and installed command output; these are local conventions.

zmx 0.8.0 replaced `start_dir` with `cwd` and introduced directory tracking through
OSC7. A picker still expecting `start_dir` may need a separate fix. Do not silently
modify it or claim picker compatibility because session creation succeeded.
Depending on the installed version, `cwd` may track subsequent shell navigation
rather than an immutable creation path. Create from the intended directory and
check the actual metadata keys.

## Command behavior

- The original 0.6.0 setup observed `zmx attach <name> <command...>` dropping the
  trailing command and opening in the daemon's directory. Preserve this as a
  legacy caveat, not a claim about every release. Use detached `run` from the
  intended directory for this workflow.
- Starting in 0.7.0, `run` creates sessions using bash rather than the configured
  SHELL. Explicitly start the intended login shell before launching a coding TUI.
- `send` transmits raw bytes without Enter. The skill's `printf` examples append
  carriage return (`\r`) explicitly.
- Commands sent through `run` execute sequentially. Do not run setup commands
  concurrently in one PTY or use fixed sleeps as proof of readiness.
- `ZMX_SESSION` can make `attach` switch the caller's terminal. Use `run` for
  automation and return an attach command rather than moving the user's terminal.

## Nested Claude Code

The original local setup needed `CLAUDE_CODE_FORCE_SESSION_PERSISTENCE=1` when
launching Claude inside another Claude session to retain its own transcript:

```sh
printf 'CLAUDE_CODE_FORCE_SESSION_PERSISTENCE=1 claude\r' | zmx send "$session"
```

Preserve this workaround where it applies. Do not apply it to Codex or claim it
is required for every Claude version. If a nested launch is blocked, report it
rather than clearing environment markers or adding permission-bypass flags.

## Sources

- The user's local sessionizer for local naming and directory conventions.
- [zmx usage](https://github.com/neurosnap/zmx#usage) and installed `zmx help` for
  command behavior.
- [zmx changelog](https://github.com/neurosnap/zmx/blob/main/CHANGELOG.md) for the
  0.7.0 shell and 0.8.0 directory-field changes.
