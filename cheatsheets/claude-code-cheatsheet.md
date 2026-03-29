# Claude Code Cheat Sheet

> **Source**: [cc.storyfox.cz](https://cc.storyfox.cz/), [GeekNews](https://news.hada.io/topic?id=27806)
> **Author**: Storyfox
> **Fetched**: 2026-03-30
> **Archived**: 2026-03-30

A comprehensive A4-printable reference guide for Claude Code v2.1, covering keyboard shortcuts, slash commands, configuration, MCP servers, skills, agents, and environment variables. Auto-updates daily tracking changelog changes.

---

## Keyboard Shortcuts

### General Controls

| Shortcut | Action |
| --- | --- |
| `Ctrl+C` | Cancel current operation |
| `Ctrl+D` | End session |
| `Ctrl+L` | Clear screen |
| `Ctrl+R` | History search |

### Mode Switching

| Shortcut | Action |
| --- | --- |
| `Shift+Tab` | Cycle permission modes |
| `Alt+P` | Switch model |
| `Alt+T` | Toggle thinking mode |

### Input Prefixes

| Prefix | Action |
| --- | --- |
| `/` | Slash commands |
| `!` | Bash command execution |
| `@` | File mention |

---

## Slash Commands

Over 40 commands across categories:

- **Session**: clear, compact, resume, `/branch` (replaces fork)
- **Configuration**: model switching, theme, permissions
- **Tools**: `/mcp` server management, skill listing, agent control
- **Special**: `/plan`, `/loop`, `/voice`, `/remote-control`, side questions

---

## Memory & Files (CLAUDE.md System)

### Scopes

- **Project** — `.claude/CLAUDE.md` (committed, team-shared)
- **Personal** — `CLAUDE.local.md` (gitignored, individual)
- **Organization** — shared org-wide settings
- **Global** — `~/.claude/CLAUDE.md` (all projects)

### Auto-Loading

Memory files from `~/.claude/projects/memory/` load automatically. Cap: 25KB / 200 lines.

---

## MCP Servers

### Transport Options

- **HTTP** — remote, recommended
- **stdio** — local processes
- **SSE** — server-sent events

### Configuration Scopes

Local, project, and user-level. Manage via `/mcp` interactive UI or CLI commands.

---

## Skills & Agents

### Built-in Skills

- `/simplify` — code review and simplification
- `/batch` — parallel changes
- `/debug` — debugging assistance
- `/loop` — recurring tasks
- `/claude-api` — API reference loading

### Custom Skills

Support frontmatter for descriptions, tool permissions, model overrides, and effort levels. Located in `.claude/skills/`.

### Built-in Agents

- **Explore** — codebase exploration
- **Plan** — architecture and planning
- **General** — general-purpose tasks
- **Bash** — shell execution

---

## Workflows & Tips

- **Plan mode**: three permission states for structured planning
- **Thinking toggle**: `Alt+T` to control reasoning depth
- **Git worktrees**: isolated feature branches for parallel development
- **Voice mode**: supports 20 languages
- **Context management**: 1M token capacity, use compact to manage
- **Session continuation**: resume via CLI

---

## CLI & Headless Mode

### Key Flags

- `--bare` — headless mode (new)
- `--model` — model selection
- `--output-format` — structured output
- `--max-turns` — turn limits
- `--budget` — cost caps
- `--channels` — MCP integration for Discord/Telegram

### Headless Execution

Non-interactive execution with budget limits. Schedule via `/loop`, remote control via `/rc`.

---

## Environment Variables

| Variable | Purpose |
| --- | --- |
| `ANTHROPIC_API_KEY` | API authentication |
| `CLAUDE_CODE_EFFORT_LEVEL` | Effort level override |
| `MAX_THINKING_TOKENS` | Thinking token limit |
| `CLAUDE_CODE_SCRUB_ENV` | Subprocess environment scrubbing |

---

## Recent Updates (v2.1.84–2.1.87)

- PowerShell Windows support
- Conditional hook syntax
- VCS directory exclusions
- YAML glob list path support
- `SendMessage` automatic recovery
- `/branch` replacing `fork`

---

## Design Notes

- Auto-detects Mac/Windows keyboard shortcuts
- Auto-updates via daily cron job
- Print-optimized A4 landscape layout (`Ctrl+P`)
- Mobile-compatible, no registration required
