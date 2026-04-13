# Archive

A curated collection of rules, guides, skills, and resources for coding agents.

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

## Categories

| Directory | Description |
| --- | --- |
| [guides/](./guides/) | How-to guides and best practices |
| [rules/](./rules/) | Coding rules and conventions |
| [skills/](./skills/) | Agent skill definitions and archives |
| [mcp/](./mcp/) | MCP server documentation |
| [tools/](./tools/) | CLI tools and utilities for AI agents |
| [plugins/](./plugins/) | Third-party Claude Code plugin archives |
| [showcase/](./showcase/) | Configuration examples |
| [cheatsheets/](./cheatsheets/) | Quick references and cheat sheets |

## Roles

Standalone Claude role prompts for specialized personas. See [role/](./role/) for all available roles.

| Role | Tech Stack |
| --- | --- |
| [CTO](./role/cto.md) | General |
| [Debugger](./role/debugger.md) | DevTools |
| [Refactoring Expert](./role/refactoring-expert.md) | General |
| [React Performance Engineer](./role/react-performance-engineer.md) | React, Next.js |
| [Frontend Code Reviewer](./role/frontend-code-reviewer.md) | Frontend |
| [Frontend UX Engineer](./role/frontend-ux-engineer.md) | WCAG, ARIA |
| [Three.js Creative Developer](./role/threejs-creative-developer.md) | Three.js, WebGL, GSAP |
| [RESTful API Designer](./role/restful-api-designer.md) | REST, OpenAPI, Zod |
| [Jest Test Engineer](./role/jest-test-engineer.md) | Jest, Vitest, Playwright |
| [Semantic SEO Expert](./role/semantic-seo-expert.md) | HTML, Schema.org, JSON-LD |
| [Senior Product Planner](./role/senior-product-planner.md) | General |

## Contributing

- Add cross-domain resources directly under the category directory (e.g., `guides/my-guide.md`)
- Add domain-specific resources under `<category>/<domain>/` (e.g., `guides/frontend/my-guide.md`)
- Add new roles using [role/_template.md](./role/_template.md)
- Update the relevant README when adding new files
