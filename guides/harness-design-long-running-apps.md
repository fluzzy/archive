# Harness Design for Long-Running Application Development

> **Source**: [Anthropic Engineering](https://www.anthropic.com/engineering/harness-design-long-running-apps), [GeekNews](https://news.hada.io/topic?id=27863)
> **Author**: Prithvi Rajasekaran (Anthropic Labs)
> **Fetched**: 2026-03-30
> **Archived**: 2026-03-30

Advanced techniques for improving Claude's performance on complex, multi-hour coding tasks and frontend design work using GAN-inspired multi-agent architecture. Separating generator and evaluator agents overcomes context degradation and self-evaluation bias.

---

## Core Problems

Two persistent issues hit performance ceilings despite successful prompt engineering:

1. **Context degradation**: Models lose coherence on lengthy tasks as context windows fill. Some exhibit "context anxiety" — prematurely wrapping up work when approaching perceived limits.

2. **Self-evaluation bias**: When agents grade their own work, they confidently praise mediocre outputs. Particularly problematic for subjective tasks like design.

**Solution**: Context resets with structured handoffs, and separation of generator and evaluator into distinct agents.

---

## GAN-Inspired Multi-Agent Architecture

Inspired by Generative Adversarial Networks, the system separates **generator** and **evaluator** agents. This proves more effective than making single agents self-critical.

---

## Frontend Design Application

### Four Grading Criteria

- **Design quality**: Coherent visual identity combining colors, typography, layout — "does it create a consistent mood and identity?"
- **Originality**: Custom decisions versus template defaults and AI patterns — "is it not just a template or AI-generated pattern?"
- **Craft**: Technical execution — typography hierarchy, spacing, color harmony
- **Functionality**: Usability and task completion — "can users understand the interface and find key actions?"

### Process

- Generator produces HTML/CSS/JS
- Evaluator uses Playwright MCP to navigate live pages and capture screenshots
- 5–15 iteration cycles per task, up to 4 hours
- Generator strategically refines or pivots directions based on evaluator feedback

---

## Full-Stack Coding Architecture

A three-agent system for application development:

### Planner Agent

Transforms simple 1–4 sentence prompts into comprehensive product specifications. Maintains high-level focus while integrating AI features.

### Generator Agent

Implements features iteratively using React, Vite, FastAPI, and SQLite/PostgreSQL stacks. Performs self-evaluation before QA handoff.

### Evaluator Agent

Uses Playwright MCP for end-to-end testing. Validates against sprint contracts negotiated before implementation began.

### Sprint Contracts

Before code is written, the generator and evaluator agree on a "definition of done." This prevents scope drift and ensures measurable outcomes.

---

## Concrete Results

### Retro Game Maker

| Approach | Time | Cost | Outcome |
| --- | --- | --- | --- |
| Solo agent | 20 min | $9 | Broken game mechanics despite polished interface |
| Full harness | 6 hours | $200 | Fully functional game editor with sprite animation, behavior templates, and AI-assisted features |

The evaluator kept implementation aligned with spec (Sprint 3 alone had 27 criteria).

### Digital Audio Workstation

- **Duration**: ~4 hours with simplified harness (removed sprint construct)
- **Cost**: $124.70
- **Outcome**: Functional browser-based DAW with working arrangement view, mixer, and agent-driven composition

---

## Evolution with Opus 4.6

As Claude Opus 4.6 improved, the author methodically removed load-bearing components:

- **Dropped sprint decomposition** — model capacity now supports 2+ hours of consistent continuous execution
- **Moved evaluator to single-pass at end** — reduced overhead for simpler tasks
- **Claude Agent SDK auto-compaction** handles context management automatically
- Evaluator remains valuable only when tasks exceed baseline model capability

---

## Key Lessons

1. **Assume nothing permanent**: Components encoding model limitations become obsolete as capabilities improve. Remove scaffolding that no longer bears performance load.

2. **Specialization matters**: Dedicated evaluators outperform self-critical generators. Each agent can be tuned independently.

3. **Criteria shape outputs**: Prompt language ("museum quality") directly influences generation character, independent of feedback loops.

4. **Harness complexity is task-dependent**: Evaluators add value beyond baseline capability; below that threshold, they represent pure overhead.

5. **Design space shifts, not shrinks**: Superior models enable new, more ambitious harness combinations rather than eliminating orchestration entirely. The art of AI engineering involves continuously re-examining assumptions and identifying novel architectural patterns.
