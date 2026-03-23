# Impeccable: AI Design Skills Package

> **Source**: [GitHub](https://github.com/pbakaus/impeccable) | [Hacker News KR](https://news.hada.io/topic?id=27750)
> **Author**: Paul Bakaus
> **Fetched**: 2026-03-23
> **Archived**: 2026-03-23

An open-source skill package that enhances frontend design quality in AI code assistants. Built on Anthropic's `frontend-design` skill, expanded with curated patterns, anti-patterns, and 20 specialized design commands.

---

## Problem

AI models tend to generate predictable design mistakes: overused fonts (Inter, system-ui), poor color contrast, excessive card nesting, dated animation patterns (bounce easing), and generic layouts. Impeccable provides structured guidance to steer AI toward professional design decisions.

## Key Components

### The Skill

A comprehensive framework with 7 reference documents covering:

1. **Typography** - Font selection, hierarchy, sizing
2. **Color & Contrast** - Accessible palettes, avoiding low-contrast text
3. **Spatial Design** - Spacing, layout, visual rhythm
4. **Motion** - Animation principles, timing functions
5. **Interaction Design** - Hover states, transitions, feedback
6. **Responsive Patterns** - Breakpoints, fluid layouts
7. **UX Writing** - Microcopy, labels, error messages

### 20 Steering Commands

| Command | Purpose |
| --- | --- |
| `/audit` | Diagnostic report without modifications |
| `/critique` | UX feedback distinct from technical fixes |
| `/normalize` | Design system alignment |
| `/polish` | Final pre-deployment refinement |
| `/distill` | Simplify and reduce visual noise |
| `/clarify` | Improve readability and clarity |
| `/optimize` | Performance-focused refinements |
| `/harden` | Accessibility and edge-case hardening |
| `/animate` | Add or improve motion design |
| `/colorize` | Color palette improvements |
| `/bolder` | Increase visual weight and impact |
| `/quieter` | Reduce visual intensity |
| `/delight` | Add subtle interaction delights |
| `/extract` | Extract reusable design tokens |
| `/adapt` | Responsive design adjustments |
| `/onboard` | Onboarding flow improvements |
| `/typeset` | Typography refinement |
| `/arrange` | Layout and composition fixes |
| `/overdrive` | Maximum design enhancement mode |

Commands accept optional arguments to focus on specific areas (e.g., `/audit checkout`).

### Anti-Pattern Protection

Blocks common LLM design mistakes including:

- Default/overused fonts (Inter, system-ui everywhere)
- Gray text on colored backgrounds
- Pure black/gray tints without warmth
- Nested card layouts (cards within cards)
- Low-contrast text
- Bounce easing on all animations
- Generic hero sections

## Command Chaining

Commands can be chained together for workflows:

```
/audit /normalize /polish [component]
```

This runs quality checks, applies design system standards, then performs final refinement sequentially.

## Installation

```bash
npx skills add pbakaus/impeccable
```

### Supported Platforms

- Claude Code
- Cursor
- Codex CLI
- OpenCode
- Pi
- Gemini CLI
- VS Code
- Kiro

Manual setup is also available by copying folders from the repository distribution directory.

## Project Stats

- 12.5k GitHub stars, 494 forks
- Apache 2.0 license
- Built with JavaScript (53.2%), CSS (27.9%), HTML (18.9%)
- Demo: https://impeccable.style/
