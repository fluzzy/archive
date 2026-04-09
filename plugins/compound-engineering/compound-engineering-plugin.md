# Compound Engineering Plugin

> **Source**: [Every](https://github.com/EveryInc/compound-engineering-plugin)
> **Author**: Every Inc
> **Fetched**: 2026-04-09
> **Archived**: 2026-04-09

A Claude Code plugin marketplace featuring AI skills and agents that make each unit of engineering work easier than the last. 13.7k+ stars. Supports Claude Code, Cursor, OpenCode, Codex, Droid, Pi, Gemini, Copilot, Kiro, Windsurf, OpenClaw, and Qwen.

---

## Philosophy

**Each unit of engineering work should make subsequent units easier -- not harder.**

Traditional development accumulates technical debt. Every feature adds complexity. Compound engineering inverts this: 80% planning and review, 20% execution.

- Plan thoroughly before writing code
- Review to catch issues and capture learnings
- Codify knowledge so it's reusable
- Keep quality high so future changes are easy

---

## Workflow

```
Brainstorm -> Plan -> Work -> Review -> Compound -> Repeat
    ^
  Ideate (optional -- when you need ideas)
```

| Command | Purpose |
|---------|---------|
| `/ce:ideate` | Discover high-impact project improvements through divergent ideation and adversarial filtering |
| `/ce:brainstorm` | Explore requirements and approaches before planning |
| `/ce:plan` | Turn feature ideas into detailed implementation plans |
| `/ce:work` | Execute plans with worktrees and task tracking |
| `/ce:review` | Multi-agent code review before merging |
| `/ce:compound` | Document learnings to make future work easier |

`/ce:brainstorm` is the main entry point -- it refines ideas into a requirements plan through interactive Q&A, and short-circuits automatically when ceremony isn't needed. `/ce:plan` takes either a requirements doc from brainstorming or a detailed idea and distills it into a technical plan that agents (or humans) can work from.

`/ce:ideate` is used less often but can be a force multiplier -- it proactively surfaces strong improvement ideas based on your codebase, with optional steering from you.

Each cycle compounds: brainstorms sharpen plans, plans inform future plans, reviews catch more issues, patterns get documented.

---

## Installation

### Claude Code

```bash
/plugin marketplace add EveryInc/compound-engineering-plugin
/plugin install compound-engineering
```

### Cursor

```text
/add-plugin compound-engineering
```

### Other Targets (OpenCode, Codex, Droid, Pi, Gemini, Copilot, Kiro, Windsurf, OpenClaw, Qwen)

A Bun/TypeScript CLI converts the plugin to other AI coding tool formats:

```bash
# Convert to any target format
bunx @every-env/compound-plugin install compound-engineering --to <target>

# Auto-detect installed tools and install to all
bunx @every-env/compound-plugin install compound-engineering --to all
```

Supported `--to` targets: `opencode`, `codex`, `droid`, `pi`, `gemini`, `copilot`, `kiro`, `windsurf`, `openclaw`, `qwen`, `all`.

<details>
<summary>Output format details per target</summary>

| Target | Output path | Notes |
|--------|------------|-------|
| `opencode` | `~/.config/opencode/` | Commands as `.md` files; `opencode.json` MCP config deep-merged |
| `codex` | `~/.codex/prompts` + `~/.codex/skills` | Claude commands become prompt + skill pairs |
| `droid` | `~/.factory/` | Tool names mapped (`Bash`->`Execute`, `Write`->`Create`) |
| `pi` | `~/.pi/agent/` | Prompts, skills, extensions, and `mcporter.json` for MCPorter interoperability |
| `gemini` | `.gemini/` | Skills from agents; commands as `.toml` |
| `copilot` | `.github/` | Agents as `.agent.md` with Copilot frontmatter |
| `kiro` | `.kiro/` | Agents as JSON configs + prompt `.md` files; only stdio MCP servers |
| `openclaw` | `~/.openclaw/extensions/<plugin>/` | Entry-point TypeScript skill file |
| `windsurf` | `~/.codeium/windsurf/` (global) or `.windsurf/` (workspace) | Agents become skills; commands become flat workflows |
| `qwen` | `~/.qwen/extensions/<plugin>/` | Agents as `.yaml`; env vars with placeholders extracted as settings |

All provider targets are experimental.

</details>

---

## Local Development

### From local checkout

Add a shell alias so the local copy loads alongside normal plugins:

```bash
alias cce='claude --plugin-dir ~/code/compound-engineering-plugin/plugins/compound-engineering'
```

Run `cce` instead of `claude` to test changes. Production install stays untouched.

For other targets:

```bash
# from the repo root
bun run src/index.ts install ./plugins/compound-engineering --to codex
```

### From a pushed branch

Test someone else's branch without switching checkouts using `--branch`:

```bash
# Get cached clone path for a branch
bun run src/index.ts plugin-path compound-engineering --branch feat/new-agents

# Install a branch to any target
bun run src/index.ts install compound-engineering --to codex --branch feat/new-agents
```

### Shell aliases

```bash
CE_REPO=~/code/compound-engineering-plugin

ce-cli() { bun run "$CE_REPO/src/index.ts" "$@"; }

# Local checkout (active development)
alias cce='claude --plugin-dir $CE_REPO/plugins/compound-engineering'

# Pushed branch (testing PRs)
ccb() {
  claude --plugin-dir "$(ce-cli plugin-path compound-engineering --branch "$1")" "${@:2}"
}
```

---

## Config Synchronization

Sync personal Claude Code config (`~/.claude/`) to other AI coding tools:

```bash
# Sync to all detected tools (default)
bunx @every-env/compound-plugin sync

# Sync to a specific target
bunx @every-env/compound-plugin sync --target opencode
```

This syncs:
- Personal skills from `~/.claude/skills/` (as symlinks)
- Personal slash commands from `~/.claude/commands/`
- MCP servers from `~/.claude/settings.json`

Skills are symlinked (not copied) so changes in Claude Code are reflected immediately.

Supported sync targets: `opencode`, `codex`, `pi`, `droid`, `copilot`, `gemini`, `windsurf`, `kiro`, `qwen`, `openclaw`.

---

## References

- [EveryInc/compound-engineering-plugin](https://github.com/EveryInc/compound-engineering-plugin) -- Source repository
- [Compound engineering: how Every codes with agents](https://every.to/chain-of-thought/compound-engineering-how-every-codes-with-agents)
- [The story behind compound engineering](https://every.to/source-code/my-ai-had-already-fixed-the-code-before-i-saw-it)
