# Obsidian Vault Organization

> **Source**: [Steph Ango](https://stephango.com/vault)
> **Author**: Steph Ango (CEO, Obsidian)
> **Fetched**: 2026-05-04
> **Archived**: 2026-05-04

Steph Ango's personal system for organizing an Obsidian vault. A bottom-up, "file over app" approach that favors emergent links over rigid folders, with concrete rules for naming, properties, and review cadence.

---

## Core Philosophy

A bottom-up approach that embraces controlled chaos. The system prioritizes the **"file over app"** principle — notes stay as portable, standard files rather than locked into proprietary formats. Structure emerges from links and properties, not folder hierarchies.

## Personal Rules

- Avoid splitting content into multiple vaults
- Avoid folders for organization
- Avoid non-standard Markdown
- Always pluralize categories and tags
- Use internal links profusely
- Use `YYYY-MM-DD` dates everywhere
- Use the 7-point scale for ratings
- Keep a single to-do list per week

## Folder Structure

The vault is mostly flat. Personal writing lives in the root; only a handful of reference folders exist, and nested subfolders are avoided.

| Folder | Purpose |
| --- | --- |
| *(root)* | Personal writing — journal entries, essays, evergreen notes |
| `References/` | External subjects (books, movies, people, places) |
| `Clippings/` | Content written by others |
| `Attachments/` | Media files |
| `Daily/` | Daily notes named `YYYY-MM-DD.md`, used for linking only |
| `Templates/` | Note templates |

Navigation happens through quick switcher, backlinks, and internal links — not the file explorer.

## Organization via Categories

Notes are grouped through a `categories` property rather than folders. Obsidian's **bases** feature renders category-based overviews, letting related notes cluster across the vault without folder constraints.

## Linking Strategy

Internal links are used aggressively, including **unresolved links** (to notes that don't yet exist). These act as breadcrumbs for future writing and create a traceable web showing how ideas develop over time.

## Fractal Journaling

Atomic thought notes are created throughout the day via the unique-note hotkey, auto-prefixed with `YYYY-MM-DD HHmm`. Review happens at multiple zoom levels:

- **Every few days** — review fragments, compile salient thoughts
- **Monthly** — review monthly summaries
- **Yearly** — review yearly summaries
- **Every few months** — "random revisit" sessions using the random-note feature to rediscover old ideas

This creates a fractal web that can be zoomed at varying levels.

## Templates and Properties

Nearly every note starts from a template with reusable properties:

| Group | Examples |
| --- | --- |
| Dates | created, start, end, published |
| People | author, director, artist, cast, host, guests |
| Themes | genre, type, topic, related |
| Locations | neighborhood, city, coordinates |
| Ratings | 7-point scale |

**Property design principles:**
- Reusability across categories
- Composability of templates
- Short naming conventions
- Default to **list-type** properties for any field that could be multi-value

## Rating System (1–7)

| Score | Meaning |
| --- | --- |
| 7 | Perfect, life-changing |
| 6 | Excellent, worth repeating |
| 5 | Good, enjoyable |
| 4 | Passable, adequate |
| 3 | Bad, avoid if possible |
| 2 | Atrocious, repulsive |
| 1 | Evil, negatively life-changing |

Seven points provide more granularity at the top — for the good experiences — than a 5- or 10-point scale.

## Tools and Plugins

| Tool | Purpose |
| --- | --- |
| Minimal theme + Flexoki palette | Visual style |
| Web Clipper | Save articles with category-specific templates |
| Obsidian Sync | Multi-device sync |
| Obsidian Bases | Category-based note views |
| Obsidian Maps | Location visualization |
| Obsidian Git | Publishing workflow integration |

## Publishing

The personal site is generated via Jekyll, with notes pushed through GitHub using the Obsidian Git plugin. The site is a separate vault from the personal one and requires technical setup, but provides full layout control. The Flexoki color palette was created specifically for the site.

---

## References

- [Steph Ango — Vault](https://stephango.com/vault)
- [File over app](https://stephango.com/file-over-app)
- [Flexoki color palette](https://stephango.com/flexoki)
