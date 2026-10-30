# skills

Reusable agent skills for Claude Code, Codex, and other clients that support the
[Agent Skills format](https://agentskills.io/specification).

## Install Laws of UX

Install for **Claude Code and Codex**, across your projects:

```sh
npx --yes skills add al3rez/skills --skill laws-of-ux --agent claude-code codex --global --yes
```

Requires Node.js 22.20 or later for the installer. The skill itself is Markdown
and needs no runtime, API keys, MCP servers, or plugins.

To choose other supported agents, including Cursor and Gemini CLI, run this from
your project and select the clients you use:

```sh
npx --yes skills add al3rez/skills --skill laws-of-ux
```

This installs into the current project. Add `--global` to install across projects.
To install only for one client, use `--agent claude-code` or `--agent codex`.

### Use it

In Claude Code:

```text
/laws-of-ux review the onboarding flow and rank the most important friction
```

In Codex:

```text
$laws-of-ux review the onboarding flow and rank the most important friction
```

Both clients can also select the skill automatically when the task matches its
description. Other clients use their own skill invocation syntax.

### Manual installation

Clone this repository and copy the **whole skill folder**, including its
references, into your client's skill directory. Run from the repository root:

```sh
# Claude Code: all projects
mkdir -p ~/.claude/skills
cp -R skills/laws-of-ux ~/.claude/skills/

# Codex: all projects
mkdir -p ~/.agents/skills
cp -R skills/laws-of-ux ~/.agents/skills/
```

For a project install, use `.claude/skills/` or `.agents/skills/` inside the target
project. Use these copy commands for a fresh install; if `laws-of-ux` already
exists, compare it first so you preserve any local edits.

Other Agent Skills clients can consume the same folder at their documented
location. `agents/openai.yaml` provides optional Codex UI metadata; the shared
instructions do not depend on it.

To update an installation managed by the skills CLI:

```sh
npx --yes skills update laws-of-ux
```

## Skills

### [laws-of-ux](skills/laws-of-ux/SKILL.md)

Review and design user journeys using the ten principles in Jon Yablonski's
*Laws of UX*, second edition (O'Reilly, 2024).

- Connect observed friction to the user's task, an applicable principle, a focused
  change, and a way to validate it.
- Cover mental models, action targets, grouping, decisions, input tolerance,
  closure, visual perception, emphasis, complexity, and responsiveness.
- Distinguish source-backed behavior from hypotheses about user impact.
- Avoid common misreadings, including a seven-item navigation cap and a 400 ms
  deadline for an entire operation.
- Preserve informed choice, useful safeguards, and the requested scope.

The [principles reference](skills/laws-of-ux/references/principles.md) includes
diagnostic questions, practical design moves, and misapplications to avoid.
The skill is an original operational synthesis based on the book. The full book
and its illustrations are not included.

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

`laws-of-ux` is client-neutral and has no machine-specific dependencies.
`zmx-worktree` is written for my setup: worktrees under `~/Work/worktrees`, Homebrew
paths, and zsh. Adjust those paths before using it on yours.

Installation references: [skills CLI](https://github.com/vercel-labs/skills),
[Claude Code skills](https://code.claude.com/docs/en/skills), and
[Codex skills](https://developers.openai.com/codex/skills/).
