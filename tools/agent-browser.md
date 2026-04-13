# Agent Browser

> **Source**: [vercel-labs/agent-browser](https://github.com/vercel-labs/agent-browser)
> **Author**: Vercel Labs
> **Fetched**: 2026-04-13
> **Archived**: 2026-04-13

A fast, native Rust CLI for browser automation designed specifically for AI agents. Controls browsers through the Chrome DevTools Protocol (CDP) with ref-based element selection optimized for AI workflows.

---

## Key Features

- **Native Rust implementation** — No Node.js required for the daemon
- **Ref-based element selection** — Uses deterministic references from accessibility snapshots, optimized for AI workflows
- **Multi-session support** — Run isolated browser instances simultaneously
- **Authentication persistence** — Reuse login state across sessions via profiles or state files
- **Cloud provider integration** — Works with Browserless, Browserbase, Browser Use, Kernel, and AgentCore
- **iOS support** — Control Mobile Safari via Appium
- **Real-time streaming** — WebSocket-based viewport preview for live monitoring
- **Security features** — Domain allowlists, action policies, and encryption at rest

## Installation

Global (recommended):

```bash
npm install -g agent-browser
agent-browser install
```

Also available via Homebrew, Cargo, or build from source (requires Rust).

## Core Workflow

1. Navigate to a URL: `agent-browser open <url>`
2. Get an accessibility tree snapshot: `agent-browser snapshot -i`
3. Interact using element references: `agent-browser click @e2`
4. Take screenshots or re-snapshot after changes

## Commands

### Navigation

`open`, `back`, `forward`, `reload`, `close`

### Interaction

`click`, `type`, `fill`, `select`, `check`, `upload`, `drag`, `scroll`

### Information Retrieval

`snapshot`, `screenshot`, `get text/html/value`, `is visible/enabled`

### Semantic Finding

`find role`, `find text`, `find label` with filtering options

### Waiting

Wait for elements, text patterns, URLs, or custom JS conditions

### Batch Execution

Run multiple commands efficiently to avoid startup overhead

### Network Control

Route requests, mock responses, block URLs, record HAR files

### Browser Configuration

Set viewport, device emulation, geolocation, headers, cookies

### Debugging

Trace recording, profiling, console inspection, error tracking

## Authentication

- **Chrome profile reuse** — `--profile Default`
- **Persistent profiles** — Store cookies and IndexedDB
- **Session persistence** — Optional AES-256-GCM encryption
- **Auth vault** — Credential storage

## Security

- Content boundary markers wrap page output in delimiters so LLMs can distinguish tool output from untrusted content
- Domain allowlists with wildcard support
- Action policies gating destructive operations
- Output length limits

## Cloud Providers

Connect to remote browsers instead of local instances for serverless deployment. Supports Browserless, Browserbase, Browser Use, Kernel, and AgentCore.

## Configuration

Settings defined in `agent-browser.json` at user level (`~/.agent-browser/config.json`) or project level (`./agent-browser.json`). Environment variables and CLI flags override config file values.

## Output Modes

- `--json` — Machine-readable output for agent consumption
- `--headed` — Visible browser window for debugging
