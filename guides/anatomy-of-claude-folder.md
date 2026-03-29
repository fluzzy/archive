# Anatomy of the .claude/ Folder

> **Source**: [Daily Dose of Data Science](https://blog.dailydoseofds.com/p/anatomy-of-the-claude-folder), [GeekNews](https://news.hada.io/topic?id=27941)
> **Author**: Avi Chawla
> **Fetched**: 2026-03-30
> **Archived**: 2026-03-30

A comprehensive guide to the `.claude/` directory — Claude Code's control center for managing project rules, commands, permissions, and memory. Covers every component from `CLAUDE.md` to agents, with practical examples and an implementation roadmap.

---

## Two Distinct .claude Directories

The system maintains two separate `.claude` folders:

1. **Project-level** (`.claude/` in repo root) — shared team configuration, committed to git
2. **Global** (`~/.claude/` in home directory) — personal preferences and session history, machine-local only

---

## CLAUDE.md: The Core Configuration File

Whatever you write in `CLAUDE.md`, Claude will follow. This file loads directly into the system prompt at session start and guides behavior throughout conversations.

### What to Include

- Build, test, and lint commands (e.g., `npm run test`, `make build`)
- Key architectural decisions and structure
- Non-obvious project gotchas
- Import conventions and naming patterns
- File organization and module structure

### What to Avoid

- Linter or formatter configuration details
- Full documentation (link instead)
- Lengthy theoretical explanations

### Size Guideline

Keep under 200 lines. Longer files consume excessive context and reduce instruction adherence.

### Example

```
# Project: Acme API

## Commands
npm run dev          # Start dev server
npm run test         # Run tests (Jest)
npm run lint         # ESLint + Prettier check
npm run build        # Production build

## Architecture
- Express REST API, Node 20
- PostgreSQL via Prisma ORM
- All handlers live in src/handlers/
- Shared types in src/types/

## Conventions
- Use zod for request validation in every handler
- Return shape is always { data, error }
- Never expose stack traces to the client
- Use the logger module, not console.log

## Watch out for
- Tests use a real local DB, not mocks. Run `npm run db:test:reset` first
- Strict TypeScript: no unused imports, ever
```

---

## CLAUDE.local.md: Personal Overrides

Individual developers can create `CLAUDE.local.md` for personal preferences without affecting the team. Automatically gitignored, keeping personal customizations out of version control.

---

## The rules/ Folder: Modular Instructions

Once teams grow, a monolithic `CLAUDE.md` becomes unwieldy. The `.claude/rules/` directory enables modular organization:

```
.claude/rules/
├── code-style.md
├── testing.md
├── api-conventions.md
└── security.md
```

### Path-Scoped Rules

Rules can include YAML frontmatter to target specific file patterns:

```yaml
---
paths:
  - "src/api/**/*.ts"
  - "src/handlers/**/*.ts"
---
# API Design Rules

- All handlers return { data, error } shape
- Use zod for request body validation
- Never expose internal error details to clients
```

Only loads when Claude works with matching files, keeping context focused.

---

## The commands/ Folder: Custom Slash Commands

Every markdown file in `.claude/commands/` becomes a slash command. A file named `review.md` creates `/project:review`.

### Example: Code Review Command

```markdown
---
description: Review the current branch diff for issues before merging
---
## Changes to Review

!`git diff --name-only main...HEAD`

## Detailed Diff

!`git diff main...HEAD`

Review the above changes for:
1. Code quality issues
2. Security vulnerabilities
3. Missing test coverage
4. Performance concerns

Give specific, actionable feedback per file.
```

The backtick syntax with `!` prefix executes shell commands and embeds output into the prompt.

### Passing Arguments

Use `$ARGUMENTS` for dynamic input:

```markdown
---
description: Investigate and fix a GitHub issue
argument-hint: [issue-number]
---
Look at issue #$ARGUMENTS in this repo.

!`gh issue view $ARGUMENTS`

Understand the bug, trace it to the root cause, fix it, and write a
test that would have caught it.
```

Running `/project:fix-issue 234` feeds issue #234 into the prompt.

### Command Scope

- **Project commands** (`.claude/commands/`) — team-shared, shown as `/project:command-name`
- **Personal commands** (`~/.claude/commands/`) — individual use, shown as `/user:command-name`

---

## The skills/ Folder: Auto-Invoked Workflows

Unlike commands (manually triggered), skills activate automatically when Claude recognizes matching task descriptions.

### Directory Structure

```
.claude/skills/
├── security-review/
│   ├── SKILL.md
│   └── DETAILED_GUIDE.md
└── deploy/
    ├── SKILL.md
    └── templates/
        └── release-notes.md
```

### Example Skill Definition

```markdown
---
name: security-review
description: Comprehensive security audit. Use when reviewing code for
  vulnerabilities, before deployments, or when the user mentions security.
allowed-tools: Read, Grep, Glob
---
Analyze the codebase for security vulnerabilities:

1. SQL injection and XSS risks
2. Exposed credentials or secrets
3. Insecure configurations
4. Authentication and authorization gaps

Report findings with severity ratings and specific remediation steps.
Reference @DETAILED_GUIDE.md for our security standards.
```

### Key Differences from Commands

- Skills can bundle supporting files alongside `SKILL.md`
- Skills activate proactively based on task descriptions
- Skills work in isolated context windows
- Commands are single files manually invoked

---

## The agents/ Folder: Specialized Subagents

For complex tasks requiring specialized expertise, agents create isolated personas with restricted capabilities.

### Example Agent Configuration

```markdown
---
name: code-reviewer
description: Expert code reviewer. Use PROACTIVELY when reviewing PRs,
  checking for bugs, or validating implementations before merging.
model: sonnet
tools: Read, Grep, Glob
---
You are a senior code reviewer with a focus on correctness and maintainability.

When reviewing code:
- Flag bugs, not just style issues
- Suggest specific fixes, not vague improvements
- Check for edge cases and error handling gaps
- Note performance concerns only when they matter at scale
```

### Configuration Options

- **model**: Choose cheaper alternatives (Haiku) for read-only tasks
- **tools**: Restrict capabilities (e.g., a reviewer agent shouldn't write files)
- **description**: Controls when Claude invokes the agent proactively

Agents compress findings and report back, preventing main session context bloat.

---

## settings.json: Permissions and Configuration

Controls what Claude can and cannot do within your project.

### Basic Structure

```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "permissions": {
    "allow": [
      "Bash(npm run *)",
      "Bash(git status)",
      "Bash(git diff *)",
      "Read",
      "Write",
      "Edit"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(curl *)",
      "Read(./.env)",
      "Read(./.env.*)"
    ]
  }
}
```

### Permission Logic

- **Allow list**: Commands Claude executes without confirmation
- **Deny list**: Blocked entirely, no override possible
- **Unmarked items**: Claude asks for permission before proceeding

### Recommended Configuration

**Allow:**
- `Bash(npm run *)` or `Bash(make *)` for script execution
- `Bash(git *)` for read-only git operations
- `Read`, `Write`, `Edit`, `Glob`, `Grep` for file operations

**Deny:**
- `Bash(rm -rf *)` and destructive shell commands
- `Bash(curl *)` and direct network operations
- `.env` files and anything in `secrets/`

### Local Overrides

Create `settings.local.json` for personal permission changes, automatically gitignored.

---

## Global ~/.claude/ Directory

### Contents

- **CLAUDE.md** — global instructions applying across all projects
- **settings.json** — global permission defaults
- **commands/** — personal commands available everywhere
- **skills/** — personal skills for all projects
- **agents/** — personal agents across all projects
- **projects/** — session transcripts and auto-memory per project

### Auto-Memory

Claude automatically saves observations, discovered patterns, and architectural insights to `~/.claude/projects/`. These notes persist across sessions, creating continuity. Access and edit with `/memory`.

---

## Implementation Roadmap

### Phase 1: Foundation

1. Run `/init` in Claude Code to generate starter `CLAUDE.md`
2. Edit down to essentials
3. Create `.claude/settings.json` with basic allow/deny rules

### Phase 2: Optimization

1. Define one or two high-value commands (code review, issue fixing)
2. Split `CLAUDE.md` into `.claude/rules/` files as it grows
3. Add path-scoping to focus rules on relevant codebases

### Phase 3: Advanced

1. Create `~/.claude/CLAUDE.md` with personal coding principles
2. Define specialized skills for recurring complex workflows
3. Build agents for focused, resource-intensive tasks

---

## Key Insights

- **Highest-leverage file**: `CLAUDE.md` drives the most impact — get this right first
- **Context efficiency**: Modular rules and path-scoped configurations keep context focused rather than cluttered
- **Appropriate tooling**: Commands for manual workflows, skills for automatic detection, agents for isolated specialized work
- **Permission safety**: Balance automation (allow list) with safety (deny list and implicit confirmation)
- **Plain Claude caution**: Large codebases benefit from agent toolkits, while smaller projects risk configuration bloat reducing effectiveness
