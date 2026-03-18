# gstack: Claude Code Workflow Automation

> **Source**: [garrytan/gstack](https://github.com/garrytan/gstack)
> **Author**: Garry Tan (President & CEO, Y Combinator)
> **Fetched**: 2026-03-18
> **Archived**: 2026-03-18

gstack transforms Claude Code into a specialized multi-agent system with 13 opinionated workflow skills. Each skill assigns a distinct cognitive mode — founder thinking, engineering rigor, design critique, paranoid review, or execution focus — rather than blending them into one generic assistant. The author reports shipping 10,000-20,000 usable lines of code per day using this system.

---

## Design Philosophy

> "Planning is not review. Review is not shipping. Founder taste is not engineering rigor."

The system separates cognitive modes so each slash command summons a specialist. Targeted at engineers already using Claude Code heavily who want consistent, high-rigor workflows.

**Completeness Principle** ("Boil the Lake"): always recommend the full implementation when AI makes the marginal cost near-zero. Every option shows a completeness score (1-10) and dual time estimates (human-team vs CC+gstack time).

**Cognitive Patterns**: Plan reviews apply named mental models from industry leaders:
- `/plan-ceo-review`: 14 patterns — Bezos (one-way doors, Day 1 proxy skepticism), Chesky/Graham (founder mode), Munger (inversion), Altman (leverage obsession)
- `/plan-eng-review`: 15 patterns — Brooks (essential vs accidental complexity), Beck (make the change easy), Majors (own your code in production), Google SRE (error budgets)
- `/plan-design-review`: 12 patterns — Rams (subtraction default), Norman (time-horizon design), Gebbia (design for trust)

These name-drop frameworks so the LLM draws on deep knowledge of how these people actually think — "internalize these, don't enumerate them."

---

## Skills

| Command | Mode | Function |
|---------|------|----------|
| `/plan-ceo-review` | Founder/CEO | Rethinks problems to find the "10-star product hiding inside the request." Four modes: Expansion, Selective Expansion, Hold Scope, Reduction |
| `/plan-eng-review` | Eng Manager | Locks in architecture, data flow, ASCII diagrams, edge cases, tests. Forces hidden assumptions into the open |
| `/plan-design-review` | Senior Designer | 80-item design audit with AI Slop detection, letter grades (A-F). Report-only |
| `/design-consultation` | Design Partner | Builds complete design systems from scratch. Researches landscape, proposes creative risks, generates realistic product mockups. Design flows through all other phases |
| `/qa-design-review` | Designer + Engineer | Runs design audit then fixes findings with atomic commits and before/after screenshots |
| `/review` | Staff Engineer | Finds bugs that pass CI but fail production. Auto-fixes obvious ones. Flags completeness gaps where shortcuts cost <30 min CC time |
| `/ship` | Release Engineer | Syncs main, runs tests, bootstraps test frameworks if missing, coverage audit, pushes, opens PR |
| `/browse` | QA Engineer | Gives agent eyes — real Chromium browser, real clicks, real screenshots. ~100ms per command |
| `/qa` | QA + Fix Engineer | Tests app, finds bugs, fixes with atomic commits, auto-generates regression tests, re-verifies |
| `/qa-only` | QA Reporter | Report-only testing without code changes |
| `/setup-browser-cookies` | Session Manager | Imports cookies from real browser (Chrome, Arc, Brave, Edge) for authenticated testing |
| `/retro` | Engineering Manager | Team-aware retrospective with per-person breakdowns, shipping streaks, test health trends |
| `/document-release` | Technical Writer | Cross-references diffs against all docs (README, ARCHITECTURE, CONTRIBUTING, CLAUDE.md), updates everything that drifted |

---

## Browser Automation

gstack includes a persistent headless Chromium daemon built on Playwright and compiled with Bun (~58MB binary). The daemon model provides sub-second latency after initial startup.

### Architecture

```
Claude Code → CLI (compiled binary) → HTTP POST → Bun.serve server → CDP → Chromium (headless)
```

- **First call**: ~3s (starts Chromium daemon)
- **Subsequent calls**: ~100-200ms (HTTP round-trip)
- **Auto-shutdown**: 30 minutes idle timeout
- **Crash recovery**: Server exits immediately on Chromium crash; CLI auto-restarts on next command
- **Multi-workspace**: Each project gets isolated Chromium process, tabs, cookies, and logs via `.gstack/` in project root

### The Ref System

Instead of CSS selectors or XPath, gstack uses accessibility tree refs (`@e1`, `@e2`, `@c1`):

1. `snapshot -i` calls Playwright's accessibility tree
2. Parser assigns sequential refs: `@e1`, `@e2`, `@e3`...
3. Each ref maps to a Playwright Locator via `getByRole(role, { name }).nth(index)`
4. Agent runs `click @e3` → server resolves ref → Locator → `locator.click()`

No DOM mutation, no CSP issues, no framework conflicts. Refs are cleared on navigation; stale refs fail fast (~5ms check) with guidance to re-run `snapshot`.

Extended features:
- `--diff` (`-D`): Stores baseline, returns unified diff on next call to verify actions worked
- `--annotate` (`-a`): Injects temporary overlay divs with ref labels, takes screenshot, removes overlays
- `--cursor-interactive` (`-C`): Finds non-ARIA clickable elements (`cursor: pointer`, `onclick`, `tabindex`) as `@c1`, `@c2`...

### Command Categories

| Category | Commands |
|----------|----------|
| Navigate | `goto`, `back`, `forward`, `reload`, `url` |
| Read | `text`, `html`, `links`, `forms`, `accessibility` |
| Snapshot | `snapshot [-i] [-c] [-d N] [-s sel] [-D] [-a] [-o] [-C]` |
| Interact | `click`, `fill`, `select`, `hover`, `type`, `press`, `scroll`, `wait`, `viewport`, `upload` |
| Inspect | `js`, `eval`, `css`, `attrs`, `is`, `console`, `network`, `dialog`, `cookies`, `storage`, `perf` |
| Visual | `screenshot [--viewport] [--clip x,y,w,h] [sel\|@ref]`, `pdf`, `responsive` |
| Compare | `diff <url1> <url2>` |
| Dialogs | `dialog-accept [text]`, `dialog-dismiss` |
| Tabs | `tabs`, `tab`, `newtab`, `closetab` |
| Cookies | `cookie-import`, `cookie-import-browser` |
| Multi-step | `chain` (JSON batch) |

50+ commands total. All selector arguments accept CSS selectors, `@e` refs, or `@c` refs.

### Performance Comparison

| Tool | First call | Subsequent | Context overhead/call |
|------|-----------|------------|----------------------|
| Chrome MCP | ~5s | ~2-5s | ~2000 tokens |
| Playwright MCP | ~3s | ~1-3s | ~1500 tokens |
| **gstack browse** | **~3s** | **~100-200ms** | **0 tokens** |

### Why CLI over MCP

- **Zero token overhead**: Plain text stdout vs MCP's JSON schema framing (30,000-40,000 tokens per 20-command session)
- **No connection fragility**: No WebSocket/stdio disconnections
- **Simpler debugging**: Works with `curl`, standard HTTP

---

## Test Automation

### Test Bootstrapping

`/ship` detects if a project has no test framework and bootstraps one from scratch: detects runtime, researches best framework, installs it, writes 3-5 real tests for actual code, sets up CI/CD (GitHub Actions), creates TESTING.md.

### Regression Tests

Every `/qa` bug fix automatically generates a regression test (Phase 8e.5) that catches the exact scenario that broke. Tests include full attribution tracing back to the QA report.

### Coverage Audit

`/ship` builds a code path map from the diff, searches for corresponding tests, and produces an ASCII coverage diagram with quality stars (★★★ = edge cases + errors, ★★ = happy path, ★ = smoke test). PR body shows "Tests: 42 → 47 (+5 new)".

---

## Design Scoring

`/plan-design-review` and `/qa-design-review` provide:

- **Design Score**: A-F letter grades across 10 categories (visual hierarchy, typography, color/contrast, spacing/layout, interaction states, etc.)
- **AI Slop Score**: Detects 10 machine-generated anti-patterns (gradient hero, icon grid, uniform radius, etc.)
- **Inferred Design System**: Extracted from actual CSS
- **80-item structured audit** with automated fixes (in `/qa-design-review`)

---

## Why Bun

1. **Compiled binaries**: `bun build --compile` → single ~58MB executable, no `node_modules` at runtime
2. **Native SQLite**: Cookie decryption reads Chromium's SQLite DB directly — no `better-sqlite3`, no gyp
3. **Native TypeScript**: No compilation step for development, no `ts-node`
4. **Built-in HTTP server**: `Bun.serve()` handles ~10 routes without Express/Fastify

---

## Security Model

- **Localhost only**: HTTP server binds to `localhost`, not `0.0.0.0`
- **Bearer token auth**: Random UUID per session, state file with mode `0o600`
- **Cookie security**: Keychain access requires user approval, decryption in-memory only, DB read-only, no cookie values in logs
- **Shell injection prevention**: Hardcoded browser registry, `Bun.spawn()` with explicit argument arrays
- **Version auto-restart**: Binary version checked on each invocation; mismatches trigger automatic server restart

---

## SKILL.md Template System

SKILL.md files are generated from `.tmpl` templates at build time, not hand-maintained:

```
SKILL.md.tmpl → gen-skill-docs.ts (reads source code metadata) → SKILL.md (committed)
```

Placeholders filled from source code:

| Placeholder | What it generates |
|-------------|-------------------|
| `{{COMMAND_REFERENCE}}` | Categorized command table |
| `{{SNAPSHOT_FLAGS}}` | Flag reference with examples |
| `{{PREAMBLE}}` | Update check, session tracking, contributor mode, AskUserQuestion format |
| `{{BROWSE_SETUP}}` | Binary discovery + setup |
| `{{BASE_BRANCH_DETECT}}` | Dynamic base branch detection for PR-targeting skills |
| `{{QA_METHODOLOGY}}` | Shared QA methodology for `/qa` and `/qa-only` |
| `{{DESIGN_METHODOLOGY}}` | Shared design audit for `/plan-design-review` and `/qa-design-review` |
| `{{REVIEW_DASHBOARD}}` | Review Readiness Dashboard for `/ship` pre-flight |
| `{{TEST_BOOTSTRAP}}` | Test framework detection and bootstrap |

CI validates freshness via `gen:skill-docs --dry-run` + `git diff --exit-code`.

---

## Testing Tiers

| Tier | What | Cost | Speed |
|------|------|------|-------|
| 1 — Static | Parse `$B` commands, validate against registry | Free | <2s |
| 2 — E2E | Spawn `claude -p` subprocess, run each skill | ~$3.85 | ~20min |
| 3 — LLM-as-judge | Sonnet scores docs on clarity/completeness/actionability | ~$0.15 | ~30s |

Tier 1 runs on every `bun test`. Tiers 2+3 gated behind `EVALS=1`. E2E tests auto-select based on diff — only changed skills get tested.

---

## Multi-Session Support

gstack works with [Conductor](https://conductor.build) for 10+ parallel Claude Code sessions, each in its own isolated workspace. One session running `/qa`, another doing `/review`, a third implementing a feature — all at the same time.

- **ELI16 mode**: When 3+ sessions active, skills auto-ground the user on context
- **Isolated browsers**: Each workspace gets its own Chromium daemon on a random port (10000-60000)

---

## Key Features

- **Atomic commits**: `/qa` and `/qa-design-review` create one commit per fix (e.g., `style(design): FINDING-001`)
- **Greptile integration**: Auto-triages PR review comments — classifies as valid, already-fixed, or false positive
- **Contributor mode**: Self-improving — Claude rates its gstack experience 0-10, files reports when something isn't a 10
- **Auto-upgrade**: `/gstack-upgrade` detects stale vendored copies, syncs with backup/rollback safety

---

## Installation

**Requirements**: Claude Code, Git, Bun v1.0+

```bash
# Global install
git clone https://github.com/garrytan/gstack.git ~/.claude/skills/gstack \
  && cd ~/.claude/skills/gstack && ./setup

# Per-project install
cp -Rf ~/.claude/skills/gstack .claude/skills/gstack \
  && rm -rf .claude/skills/gstack/.git \
  && cd .claude/skills/gstack && ./setup
```

---

## References

- [GitHub Repository](https://github.com/garrytan/gstack)
- [Skill Deep Dives](https://github.com/garrytan/gstack/blob/main/docs/skills.md)
- [Architecture Documentation](https://github.com/garrytan/gstack/blob/main/ARCHITECTURE.md)
- [Browser Technical Details](https://github.com/garrytan/gstack/blob/main/BROWSER.md)
- [Contributing Guide](https://github.com/garrytan/gstack/blob/main/CONTRIBUTING.md)
- [Changelog](https://github.com/garrytan/gstack/blob/main/CHANGELOG.md)
