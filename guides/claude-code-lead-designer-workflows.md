# How Claude Code's Lead Designer Builds with AI

> **Source**: [Dive Club](https://www.youtube.com/watch?v=hKeDfupbA4U)
> **Author**: Meaghan Choi (Lead Designer, Claude Code @ Anthropic)
> **Fetched**: 2026-06-17
> **Archived**: 2026-06-17

A 12-minute practical demo of the Claude Code workflows that have become popular internally at Anthropic, presented live by Claude Code's lead designer. The demo adds an autocomplete feature to the open-source Excalidraw repo while making the larger point that AI should be wired into the *entire* product pipeline — ideation through deployment — not just the coding step.

> Transcript-based summary of the talk "How Claude Code's lead designer builds with AI" (Dive Club, 2026-06-04, 12:18). Also summarized in Korean on [GeekNews](https://news.hada.io/topic?id=30562).

---

## Framing

- The presenter is a self-described **CLI die-hard** who designed the Claude Code CLI — but stresses the desktop app is more accessible and can do everything shown here. The CLI is a personal preference, not a requirement.
- At Anthropic, the team has constant access and is "always looking for the next way to work and make ourselves more optimal." The demo distills habits that spread internally.
- The demo runs on **Excalidraw** — an open-source product with a great backlog of issues and an open community, recommended as a place to practice.

---

## Core Beliefs (the tenets behind the workflows)

1. **Claude (and most LLMs) are not good at design yet.** You must stay in the loop for craft and decision-making. Most of the workflows are built around the human still owning what actually ships into the product.
2. **Coding is automated, but so is everything around it.** A huge amount of *non-coding* work gets handed to Claude. If you're only automating code, you're not using Claude in its most automated way. Think beyond code when thinking about AI automation.
3. **Just because everyone *can* ship doesn't mean everything *should* ship.** As code access democratizes and anyone can push to production, you need better systems that scale to maintain quality.

---

## Setup Pro Tips

| Tip | What it does |
| --- | --- |
| **Always start in a worktree** | A worktree makes an isolated copy of the repo so you can run multiple Claude sessions in parallel without them conflicting / overriding each other. Start with `claude --worktree`; it auto-checks-out a new branch. Easier to manage than keeping `repo-1`, `repo-2`, `repo-3` copies. |
| **Opus 1M context + fast mode** | The presenter is "always in Opus 1 million context" and "always in fast mode" to move faster. (Acknowledged as not available to every org.) |
| **Auto mode** | Anthropic's workaround for lower permission modes: a classifier detects whether a risky action is being taken, so you don't sit there approving "yes, yes, yes." Everyone at Anthropic runs in auto mode — it's much faster. |
| **Just type (typos OK)** | Claude is good at handling typos; no need to edit prompts for them. |

---

## The Prompting Demo: adding autocomplete to Excalidraw

The prompt was deliberately minimal — *"add a new autocomplete feature, I want to see what it looks like"* — no design specs. Key practices baked into that single prompt:

- **`/prototype` skill (self-built).** Generates *n* options (default 5) of a feature as different implementations, builds an HTML file, previews it, and iterates. Built by asking Claude to build it — *"No one ever writes their skills by hand anymore. If anyone tells you they do, they're lying. Everyone just prompts them."*
- **Let Claude choose first, then explain why.** Instead of "prototype, then I'll choose," the presenter now says "prototype, then *you* choose what to do and tell me why." It's more fun and surfaces Claude's reasoning.
- **Allow online research.** For an open-source repo, "feel free to research online." In a production codebase she'd instead point Claude at Slack, Google Docs, discussions, and **BigQuery** to find the best option.
- **Implement, verify, match styles, and put up a PR with a screenshot.** Claude is good at this. She no longer reviews the raw transcript output — she reviews **the PR Claude opened, which includes a recording/GIF of the implemented feature**.
- **Loop ("loop until you're done").** A standard prompt meaning "keep going until it's fully done."
- **Claude in Chrome for self-verification.** Claude opens Chrome and tests the change directly. For front-end changes this is the best way to let Claude self-verify, and it produces the screenshots/GIFs that go into the PR.

---

## Workflows That Scale Quality

### 1. Claude in the web — hundreds of tiny polish fixes
Uses cloud/web Claude to fire off "hundreds of tiny little polish fixes" spotted in the app — too small to justify a dedicated session. Engineers sometimes complain there are too many, so she occasionally asks Claude to **squash them all into one PR**. Many are tiny CSS changes that get auto-approved without review. A low-effort way to maintain craft quality while Claude is busy elsewhere.

### 2. Always-on PR merging
Almost everyone at Anthropic keeps Claudes running to help **merge PRs**. The presenter is "never in CI" anymore — never manually addressing code-review comments through to merge; it's fully automated. Two pieces:

- **Hygiene before big changes:** internal `simplify` and `code-review` skills prune the codebase, plus a `commit / push / PR` command that runs internal checks. *Ask your engineering team what their equivalents are — every team using AI has them.*
- **Get open PRs over the finish line:** a near-universal internal skill reviews any open PRs and drives them to merge. Because it's wired into **Slack**, it DMs the reviewers on a PR, or DMs the stamp channel / pings whoever is on call. "The full integration of your suite is where it's at."

### 3. Scheduled routine — "shipped without a designer" detector
The most game-changing workflow as the product suite grew. A **Claude Code routine** (a scheduled task in Claude Code) that:

1. Scrapes all repositories for front-end changes anyone shipped.
2. Checks whether a designer was involved — by reading Slack, Google Meet transcripts, Google Docs, any data it can access.
3. If no designer was involved, **flags it as shipped without a designer**, reviews the design, drafts an *adversarial* redesign in a PR, DMs the engineer who shipped it, and asks them to work with a designer.

She had to **turn off the auto-DMing** because Claude's design output was genuinely bad — reinforcing tenet #1. But the point stands: automate not just the first step, but the next, and the next — push it toward building the actual end product. The industry is forgiving right now while everyone tests workflows, and because the routine is already written up, she can re-run it against each new model and it tends to get better.

---

## Takeaways

- Three demoed pillars: **Claude in the web for polish**, **always-on automated PR merging**, and the **scheduled "shipped without a designer" routine**.
- Keep the human in the loop on **design and what ships** — LLMs aren't good at design yet.
- Hand off everything that *isn't* coding, not just the code.
- Write your automations up fully now, so they're ready to consume the next, more capable model the day it lands.

---

## References

- [How Claude Code's lead designer builds with AI — Dive Club (YouTube)](https://www.youtube.com/watch?v=hKeDfupbA4U)
- [Claude Code의 수석 디자이너가 AI로 빌드하는 방법 — GeekNews summary](https://news.hada.io/topic?id=30562)
