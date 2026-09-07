# Archive Format Template

Every archived document follows this shape:

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
- Put source attribution in a blockquote immediately after the title
- Include `Fetched` (when content was retrieved) and `Archived` (when committed here) — for same-session archiving, both are today's date
- Add `Author` if identifiable. For organization or team projects (e.g., GitHub repos by an org), you may omit it since `Source` already credits the org. For blog posts and articles, always include it
- Extra blockquote fields are welcome when they carry real information — `> **Homepage**:` for a project with its own site, a second `Source` link when the piece was found via an aggregator. The four core fields are the floor, not a cap
- Use `---` horizontal rules to separate major sections
- End with a References section linking back to all source material
- Keep content structured: prefer headers, bullets, and tables over long paragraphs
- Summarize clearly but preserve key details, data points, and actionable advice
- If the source has code examples, preserve them
- Write in English, unless the source is Korean and its original phrasing is the substance (see `guides/code-reading-era.md`)

## Length

Most archives land in **50-200 lines**. That range describes typical blog posts and repo READMEs — it is not a ceiling to cut structure for. Reference documents and rule sets that must keep their tables, code, and configuration intact run longer; `rules/frontend/playwright-e2e.md` is 680 lines and correctly so.

The thing worth trimming is restatement — intros that repeat the title, marketing copy, and three examples where one carries the point. The thing worth keeping is anything a reader would otherwise have to reopen the original to find.

## Source Name Formatting

The Source field links to the original URL. Use the **author or organization name** as the link text, not the raw domain — a reader scanning a README index recognizes a person or a company, while a hostname tells them almost nothing.

Good examples:
- `[Simon Willison](https://simonwillison.net/...)`
- `[Anthropic](https://www.anthropic.com/...)`
- `[Toss Tech](https://toss.tech/...)`
- `[HKUDS](https://github.com/HKUDS/ClawTeam)`

Bad examples:
- `[simonwillison.net](...)` — use the author name
- `[GitHub](https://github.com/...)` — use the repo owner or org name
- `[www.anthropic.com](...)` — use the company name

For GitHub repos, take the organization or author from the repo path (`anthropics/claude-code-action` → `[Anthropic](url)`).

Some older entries in the archive still use domains and a bare `[GitHub]`. They predate this rule — match the surrounding table's *columns*, not its legacy link text.

## Content Guidance by Source Type

**Blog posts / Articles**: Capture the core argument, key examples, and actionable advice. Preserve the author's structure when it's clear.

**GitHub repos**: Focus on what the tool does, key features, architecture decisions, and usage patterns. Don't reproduce the entire README — distill the important parts.

**Documentation / Guides**: Preserve the reference structure. Keep code examples, tables, and configuration details intact.

**Research / Papers**: Lead with findings and practical implications. Summarize methodology briefly.
