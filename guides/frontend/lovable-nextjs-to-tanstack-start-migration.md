# How Lovable Migrated lovable.dev from Next.js to TanStack Start

> **Source**: [Lovable](https://lovable.dev/blog/how-we-migrated-lovable-dev-away-from-nextjs)
> **Author**: Alexander Lebedev
> **Published**: 2026-08-18
> **Fetched**: 2026-09-07
> **Archived**: 2026-09-07

A six-month migration of an 850K+ line production app from Next.js on Vercel to TanStack Start on Cloudflare Workers — executed by one developer with AI agents, rolled out behind a per-route proxy, and nearly derailed by a 128MB isolate memory limit.

---

## Why Migrate

The stated driver was dogfooding: lovable.dev is now built on the same stack Lovable ships to users, so every hosting improvement benefits both. Supporting reasons:

- **Prove single-app scale.** Lovable hosts millions of small apps well; they wanted evidence for one app taking tens of millions of visits.
- **Unify the stack with the builder agent.** Internal knowledge about the stack transfers directly to the agent that writes user apps.

Concrete pain that made the move attractive:

| Metric | Next.js | After migration |
| --- | --- | --- |
| Local dev startup | 70s | 10s (Vite) |
| Local dev memory | 8GB (up to 20GB+ reported) | 1.5GB |
| CI/CD production build | 12+ min | 6–9 min |

The team also flagged an agent-specific cost: Next.js training data spans v12/v14/v16 with contradictory patterns (pages vs. app router), so agents made version-confused mistakes. Server components, server actions, and layout-vs-page separation were described as harder to reason about than TanStack's server functions, route loaders, and nested routes.

## Target Architecture

**TanStack Start**, chosen for isomorphic execution, deployment simplicity, and type safety. Vite for the dev server, TanStack Router for nested routes.

Hosting is Cloudflare Workers, where every Lovable app is a standalone worker bundle in its own V8 isolate:

```
Single app-loader worker (hostname resolution, access checks)
    ↓ dispatches to one of 60M+ app worker bundles
lovable.dev is one bundle among them
    ↓
Each app runs in an isolated V8 heap (cheaper than separate processes)
```

## Migration Strategy: Run Both Frameworks for Six Months

Rather than a cutover, a proxy worker routed each request to Next.js or TanStack Start per route and per user via feature flags.

```
[Proxy Worker]
    ├→ Next.js (Vercel)
    └→ TanStack Start bundle
       (per-route, per-user decision via feature flags)
```

Tests could pin a framework deterministically with an internal search parameter (`?__framework=tanstack`).

**Route grouping.** User journeys were mapped to find frequently-traversed route pairs, then split into five major groups migrated together. The reason is cost of crossing: navigation inside one framework is a ~1.5s soft navigation, crossing frameworks is a ~5s hard navigation. The View Transitions API softened what crossings remained:

```css
@view-transition { navigation: auto; }
```

## Keeping Code Framework-Agnostic

The target was 90–95% shared code. At Next.js removal, only **3%** was framework-specific.

**Shared directory via import aliases:**

```json
/* web/package.json (TanStack Start) */
"imports": { "#shared/*": "./shared/*" }

/* app/package.json (Next.js) */
"imports": { "#shared/*": "../web/shared/*" }
```

**Lint rules enforcing the boundary** — shared code may not import either framework, nor Node APIs:

```typescript
/* oxlint.config.ts */
{
  files: ["web/shared/**/*.{ts,tsx}"],
  rules: {
    "no-restricted-imports": ["error", {
      paths: [
        { name: "next", message: "Shared code cannot depend on Next.js." },
        { name: "@tanstack/react-start", message: "Shared code cannot depend on TanStack Start." },
        { name: "@tanstack/react-router", message: "Shared code cannot depend on TanStack Router." },
      ],
      patterns: [
        { group: ["next/*"], message: "Shared code cannot depend on Next.js." },
        { group: ["node:*"], message: "Shared code cannot use Node.js APIs. Must be runtime-agnostic." },
      ],
    }],
  },
}
```

**Platform adapters via TypeScript path mapping** — dependency injection at the type level, so shared code imports one name and each app resolves it differently:

```typescript
/* app/tsconfig.json */
"@platform/router": ["./lib/router/next-adapter.ts"]
/* web/tsconfig.json */
"@platform/router": ["./lib/router/tanstack-adapter.ts"]

/* next-adapter.ts */
export { usePathname } from "next/navigation";

/* tanstack-adapter.ts */
export function usePathname(): string {
  return useLocation({ select: (loc) => loc.pathname });
}

/* shared code — works in both */
import { usePathname } from "@platform/router";
```

**Framework APIs replaced before moving code**: `next/font` and `next/image` swapped for in-house equivalents, authentication brought in-house (10x+ fewer Firebase Auth calls), and a new i18n solution (~3x lower CPU at page load, plus typecheck errors on misspelled keys).

## Agent-Driven Execution

One developer plus AI agents, six months, against a moving target: the codebase grew from 375K to 850K+ lines during the migration (~60K lines of new work landed alongside).

Three techniques, in the order they evolved:

1. **Reusable migration skills.** Prompts extracted into components, so "Move component X to shared code" produced a production-ready PR.
2. **Delegate at a higher abstraction level.** Early work tracked low-level goals individually (21 React context providers as separate Linear tickets). Later work ran a loop: define a measurable metric ("how many agent tools still have Next.js UI?"), write a deterministic script to measure it, have an agent batch coherent PRs (max 12 PRs, 800–1600 lines each), have implementation agents execute, repeat until the metric hits zero.
3. **Portability checks on every PR.** An automated check reported whether new logic landed in shared code and educated authors when a non-portable pattern appeared.

```
Status: OK
Feature: Yes
Policy compliant: ✓
New feature logic correctly placed in web/shared/lib/favicon
Only app/ touch: two-line wiring in framework-bound instrumentation
```

## Rollout

Roughly two months, one route group externally at a time, with code migration still finishing during the first weeks:

```
Route group (×5, sequential):
├─ Internal testing (1 week)
├─ 1% external users (1–3 weeks)
├─ Gradual increase to 100%
└─ Next group queued (no rush)
```

Monitored throughout: TTFB, error rates, Web Vitals, memory per isolate, and synthetic vs. real-user metrics.

## The Out-of-Memory Incident

On a Friday afternoon at 20% dashboard rollout, error rates went 0.1% → 0.5% → 50% within minutes. Rollback took 11 minutes.

Cause: an unrelated feature had shipped a multi-megabyte static JSON at module level behind a flag. Against the Workers **128MB** isolate limit, isolates were being evicted after serving fewer than 10 requests (healthy baseline: 500–10,000), and every request on a dying isolate errored.

The retrospective named three mistakes: never measuring baseline memory, ignoring the early 0.1% error signal, and optimism ahead of data.

Memory fixes:

```typescript
// 1. Module-level JSON stays resident forever
import data from './huge.json';            // before
import dataRaw from './huge.json?raw';     // after — import as string
const data = JSON.parse(dataRaw);          // parse per request
// 2x–12x reduction depending on route

// 2. Drop unused fields from list APIs (template descriptions) — a few MB

// 3. Stub client-only code out of the server bundle
// TypeScript compiler, Prettier, syntax highlighter → empty stubs
// ~9MB bundle → ~18MB worker memory saved
// (bundle bytes roughly double in V8 due to two-byte character format)
```

## Results

- **Median TTFB: −49%**
- **p90 TTFB**: initially +200% (worse), brought to −16% after optimization
- Client-side metrics at parity
- 910K+ lines across 400+ routes, 97% framework-agnostic, 150+ agent tools

## Caveats

TanStack Start is not free of configuration cost at this size — lovable.dev's build needs **17 custom build plugins** for code splitting, multiple dev environments, and the asset pipeline, where Next.js optimizes out of the box. The article predates Next.js v16.3's claimed 90% memory reduction. And Lovable is not declaring Next.js obsolete: the post closes by saying not to be surprised if Lovable lets you import Next.js apps before long.

---

## References

- [How we migrated lovable.dev away from Next.js and turned it into another Lovable app](https://lovable.dev/blog/how-we-migrated-lovable-dev-away-from-nextjs) — Alexander Lebedev, Lovable, 2026-08-18
