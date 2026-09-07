---
name: archive-url
description: "Archives external documents, blog posts, links, and PDFs into this repository as structured markdown files. Use when the user says \"archive this\", \"save this\", \"아카이빙해\", \"이거 읽고 정리해\", \"이거 추가해\", or provides a URL/document to be preserved. Also trigger when the user pastes a URL with minimal context (e.g., just a link), shares a blog post or GitHub repo link and expects it to be saved, or says things like \"add this to the archive\", \"이거 저장해\", \"읽어봐\". Handles content fetching, summarization, file placement, and README index updates."
argument-hint: "[url-or-path]"
---

# Archive

Read external content (URLs, PDFs, documents) and archive it as a well-structured markdown file in the appropriate location within this repository.

The point of this repo is that a future reader finds the archived summary instead of chasing a link that may be dead or paywalled. That is why the two things that can't slip are **fidelity** (the summary stands in for the original) and **discoverability** (an unindexed file is effectively lost).

## Critical

- Each source document gets its own separate md file — never merge multiple sources into one
- File names are kebab-case: `descriptive-topic-name.md`
- Always update the category `README.md` index after creating the file
- Write in English. One exception: a Korean-language source may stay in Korean when the original phrasing is the substance (see `guides/code-reading-era.md`); its README description is then Korean too

## Steps

### 1. Check for Duplicates

Before fetching, check whether this source is already archived:

- **URLs**: `Grep` for the URL (or its domain + distinctive path segment) across `.md` files in the archive directories only — `guides/`, `rules/`, `mcp/`, `showcase/`, `skills/`, `tools/`, `plugins/`, `cheatsheets/`, `role/`. Exclude workspace, `.claude/`, and test directories.
- **PDFs and local files**: there's no URL to match on, so grep the document title and skim the target category's `README.md` instead.

If a match is found, tell the user and ask whether to update the existing file or skip.

### 2. Fetch and Read Content

- **URLs**: use `WebFetch`
- **GitHub repos**: fetch the raw README at `<repo-url>/raw/refs/heads/main/README.md`. If that 404s, the default branch is probably `master` — retry with it before giving up. Also check the repo page itself for the description and topics.
- **PDFs**: use `Read` with the file path. For 10+ pages, read in chunks with the `pages` parameter (e.g., `pages: "1-20"`). Max 20 pages per request.
- **Local files**: use `Read` directly

Extract all key points, structure, examples, and recommendations.

**If fetching genuinely fails** (403, paywall, timeout, or 404 on every branch you tried): stop and tell the user, including the HTTP status, and ask them to paste the content or supply an alternative URL. Reaching the *same* document by another path — a `master` branch, a raw URL, a print view — is fine. Substituting a *different* document, or reconstructing content from memory, is not: the archive would then hold something the user never asked to preserve, and nothing downstream would reveal it.

### 3. Determine Target Location

Read `references/repo-structure.md` for the layout.

**Category** — what kind of thing is it? This map describes what the repo holds today, not a fixed schema:

| Content | Category |
| --- | --- |
| Technical how-to, best practices, research, essays, interviews | `guides/` |
| Coding conventions, rules, standards | `rules/` |
| MCP server documentation | `mcp/` |
| Configuration examples, reference implementations, workflow systems | `showcase/` |
| Agent skill definitions and skill collections | `skills/` |
| CLI tools and utilities for AI agents | `tools/` |
| Third-party Claude Code plugin archives | `plugins/` |
| Quick references, cheat sheets | `cheatsheets/` |

`role/` holds hand-written role prompts, not archived sources — never place fetched content there.

**Subdirectory** — three shapes; pick the one the category already uses:

1. **Cross-domain** content in a domain-organized category → category root: `guides/my-guide.md`
2. **Domain-specific** content (`frontend`, `app`, `vscode-extension`, `devops`) → `<category>/<domain>/`: `guides/frontend/my-guide.md`
3. **A named project** archived under `skills/` or `plugins/` → `<category>/<project-name>/`: `plugins/claude-ads/claude-ads-paid-advertising-audit.md`, `skills/gstack/gstack-claude-code-workflow.md`. Use this when the source is one identifiable project rather than a general topic; a skill doc that's really about a domain still uses shape 2 (`skills/frontend/...`).

