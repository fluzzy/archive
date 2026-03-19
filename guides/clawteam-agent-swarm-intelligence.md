# ClawTeam: Agent Swarm Intelligence

> **Source**: [HKUDS/ClawTeam](https://github.com/HKUDS/ClawTeam)
> **Author**: HKUDS (The University of Hong Kong)
> **Fetched**: 2026-03-19
> **Archived**: 2026-03-19

Open-source framework enabling AI agents to self-organize into collaborative teams through swarm intelligence — agents delegate tasks, communicate in real-time, and coordinate across distributed systems autonomously.

---

## Core Architecture

### Isolation Model

ClawTeam uses **git worktrees** to provide each spawned agent with its own isolated environment, branch, and working directory. This allows parallel, non-conflicting work across multiple agents simultaneously.

### Communication Layer

- **File-based coordination** through a shared filesystem
- **Optional ZeroMQ P2P transport** for distributed scenarios
- **Inbox system** for inter-agent messaging
- **Task management** with dependency tracking and blocking relationships

### Execution Backends

- **tmux**: Interactive monitoring of agent sessions
- **subprocess**: Non-interactive or batch execution
- Custom backends can be implemented

---

## Key Features

1. **Agent Spawning**: Leader agents programmatically launch specialized sub-agents via `clawteam spawn`
2. **Task Management**: Create, assign, update, and track tasks across agents with blocking dependencies
3. **Real-Time Coordination**: Agents check inboxes, report status, and share insights without human intervention
4. **Resource Monitoring**: Tmux-based board dashboard shows all agents working simultaneously
5. **Dynamic Resource Allocation**: Leader can reassign tasks and reallocate resources based on performance
6. **CLI-Driven Orchestration**: All coordination happens through CLI commands agents call within their prompts

---

## Supported Agents

- Claude Code
- OpenAI Codex
- nanobot
- Cursor
- OpenClaw
- Any custom CLI agent meeting compatibility requirements

**Compatibility requirements**: must exist on system PATH, run successfully standalone, accept initial task input, and execute within specified working directories.

---

## Installation & Quick Start

```bash
pip install clawteam
# Or from source
git clone https://github.com/HKUDS/ClawTeam.git && cd ClawTeam && pip install -e .
```

Requirements: Python 3.10+, tmux, a compatible CLI agent.

### Agent-Driven (Recommended)

Prompt any installed agent to use ClawTeam:

```
"Build a web app. Use clawteam to split work across multiple agents."
```

The agent handles team creation, worker spawning, and coordination automatically.

### Manual Control

```bash
clawteam team spawn-team my-team -d "Project description"
clawteam spawn --team my-team --agent-name alice --task "Implement OAuth"
clawteam spawn --team my-team --agent-name bob --task "Write tests"
clawteam board attach my-team  # Watch them work
```

---

## Coordination CLI Commands

Workers automatically receive system prompts teaching them to use:

| Command | Purpose |
| --- | --- |
| `clawteam task list` | Check assignments |
| `clawteam inbox send` | Message teammates |
| `clawteam task update` | Report completion |
| `clawteam lifecycle idle` | Signal availability |

---

## Use Cases

### Autonomous ML Research

Orchestrates 8 specialized agents across H100 GPUs, autonomously designing and executing 2,430+ experiments with zero human intervention. Demonstrated improvement from val_bpb 1.044 to 0.977 (6.4% gain).

### Full-Stack Engineering

Agents self-organize around architectural dependencies — one designs API schemas, others implement authentication and database layers in parallel, testers integrate everything automatically.

### Investment Analysis Teams

Pre-built templates spawn 7-agent committees: value investors, growth strategists, technical analysts, fundamentals specialists, sentiment trackers, and risk managers generating signals and recommendations.

---

## Comparison with Traditional Frameworks

| Aspect | ClawTeam | Other Frameworks |
| --- | --- | --- |
| **Orchestrator** | AI agents themselves | Human-written code |
| **Setup** | `pip install` + one prompt | Docker, APIs, YAML configs |
| **Infrastructure** | Filesystem + tmux | Redis, message queues, databases |
| **Agent Support** | Any CLI tool | Framework-specific only |
| **Isolation** | Git worktrees (real diffs) | Containers/virtual environments |

---

## Technical Details

- **Team metadata**: Stored in TOML configuration files with agent profiles, skill definitions, task schemas, and communication templates
- **Worktree management**: Each agent receives a unique git worktree with its own branch, enabling parallel development without merge conflicts
- **Task dependencies**: Support blocking relationships — tasks auto-unblock when dependencies complete

---

## References

- [ClawTeam GitHub Repository](https://github.com/HKUDS/ClawTeam)
