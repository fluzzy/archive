---
name: archive-url
description: "Archives external documents, blog posts, links, and PDFs into this repository as structured markdown files. Use when the user says \"archive this\", \"save this\", \"아카이빙해\", \"이거 읽고 정리해\", \"이거 추가해\", or provides a URL/document to be preserved. Also trigger when the user pastes a URL with minimal context (e.g., just a link), shares a blog post or GitHub repo link and expects it to be saved, or says things like \"add this to the archive\", \"이거 저장해\", \"읽어봐\". Handles content fetching, summarization, file placement, and README index updates."
argument-hint: "[url-or-path]"
---

# Archive

Read external content (URLs, PDFs, documents) and archive it as a well-structured markdown file in the appropriate location within this repository.

## Critical

- Each source document gets its own separate md file — NEVER merge multiple sources into one
- File names MUST be kebab-case: `descriptive-topic-name.md`
- ALWAYS update the relevant `README.md` index after creating the file
- All content MUST be written in English

## Steps

### 1. Check for Duplicates

Before fetching, check if the URL is already archived:
- `Grep` for the URL (or its domain + path) across `.md` files in the archive directories only (e.g., `guides/`, `rules/`, `mcp/`, `showcase/`, `skills/`, `tools/`, `plugins/`, `cheatsheets/`, `role/`). Exclude workspace, `.claude/`, and test directories.
- If a match is found, tell the user and ask whether to update the existing file or skip

### 2. Fetch and Read Content

- For URLs: use `WebFetch` to retrieve the content
- For GitHub repo URLs: fetch the repo's README (append `/raw/refs/heads/main/README.md` or use the raw content URL). Also check for a description on the repo page itself.
- For PDFs: use `Read` with the file path. For large PDFs (10+ pages), read in chunks using the `pages` parameter (e.g., `pages: "1-20"`). Max 20 pages per request.
- For local files: use `Read` directly
- Extract all key points, structure, examples, and recommendations

**If WebFetch fails** (403, 404, timeout, paywall): stop and tell the user the fetch failed, including the HTTP status. Ask them to paste the content directly or provide an alternative URL. Do not silently fall back to a different URL or guess the content — the user should decide what to archive.

### 3. Determine Target Location

Read `references/repo-structure.md` to understand the repository layout.

**Decision logic:**
1. Identify the content type (guide, rule, MCP doc, showcase, skill, tool)
2. Place in the matching category directory (`guides/`, `rules/`, `mcp/`, `skills/`, `tools/`, etc.)
3. Identify the domain (frontend, app, vscode-extension, devops, or cross-domain)
4. If cross-domain → place directly in the category root (e.g., `guides/my-guide.md`)
5. If domain-specific → place in `<category>/<domain>/` (e.g., `guides/frontend/my-guide.md`)

**Category mapping:**
- Technical how-to, best practices, research → `guides/`
- Coding conventions, rules, standards → `rules/`
- MCP server documentation → `mcp/`
- Configuration examples, reference implementations, workflow systems → `showcase/`
- Agent skill definitions → `skills/`

**If content does not fit any existing category**, create a new one. Examples:
- Interview/essay/opinion pieces → `essays/`
- Cheatsheets, quick references → `cheatsheets/`
- Tool comparisons, benchmarks → `benchmarks/`

When creating a new category: create the directory, add a `README.md` index, and update the root `README.md` to list it.

### 4. Create the Archive File

Follow the format in `references/archive-format.md`.

**Date fields**: Always include both `Fetched` (date content was retrieved from the source) and `Archived` (date committed to this repo). For same-session archiving, both are today's date.

**File naming**: derive from the content title or topic, kebab-case, descriptive.
- Good: `agents-md-stop-using-init.md`, `react-server-components-guide.md`
- Bad: `blog-post.md`, `article1.md`, `temp.md`

### 5. Update README Index

Find the `README.md` in the target directory and add an entry to the appropriate section.

**Important**: Domain READMEs (e.g., `frontend/README.md`) are organized by sub-category sections (`## Skills`, `## Rules`, `## Guides`, `## Showcase`). Add the entry under the matching section header, not at the bottom of the file.

Root-level READMEs (e.g., `guides/README.md`) have a single flat table. Add the entry to that table.

Match the existing table format. Typical format:

```markdown
| [filename.md](./filename.md) | Description | [Source Name](url) |
```

**Source Name**: Use the author or organization name, not the domain. Match the style of existing entries.
- Good: `[Simon Willison](url)`, `[Anthropic](url)`, `[Toss Tech](url)`
- Bad: `[simonwillison.net](url)`, `[www.anthropic.com](url)`

### 6. Commit Message

Follow the established pattern:

```
docs: Archive <short title> (<author or source>)
```

Examples from this repo:
- `docs: Archive Agentic Engineering Patterns guide (Simon Willison)`
- `docs: Archive ClawTeam agent swarm intelligence framework (HKUDS)`

### 7. Verify

- Confirm the file was created at the correct path
- Confirm the README index was updated (in the correct section for domain READMEs)
- Confirm the content follows the archive format template

If verification fails (wrong path, missing README entry, format issues), re-fetch the source content to check for discrepancies, fix the issues, verify again, then commit the fix before proceeding to push.

### 8. Push

After verifying, push to the remote repository immediately. This is an archive repo where each commit is a standalone addition, so pushing right away is safe and expected.
