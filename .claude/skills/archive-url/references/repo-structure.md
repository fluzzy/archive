# Repository Structure

## Layout

```
archive/                    # repo root
├── frontend/               # Frontend (React, Next.js, UI/UX)
│   ├── skills/
│   ├── rules/
│   ├── guides/
│   ├── showcase/
│   └── README.md           # domain index with sub-category sections
├── backend/                # Backend (API, DB, infrastructure)
├── app/                    # App development (mobile, desktop)
├── devops/                 # DevOps (CI/CD, cloud, infrastructure)
├── data/                   # Data (ML, analytics, pipelines)
├── vscode-extension/       # VS Code extension development
├── role/                   # Claude role definitions
├── guides/                 # Cross-domain guides and best practices
├── mcp/                    # Cross-domain MCP server docs
├── rules/                  # Cross-domain coding rules
├── showcase/               # Cross-domain configuration examples
├── skills/                 # Cross-domain agent skill definitions
└── .claude/skills/         # Active Claude Code skills (auto-loaded)
```

## Sub-categories within each domain

Each domain directory uses these sub-categories:

- `skills/` - Agent skill definitions
- `mcp/` - MCP server docs
- `rules/` - Coding rules
- `guides/` - Guides and best practices
- `showcase/` - Configuration examples

## README Formats

**Domain READMEs** (e.g., `frontend/README.md`) are organized by sub-category sections:

```markdown
# Frontend

Resources for frontend development.

## Skills

| File | Description | Source |
| --- | --- | --- |
| [file.md](./skills/file.md) | Description | [Source](url) |

## Rules

| File | Description |
| --- | --- |
| [file.md](./rules/file.md) | Description |

## Guides

| File | Description | Source |
| --- | --- | --- |

## Showcase

| File | Description |
| --- | --- |
```

Note: file paths in domain READMEs include the sub-category prefix (e.g., `./skills/file.md`, `./guides/file.md`).

**Root-level READMEs** (e.g., `guides/README.md`) have a single flat table:

```markdown
# Guides

Description.

| File | Description | Source |
| --- | --- | --- |
| [file.md](./file.md) | Description | [Source](url) |
```

## Conventions

- Every directory has a `README.md` index listing its contents
- File names are kebab-case: `descriptive-topic-name.md`
- Each source document gets its own separate md file
- All content is written in English
