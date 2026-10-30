# skills

Reusable agent skills for Claude Code, Codex, and other clients that support the
[Agent Skills format](https://agentskills.io/specification).

## Install a skill

Install for **Claude Code and Codex**, across your projects:

```sh
npx --yes skills add al3rez/skills --skill laws-of-ux --agent claude-code codex --global --yes
```

For **zmx Worktree**, use the same installer:

```sh
npx --yes skills add al3rez/skills --skill zmx-worktree --agent claude-code codex --global --yes
```

Requires Node.js 22.20 or later for the installer. Both skill packages contain
Markdown instructions and optional client metadata. `laws-of-ux` has no runtime
dependencies; `zmx-worktree` requires git and zmx on macOS or Linux, plus the
selected coding client if you ask it to launch one.

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

For zmx, use `/zmx-worktree` in Claude Code or `$zmx-worktree` in Codex. For example:

```text
/zmx-worktree create a worktree and session for this repository, then launch Claude Code
```

```text
$zmx-worktree create a worktree and session for this repository, then launch Codex
```

Omit the launch request when you only need a persistent shell session. Creating
a session does not automatically start a coding client or bypass its permissions.

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
project. To install zmx manually, replace `skills/laws-of-ux` with
`skills/zmx-worktree` in these commands. Use copy commands for a fresh install;
if the target skill already exists, compare it first so you preserve local edits.

Other Agent Skills clients can consume the same folder at their documented
location. `agents/openai.yaml` provides optional Codex UI metadata; the shared
instructions do not depend on it.

To update an installation managed by the skills CLI:

```sh
npx --yes skills update laws-of-ux
npx --yes skills update zmx-worktree
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

### [zmx-worktree](skills/zmx-worktree/SKILL.md)

Creates a git worktree plus a persistent zmx session, following a local
`zmx-sessionizer` when present. Claude Code, Codex, or another shell-capable agent
can use the skill; launching a coding client inside the session is optional.

Worth reading even if you do not use zmx — it documents a few things that cost me
real time to work out:

- The original 0.6.0 setup observed `zmx attach <name> <command>` dropping the
  trailing command. The skill uses detached `run` from the worktree directory.
- Directory metadata is version-dependent: 0.8.0 replaced `start_dir` with `cwd`.
  A picker still expecting `start_dir` needs its own compatibility check.
- In zsh, naming a shell variable `path` destroys `$PATH` mid-script — it is a
  special array tied to it.
- The original nested-Claude setup needed
  `CLAUDE_CODE_FORCE_SESSION_PERSISTENCE=1` to retain its own transcripts; this
  local workaround does not apply to Codex.

It also carries safety rules for not clobbering a session someone (or some agent) is
working in.
See [compatibility notes](skills/zmx-worktree/references/compatibility.md) for
the version caveats. The legacy `.claude/skills/zmx-worktree` path is a symlink to
the shared skill folder, keeping one source of truth.

## Notes

`laws-of-ux` is client-neutral and has no machine-specific dependencies.
`zmx-worktree` preserves my default layout under `~/Work/worktrees` while allowing
the user's local sessionizer, paths, and installed shell to define their setup.

Installation references: [skills CLI](https://github.com/vercel-labs/skills),
[Claude Code skills](https://code.claude.com/docs/en/skills), and
[Codex skills](https://developers.openai.com/codex/skills/).
