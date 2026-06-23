# JSON-LD Explained for Personal Websites

> **Source**: [Ethan Hawksley](https://hawksley.dev/blog/json-ld-explained-for-personal-websites/)
> **Author**: Ethan Hawksley
> **Fetched**: 2026-06-23
> **Archived**: 2026-06-23

A practical walkthrough of JSON-LD (JSON Linked Data) structured data for personal websites — what each schema.org node type does, which ones a personal site actually needs, and copy-pasteable examples.

---

## What JSON-LD Is

JSON-LD is a format for adding structured data to webpages so search engines can understand the semantics of a site, enabling richer previews and potentially better rankings.

- Lives in a `<script type="application/ld+json">` tag, typically in the document `<head>`.
- The browser's JavaScript engine ignores it; specialized crawlers (e.g. Googlebot) parse it.
- Web crawlers can **merge the properties of a node across multiple pages** when `@id` values match — though single-page crawlers won't perform this merge, so include enough on each page.

### Basic Structure

- **`@context`**: set to `https://schema.org` to establish the schema standard.
- **`@graph`**: an array of nodes, each representing a distinct entity.
- **Node properties**:
  - `@type` — what the node represents (e.g. `WebSite`, `Person`).
  - `@id` — a unique identifier, usually a URL with a hash fragment (e.g. `https://hawksley.dev/#person`).
  - Custom key-value properties describing the entity.

---

## Recommended Node Types for Personal Sites

### WebSite

Describes overall site metadata. Use full details on root pages; abbreviated versions suffice elsewhere.

```json
{
  "@type": "WebSite",
  "@id": "https://hawksley.dev/#website",
  "url": "https://hawksley.dev/",
  "name": "Ethan Hawksley",
  "alternateName": ["hawksley.dev", "Hawksley"],
  "description": "...",
  "inLanguage": "en-GB",
  "publisher": { "@id": "https://hawksley.dev/#person" },
  "image": { "...": "..." }
}
```

### WebPage

Represents the physical HTML page itself. Use on generic pages; more specific subtypes exist for other contexts.

### Person

Critical for personal sites — include it on **every** page. Key properties:

- `url`, `name`, `givenName`, `familyName`
- `image` — a photo or associated logo
- `sameAs` — links to other profiles; **immensely useful for disambiguation**, especially with a common name, and for knowledge graphs
- `jobTitle`, `knowsLanguage`, `affiliation`, `alumniOf`

### ProfilePage

For pages discussing a person (e.g. an about page). Link to both the `WebSite` and `Person` nodes via `isPartOf` and `mainEntity`.

### SoftwareApplication

Describes software projects:

- `url` — link to the deployment (e.g. crates.io)
- `applicationCategory` — type of software
- `creator` — reference to the `Person` node
- `offers` — include even for free/open-source projects, setting `price` to `0`

### BreadcrumbList

Structures navigation hierarchy for pages beyond the root. Each item has a position, name, and URL. Influences how search results display page paths.

### CollectionPage

For pages that are primarily lists (blog archives, profile directories). Uses `about` to reference the `Person` node.

### Blog

Acts as an intermediary between `WebSite` and individual `BlogPosting` nodes. Include `dateModified` for freshness signals and `license` to clarify reuse permissions.

### BlogPosting

Detailed markup for blog articles:

- `headline`, `description`, `articleSection`
- `keywords` — comma-separated relevant terms
- `datePublished`, `dateModified` — ISO 8601 timestamps
- `author`, `publisher` — both can reference the same `Person` on a personal site
- `image` — should match the Open Graph image (1200×630 px recommended)
- `license` — terms under which the content may be used

---

## Key Recommendations

1. **Be more descriptive, not less** — it helps to fill out properties thoroughly rather than sparsely.
2. **Stay consistent across pages** — use consistent `@id` values so crawlers can merge data sitewide.
3. **Minimum implementation** — static sites without a build process should at least include `WebSite`, `ProfilePage`, and `Person` on the root page.
4. **Publisher flexibility** — for personal sites, a `Blog`'s `publisher` can reference a `Person` instead of requiring an `Organization`.
5. **Optional enhancements** — `dateCreated`, `dateModified`, and `BreadcrumbList` improve results but aren't strictly essential.

> "That's all the JSON-LD a personal site needs!" — the examples are structured for easy copy-paste adoption regardless of technical setup.

---

## References

- [JSON-LD Explained for Personal Websites — Ethan Hawksley](https://hawksley.dev/blog/json-ld-explained-for-personal-websites/) (June 21, 2026)
- Korean aggregator discussion: [GeekNews / news.hada.io](https://news.hada.io/topic?id=30729)
