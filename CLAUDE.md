# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Personal archive of rules, guides, and resources for AI coding agents (repo: fluzzy/archive). Markdown only, no executable code.

## Structure

Resources are organized by **category**. Domain-specific resources go in subdirectories within each category.

```
<category>/
├── general-resource.md        ← cross-domain
├── app/                       ← app-specific
├── frontend/                  ← frontend-specific
├── vscode-extension/          ← VS Code-specific
└── ...
```

**Categories:**

- `guides/` - How-to guides and best practices
- `rules/` - Coding rules and conventions
- `skills/` - Agent skill definitions and archives
- `mcp/` - MCP server documentation
- `tools/` - CLI tools and utilities for AI agents
- `plugins/` - Third-party Claude Code plugin archives
- `showcase/` - Configuration examples
- `cheatsheets/` - Quick references and cheat sheets
- `role/` - Claude role definitions (copy-paste prompts)

**Domains (used as subdirectories within categories):**

- `frontend/` - Frontend (React, Next.js, UI/UX)
- `app/` - App development (mobile, desktop, React Native)
- `vscode-extension/` - VS Code extension development
- `devops/` - DevOps (CI/CD, cloud, infrastructure)

## Conventions

- All content is written in English
- Every directory has a `README.md` index listing its contents
- New roles use `role/_template.md` as a starting point
- Cross-domain resources go directly under the category directory (e.g., `guides/my-guide.md`)
- Domain-specific resources go under `<category>/<domain>/` (e.g., `guides/frontend/my-guide.md`)
- Each source document (official docs, Context7, etc.) gets its own separate md file — never merge multiple sources into one
