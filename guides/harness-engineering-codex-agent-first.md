# Harness Engineering: Leveraging Codex in an Agent-First World

> **Source**: [OpenAI](https://openai.com/index/harness-engineering/)
> **Author**: Ryan Lopopolo
> **Fetched**: 2026-03-25
> **Archived**: 2026-03-25

A case study from OpenAI on building and shipping an internal product with zero manually-written code over five months, using Codex agents exclusively. Documents the engineering practices, architecture decisions, and lessons learned when human engineers shift from writing code to designing environments for agents.

---

## The Experiment

- Built a product from an empty git repo (first commit: late August 2025) with **0 lines of manually-written code**
- ~1 million lines of code across app logic, infra, tooling, docs, and internal utilities
- ~1,500 PRs merged by a team of 3 engineers (later 7), averaging **3.5 PRs per engineer per day**
- Estimated **1/10th the time** compared to manual coding
- Product has hundreds of internal users, including daily power users

Core philosophy: **Humans steer. Agents execute.**

---

## Redefining the Engineer's Role

Engineers no longer write code. Their primary job is **enabling agents to do useful work**:

- Break larger goals into smaller building blocks (design, code, review, test)
- When something fails, ask: "what capability is missing, and how do we make it legible and enforceable for the agent?"
- Interact with the system almost entirely through prompts
- Agent-to-agent review handles most PR review — humans aren't required to review

The fix is never "try harder." It's always about improving the environment.

---

## Increasing Application Legibility

The bottleneck shifted from code throughput to **human QA capacity**. Solution: make the app itself legible to agents.

**UI validation**: App bootable per git worktree, Chrome DevTools Protocol wired into agent runtime. Codex can take DOM snapshots, screenshots, navigate UI, reproduce bugs, and validate fixes directly.

**Observability**: Local ephemeral observability stack per worktree (Victoria Logs/Metrics/Traces). Agents query logs with LogQL, metrics with PromQL. Enables prompts like "ensure service startup completes in under 800ms."

Single Codex runs regularly work on tasks for **6+ hours** (often while humans sleep).

---

## Repository Knowledge as System of Record

### Why "One Big AGENTS.md" Failed

1. **Context is scarce** — giant instruction files crowd out the actual task and code
2. **Too much guidance becomes non-guidance** — when everything is "important," nothing is
3. **It rots instantly** — monolithic manuals become graveyards of stale rules
4. **Hard to verify** — no mechanical checks for coverage, freshness, or drift

### The Solution: AGENTS.md as Table of Contents

A short AGENTS.md (~100 lines) serves as a **map with pointers** to deeper sources of truth:

```
AGENTS.md
ARCHITECTURE.md
docs/
├── design-docs/       # Catalogued, indexed, with verification status
├── exec-plans/        # Active plans, completed plans, tech debt tracker
├── generated/         # Auto-generated docs (e.g., db-schema.md)
├── product-specs/     # Product specifications
├── references/        # External reference docs (*-llms.txt)
├── DESIGN.md
├── FRONTEND.md
├── PLANS.md
├── PRODUCT_SENSE.md
├── QUALITY_SCORE.md
├── RELIABILITY.md
└── SECURITY.md
```

**Progressive disclosure**: agents start with a small, stable entry point and are taught where to look next.

Enforced mechanically: linters + CI validate the knowledge base is up to date. A recurring "doc-gardening" agent scans for stale docs and opens fix-up PRs.

---

## Agent Legibility as the Goal

> From the agent's point of view, anything it can't access in-context while running effectively doesn't exist.

- Knowledge in Google Docs, Slack threads, or people's heads is invisible to agents
- Push all context into the repo as versioned artifacts (code, markdown, schemas, executable plans)
- Favor "boring" technologies — composable, stable APIs well-represented in training data
- Sometimes cheaper to reimplement functionality than work around opaque upstream libraries

---

## Enforcing Architecture and Taste

**Rigid layered architecture** per business domain:

```
Types → Config → Repo → Service → Runtime → UI
```

Cross-cutting concerns (auth, connectors, telemetry, feature flags) enter through a single explicit interface: **Providers**. Everything else is disallowed and enforced mechanically.

Key practices:
- Custom linters with error messages that **inject remediation instructions** into agent context
- Enforce invariants, not implementations (e.g., require boundary parsing, don't prescribe the library)
- Structural tests validate dependency directions
- "Taste invariants": structured logging, naming conventions, file size limits

> In a human-first workflow, these rules might feel pedantic. With agents, they become multipliers.

---

## Entropy and Garbage Collection

Agents replicate existing patterns — even suboptimal ones. Team used to spend every Friday (20% of the week) cleaning up "AI slop." Didn't scale.

**Solution: "Golden Principles" + automated cleanup**

- Opinionated, mechanical rules encoded directly in the repo
- Recurring background Codex tasks scan for deviations, update quality grades, open targeted refactoring PRs
- Most reviewable in under a minute, automerged
- Functions like **garbage collection** — pay down technical debt continuously in small increments

---

## Increasing Levels of Autonomy

From a single prompt, Codex can now end-to-end:

1. Validate codebase state
2. Reproduce a reported bug
3. Record a video demonstrating the failure
4. Implement a fix
5. Validate the fix by driving the application
6. Record a video demonstrating the resolution
7. Open a PR
8. Respond to agent and human feedback
9. Detect and remediate build failures
10. Escalate to a human only when judgment is required
11. Merge the change

---

## Key Takeaways

| Principle | Practice |
| --- | --- |
| Map, not manual | Short AGENTS.md pointing to structured docs |
| Make the app legible | Chrome DevTools + observability exposed to agents |
| Enforce invariants, not implementations | Custom linters with remediation messages |
| Repo is the system of record | All context versioned in-repo, not in Slack/Docs |
| Continuous garbage collection | Automated cleanup agents on regular cadence |
| Boring technology wins | Stable, composable, well-trained-on dependencies |
| Throughput changes merge philosophy | Minimal blocking gates, corrections are cheap |

---

## References

- [Harness engineering: leveraging Codex in an agent-first world](https://openai.com/index/harness-engineering/) — OpenAI Engineering Blog, February 11, 2026
