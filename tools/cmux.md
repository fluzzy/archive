# cmux — Native macOS Terminal for AI Coding Agents

> **Source**: [manaflow-ai](https://github.com/manaflow-ai/cmux)
> **Homepage**: [cmux.com](https://cmux.com/)
> **Fetched**: 2026-05-20
> **Archived**: 2026-05-20

A native macOS terminal app built on libghostty (Swift + AppKit), purpose-built for running multiple AI coding agents in parallel. Replaces tmux-style multiplexing with a GUI-native vertical-tabs sidebar, agent-aware notifications, and an embedded scriptable browser.

---

## What It Is

cmux ("Ghostty-based macOS terminal with vertical tabs and notifications for AI coding agents") is positioned as an alternative to terminal multiplexers like tmux for developers managing **terminal-based agentic workflows** — running Claude Code, Codex, Aider, etc. across many tabs/panes/branches simultaneously.

It is **not** a Ghostty fork. cmux uses libghostty as a library for terminal rendering, the same way apps use WebKit for web views.

**License**: GPL-3.0-or-later. Commercial licenses available for organizations that can't comply with GPL.

---

## Core Features

### Workspace & Tabs

- **Vertical tabs sidebar** showing per-tab metadata: git branch, PR status/number, working directory, listening ports
- Horizontal **and** vertical pane splits within tabs
- Multiple workspaces and surfaces with keyboard navigation
- Session restoration: window layout, pane configuration, working directories, terminal scrollback (best-effort), browser URL/history

### Agent-Aware Notifications

- Panes get **blue rings** and tabs highlight when an agent needs attention
- Dedicated notification panel showing all pending alerts
- Keyboard shortcut to jump to most recent unread notification
- Desktop notifications

### Embedded Browser

Split-pane browser based on the [agent-browser](./agent-browser.md) project:

- Cookie / history / session import from 20+ browsers (Chrome, Firefox, Arc, etc.)
- Accessibility-tree snapshot, form filling, JS evaluation — suitable for agent interaction
- Scriptable API
- Network-routed panes so remote workspaces can hit `localhost`

### Remote / SSH

- `cmux ssh user@remote` creates an SSH-backed workspace
- Image drag-and-drop upload via SCP

### Claude Code Integration

- Native `cmux claude-teams` command spawns teammates as native splits (no tmux dependency)
- Each teammate gets sidebar metadata (branch, ports, etc.)

### Automation

- CLI + socket API for workspace/pane automation
- Project-specific custom commands via `cmux.json`
- Reads existing Ghostty configs; cmux config at `~/.config/cmux/cmux.json`

---

## Supported Agents

> "All of them" — anything runnable from the command line.

Explicitly listed: **Claude Code, Codex, OpenCode, Gemini CLI, Kiro, Aider, Goose, Amp, Cline**, and others.

Session-resume hooks via `cmux hooks setup` work with: Claude Code, Codex, Grok, OpenCode, Pi, Amp, Cursor CLI, Gemini, Rovo Dev, Copilot, CodeBuddy, Factory, Qoder.

---

## Installation

**Homebrew**:

```bash
brew tap manaflow-ai/cmux
brew install --cask cmux
```

**DMG** (recommended for auto-updates via Sparkle): direct download from GitHub releases.

**Nightly**: separate app bundle ID, installs alongside stable.

---

## Keyboard Shortcuts

| Action | Shortcut |
| --- | --- |
| New workspace | ⌘N |
| Jump to workspace 1–9 | ⌘1–9 |
| Close workspace | ⌘⇧W |
| Toggle sidebar | ⌘B |
| Split right | ⌘D |
| Split down | ⌘⇧D |
| Open browser | ⌘⇧L |
| Address bar | ⌘L |
| Reload browser | ⌘R |
| DevTools | ⌥⌘I |
| Notifications panel | ⌘I |
| Latest unread | ⌘⇧U |
| Find | ⌘F |

Terminal keybindings inherit from Ghostty config; app-specific shortcuts customizable in Settings.

---

## Technical Stack

- **Language**: Swift + AppKit (native, no Electron)
- **Rendering**: libghostty (GPU-accelerated)
- **Platform**: macOS only
- **Auto-update**: Sparkle framework
- **i18n**: 15+ languages (Japanese, Korean, Chinese (Simplified/Traditional), German, Spanish, French, etc.)

---

## Design Philosophy

> "Give a million developers composable primitives and they'll collectively find the most efficient workflows faster than any product team could design top-down."

cmux ships **primitives** (tabs, panes, splits, browser, automation API, hooks) rather than prescribing a single agentic workflow — users compose their own.

---

## Related

- [agent-browser.md](./agent-browser.md) — the embedded browser's underlying CLI

---

## References

- Homepage — <https://cmux.com/>
- GitHub repo — <https://github.com/manaflow-ai/cmux>
