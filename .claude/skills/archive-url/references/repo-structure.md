# Repository Structure

## Layout

```
archive/                    # repo root
├── guides/                 # How-to guides and best practices
│   ├── app/                # App-specific guides
│   ├── frontend/           # Frontend-specific guides
│   └── vscode-extension/   # VS Code extension guides
├── rules/                  # Coding rules and conventions
│   ├── app/
│   ├── frontend/
│   └── vscode-extension/
├── skills/                 # Agent skill definitions
│   ├── app/
│   ├── frontend/
│   └── devops/
├── mcp/                    # MCP server documentation
│   └── app/
├── tools/                  # CLI tools and utilities for AI agents
├── plugins/                # Third-party Claude Code plugin archives
├── showcase/               # Configuration examples
├── cheatsheets/            # Quick references and cheat sheets
├── role/                   # Claude role definitions
└── .claude/skills/         # Active Claude Code skills (auto-loaded)
```

## Organization

Resources are organized by **category** first, with **domain** as subdirectories:

- Cross-domain resources go directly in the category root (e.g., `guides/my-guide.md`)
- Domain-specific resources go in `<category>/<domain>/` (e.g., `guides/frontend/my-guide.md`)

**Domains** (used as subdirectories within categories):
- `frontend/` - Frontend (React, Next.js, UI/UX)
- `app/` - App development (mobile, desktop, React Native)
- `vscode-extension/` - VS Code extension development
- `devops/` - DevOps (CI/CD, cloud, infrastructure)

## README Formats

**Category READMEs** (e.g., `guides/README.md`) are organized by domain sections:

```markdown
# Guides

Description.

## General

| File | Description | Source |
| --- | --- | --- |
| [file.md](./file.md) | Description | [Source](url) |

## Frontend

| File | Description | Source |
| --- | --- | --- |
| [file.md](./frontend/file.md) | Description | [Source](url) |

## App

| File | Description | Source |
| --- | --- | --- |
| [file.md](./app/file.md) | Description | [Source](url) |
```

## Conventions

- Every directory has a `README.md` index listing its contents
- File names are kebab-case: `descriptive-topic-name.md`
- Each source document gets its own separate md file
- All content is written in English
