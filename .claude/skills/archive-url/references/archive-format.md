# Archive Format Template

Every archived document MUST follow this format:

```markdown
# Title

> **Source**: [Author or Organization Name](url)
> **Author**: Author Name (if known)
> **Fetched**: YYYY-MM-DD
> **Archived**: YYYY-MM-DD

Brief 1-2 sentence introduction explaining what this document covers.

---

## Section 1

Content organized by topic with clear headers.

## Section 2

Use tables, code blocks, and lists for clarity.

---

## References

- [Full citation with link](url)
```

## Rules

- Start with a markdown heading (`# Title`)
- Include source attribution in blockquote format immediately after the title
- Include `Fetched` date (when content was retrieved) and `Archived` date (when committed to repo) — for same-session archiving, both are today's date
- Add author if identifiable. For organization/team projects (e.g., GitHub repos by an org), you may omit the Author line — the Source field already credits the org. For blog posts or articles, always include the Author.
- Use `---` horizontal rules to separate major sections
- End with a References section linking back to all source material
- Keep content structured: prefer headers, bullets, and tables over long paragraphs
- Summarize clearly but preserve key details, data points, and actionable advice
- If the source has code examples, preserve them
- Target length: 50-200 lines (adjust based on source complexity)

## Source Name Formatting

The Source field links to the original URL. Use the **author or organization name** as the link text, not the raw domain.

Good examples:
- `[Simon Willison](https://simonwillison.net/...)`
- `[Anthropic](https://www.anthropic.com/...)`
- `[Toss Tech](https://toss.tech/...)`
- `[HKUDS](https://github.com/HKUDS/ClawTeam)`

Bad examples:
- `[simonwillison.net](...)` — use author name
- `[GitHub](https://github.com/...)` — use the repo owner/org name
- `[www.anthropic.com](...)` — use company name

For GitHub repos, use the organization or author name from the repo path (e.g., `anthropics/claude-code-action` → `[Anthropic](url)`).

## Content Guidance by Source Type

**Blog posts / Articles**: Capture the core argument, key examples, and actionable advice. Preserve the author's structure when it's clear.

**GitHub repos**: Focus on what the tool does, key features, architecture decisions, and usage patterns. Don't reproduce the entire README — distill the important parts.

**Documentation / Guides**: Preserve the reference structure. Keep code examples, tables, and configuration details intact.

**Research / Papers**: Lead with findings and practical implications. Summarize methodology briefly.
