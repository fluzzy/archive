# How I'm Productive with Claude Code

> **Source**: [neilkakkar.com](https://neilkakkar.com/productive-with-claude-code.html), [GeekNews](https://news.hada.io/topic?id=27817)
> **Author**: Neil Kakkar
> **Fetched**: 2026-03-30
> **Archived**: 2026-03-30

After six weeks at Tano, Neil Kakkar's commit history dramatically increased — not by working harder, but by systematically removing friction from his development workflow. He shifted from individual contributor to "agent manager," applying constraint theory to eliminate bottlenecks one by one.

---

## Key Transformation: IC to Agent Manager

Kakkar reframed his role from "the implementer" to "the manager of agents doing the implementation." Like managing a 10-person team, the highest-leverage work became building infrastructure that enables agents to work efficiently, not crafting individual features.

The new workflow: "I'm in a tight loop: kick off a task, the agent writes code, I check the preview, read the diff, give feedback or merge."

---

## Four Friction-Reduction Strategies

### 1. Automating PR Creation (`/git-pr` Skill)

Built a Claude Code skill automating the entire PR workflow: staging changes, writing commit messages, crafting descriptions, and pushing to GitHub.

The benefit extended beyond time savings — "the real unlock was the mental overhead removed." Context switching costs disappeared. Agent-generated PR descriptions proved more thorough than manual ones because they analyzed complete diffs systematically.

### 2. Eliminating Build Wait Times (SWC Migration)

Switched the build system to SWC, reducing server restarts from ~60 seconds to under one second.

"Sub-second restarts mean you never leave the flow." Long wait times create attention gaps; instant feedback maintains momentum and focus. This seemingly minor improvement had profound impact on the entire development loop.

### 3. Delegating UI Verification (Preview Feature)

Rather than manually previewing every UI change, enabled Claude Code to set up previews, persist session data, and verify results independently.

"Agents could run much longer without oversight. They'd catch their own mistakes." The developer only intervenes for final review, increasing agent autonomy and reducing interruptions.

### 4. Enabling Parallel Work (Dynamic Port Management)

The critical bottleneck: running multiple worktrees simultaneously. Each server instance tried binding to identical ports, creating conflicts.

Built a system assigning unique port ranges per worktree: "I went from getting overwhelmed by two parallel branches to running five worktrees at once." Multiple agents work independently without port collisions.

---

## The Constraint Theory Loop

Each solved constraint revealed the next bottleneck:

1. **Formatting friction** — automated PR creation removed mental overhead
2. **Wait friction** — SWC migration eliminated rebuild delays
3. **Verification friction** — preview automation enabled self-checking agents
4. **Context switching friction** — dynamic port management enabled true parallel development

---

## Key Insight

"It's the infrastructure, not the AI" — raw AI capability matters less than removing environmental friction. Proper tooling determines whether developers experience flow or wrestle their environment continuously.

The pattern: building the infrastructure that turned a trickle of commits into a flood.
