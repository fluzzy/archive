# Harness: Agent Team & Skill Architect

> **Source**: [revfactory/harness](https://github.com/revfactory/harness)
> **Author**: revfactory
> **Fetched**: 2026-03-30
> **Archived**: 2026-03-30

Harness is a Claude Code Plugin (v1.0.1, Apache 2.0) that functions as a "meta-skill" for designing domain-specific agent teams. It decomposes complex tasks by generating specialized agents and their corresponding skills, outputting configurations into `.claude/agents/` and `.claude/skills/` directories.

---

## Core Concept

Harness automates the design of multi-agent architectures. Given a domain or task description, it analyzes the problem, selects an appropriate team pattern, defines individual agents, generates skill files for each agent, and wires them together with orchestration logic.

---

## Architectural Patterns

Six collaboration patterns are supported:

| Pattern | Description |
|---|---|
| **Pipeline** | Sequential dependent tasks — each agent's output feeds the next |
| **Fan-out/Fan-in** | Parallel independent tasks collected into a single result |
| **Expert Pool** | Context-dependent selective invocation of specialists |
| **Producer-Reviewer** | Generation followed by quality review |
| **Supervisor** | Central agent with dynamic task distribution |
| **Hierarchical Delegation** | Top-down recursive delegation |

---

## Workflow Phases

1. **Domain Analysis** — understand the problem space
2. **Team Architecture Design** — select pattern, define agent roles
3. **Agent Definition Generation** — create agent configurations
4. **Skill Generation** — produce skill files with progressive disclosure for context management
5. **Integration & Orchestration** — wire inter-agent data passing and error handling
6. **Validation & Testing** — dry-run testing and comparative analysis

---

## Installation

**Via Marketplace:**

```
/plugin marketplace add revfactory/harness
/plugin install harness@harness
```

**Direct Installation:**

```
cp -r skills/harness ~/.claude/skills/harness
```

---

## Execution Modes

| Mode | Use Case |
|---|---|
| Agent Teams (default) | 2+ agents requiring collaboration |
| Subagents | One-off tasks without inter-agent communication |

**Requirement**: Agent Teams must be enabled via `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`.

---

## Example Use Cases

- **Deep research** — multi-angle investigation with parallel experts
- **Full-stack website development** — design through deployment pipeline
- **Webtoon/comic production** — style consistency reviews across panels
- **YouTube content planning** — SEO optimization with producer-reviewer pattern
- **Code review** — parallel architecture and security checks (fan-out/fan-in)
- **API documentation generation** — structured pipeline from source to docs
- **Data pipeline design** — hierarchical delegation for complex ETL
- **Marketing campaigns** — A/B testing with supervisor coordination

---

## Research Validation

A controlled study across 15 software engineering tasks showed:

- Quality scores improved from **49.5 to 79.3** (60% improvement)
- **100% win rate** across all tasks
- **32% reduced output variance** (more consistent results)

---

## Related Projects

- **Harness 100** — 200 production-ready agent packages across 10 domains
- **claude-code-harness** — A/B testing research repository
