# Repository Structure

This is a snapshot of how the archive is laid out today, not a fixed schema. Categories were added as content arrived and will keep being added — when something has no natural home, a new category is the right answer, not the last resort.

## Layout

```
archive/                    # repo root
├── guides/                 # How-to guides, best practices, essays, interviews
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
│   ├── devops/
│   ├── gstack/             # named-project archives
│   └── harness/
├── mcp/                    # MCP server documentation
│   └── app/
├── tools/                  # CLI tools and utilities for AI agents (flat)
├── plugins/                # Third-party Claude Code plugin archives
│   ├── agent-skills/       # one folder per plugin
│   ├── claude-ads/
│   └── compound-engineering/
├── showcase/               # Configuration examples
│   └── app/
├── cheatsheets/            # Quick references and cheat sheets (flat)
├── role/                   # Claude role definitions (hand-written, not archived sources)
└── .claude/skills/         # Active Claude Code skills (auto-loaded)
```

## Organization

Resources are organized by **category** first. Within a category, a file sits in one of three places:

- **Category root** — cross-domain content: `guides/my-guide.md`
- **`<category>/<domain>/`** — domain-specific content: `guides/frontend/my-guide.md`
- **`<category>/<project-name>/`** — used by `skills/` and `plugins/` when the source is one identifiable project rather than a general topic: `plugins/claude-ads/claude-ads-paid-advertising-audit.md`

`tools/` and `cheatsheets/` are flat — they have no subdirectories.

**Domains** (used as subdirectories within categories):
- `frontend/` - Frontend (React, Next.js, UI/UX)
- `app/` - App development (mobile, desktop, React Native)
- `vscode-extension/` - VS Code extension development
- `devops/` - DevOps (CI/CD, cloud, infrastructure)

## README Formats

Only **category** directories have a `README.md`. Domain and project subdirectories don't — their files are indexed by the parent category README, with the folder in the link path.

**Categories with subdirectories** (`guides/`, `rules/`, `skills/`, `mcp/`, `showcase/`) — one table per domain section:

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

**Flat categories** (`tools/`, `plugins/`, `cheatsheets/`) — a single table, no sections:

```markdown
# Tools

Description.

| File | Description | Source |
| --- | --- | --- |
| [agent-browser.md](./agent-browser.md) | Description | [Vercel Labs](url) |
```

Column layout varies by category — `rules/README.md` has only `File | Description`, with no Source column. Match the table you're editing rather than imposing a shape on it.

## Conventions

- Every category directory has a `README.md` index; subdirectories do not
- The category list grows: a new top-level directory plus its `README.md` and a row in the root `README.md` Categories table is a normal addition
- File names are kebab-case: `descriptive-topic-name.md`
- Each source document gets its own separate md file
- Content is written in English, except where a Korean-language source is kept in Korean because the original phrasing is the substance (`guides/code-reading-era.md`)