`tools/` and `cheatsheets/` are flat — no subdirectories.

**The existing categories aren't binding.** They accumulated as content arrived rather than from a plan, so treat them as the current state of the archive, not as a set of bins everything must fit into. If a document has no natural home, make one — a folder stuffed with things that only half-belong hides them more effectively than an extra folder ever would.

A new category earns its place when you can name what it holds more precisely than any existing folder does, and you'd expect it to hold more than this one document. `benchmarks/` for tool comparisons, `papers/` for research, `talks/` for conference sessions all qualify. `misc/`, or a folder named after this single file, doesn't — that's a file with extra steps.

Creating a category can also mean **moving files that are already filed elsewhere**. That's normal, not a reason to avoid the new folder: the cost to avoid is the same kind of content sitting in two places, since then neither folder answers the question. `guides/` currently holds essays and interviews, so an `essays/` folder means relocating those and fixing their README rows too. When a move touches more than a file or two, tell the user what you're relocating rather than reshuffling the archive silently.

When creating a category: make the directory, add a `README.md` with a flat table (add domain sections later if it grows subdirectories), and add a row to the root `README.md` Categories table.

### 4. Create the Archive File

Follow the format in `references/archive-format.md`.

**Date fields**: include both `Fetched` (when the content was retrieved from the source) and `Archived` (when it was committed here). For same-session archiving, both are today's date.

**File naming**: derive from the content title or topic, kebab-case, descriptive.
- Good: `agents-md-stop-using-init.md`, `react-server-components-guide.md`
- Bad: `blog-post.md`, `article1.md`, `temp.md`

### 5. Update the Category README Index

Every **category** has a `README.md`. Domain and project subdirectories do **not** — so the entry always goes in the category README (`guides/README.md`, `plugins/README.md`, …), and you never create a README inside a subdirectory.

Two table shapes:

- **Categories with subdirectories** (`guides/`, `rules/`, `skills/`, `mcp/`, `showcase/`) have one table per domain section — `## General`, `## Frontend`, `## App`, `## DevOps`, `## VS Code Extension`. Add the row under the section matching where the file went; files at the category root go under `## General`. Create the section if it doesn't exist yet. Project-subdirectory files go under `## General` unless they're clearly domain-specific, and the link path includes the folder: `[gstack](./gstack/gstack-claude-code-workflow.md)`.
- **Flat categories** (`tools/`, `plugins/`, `cheatsheets/`) have a single table — append there.

Match the **column layout** of the table you're editing: some are `File | Description | Source`, others only `File | Description`. Don't add a Source column where the table has none.

```markdown
| [filename.md](./filename.md) | Description | [Source Name](url) |
```

**Source link text**: the author or organization, not the domain.
- Good: `[Simon Willison](url)`, `[Anthropic](url)`, `[Vercel Labs](url)`, `[HKUDS](url)`
- Bad: `[simonwillison.net](url)`, `[www.anthropic.com](url)`, a bare `[GitHub](url)`

Some older rows still use domains and `[GitHub]`. Those predate this rule — don't copy them.

### 6. Verify — before committing

- The file is at the correct path with a kebab-case name
- The README row is in the right section and matches that table's column layout
- The content follows the archive format template

Fix anything that's off in the working tree and re-check, so the commit stays one clean addition instead of an addition plus a fixup. Path, index, and format problems are all fixable from what's already on disk — only re-fetch the source if the *content* looks wrong (truncated fetch, mismatched title, missing sections), since that's the only failure the original page can settle.

### 7. Commit

Stage the new file and the updated README, then commit:

```
docs: Archive <short title> (<author or source>)
```

Examples from this repo:
- `docs: Archive Agentic Engineering Patterns guide (Simon Willison)`
- `docs: Archive ClawTeam agent swarm intelligence framework (HKUDS)`

### 8. Push

Push to the remote immediately. The usual caution about pushing without being asked exists because a push can disrupt others' work or ship something unreviewed — neither applies here. This is a personal archive where every commit is a standalone additive document with no build, no tests, and no reviewer, so an unpushed commit is just an archive that isn't backed up yet.
