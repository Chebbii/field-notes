---
name: reference_ai_workflow_research
description: Proven AI-agent/workflow orchestration techniques (subagent patterns, memory conventions, review loops) a small AI team can adopt.
type: reference
modified: 2026-08-30
---

Living log of external, verified techniques for running a small AI-assisted team's workflows. Each entry: plain-language explanation, verified source, concrete tie-in to how this team could use it. No duplicates — check existing entries before adding.

## New, verified 2026-08-30

### Orchestrator / Lead-Agent + Subagent Pattern
Rather than one large agent trying to do everything in a single context, a "lead" agent receives the task, decides how to break it up, handles some parts directly, and dispatches bounded sub-tasks to separate subagents whose results it synthesizes. Recommended practice is to scope each subagent's tool access to only what its specific sub-task needs, and to keep subagent tasks bounded rather than open-ended so results stay reviewable.
Source: [Multi-agent coordination patterns: five approaches and when to use them](https://claude.com/blog/multi-agent-coordination-patterns); [When to use multi-agent systems (and when not to)](https://claude.com/blog/building-multi-agent-systems-when-and-how-to-use-them)
Team application: For research or content-review tasks that touch multiple unrelated sources (e.g. "check these 5 competitor newsletters"), dispatch one bounded subagent per source with only read/search access, then have the lead agent merge findings into one summary — keeps any single subagent's context from ballooning and keeps each sub-result independently checkable.

### Single Auto-Loaded Memory File, Continuously Appended
A convention now adopted natively across many coding-agent tools (GitHub Copilot, Cursor, Google's Jules, Windsurf, Zed, and others) is a plain markdown file at the project root that the agent reads at the start of every session and appends to when something durable is worth keeping — project rules, decisions, and context living as flat, human-readable text rather than a database or vector store. The guiding heuristic reported by practitioners is "as little as possible, but as alive as possible": keep it short because every line costs attention on each load, but treat it as continuously-updated working knowledge rather than a static one-time-written contract.
Source: [CLAUDE.md and AGENTS.md, in depth: from basics to counterintuitive patterns](https://redreamality.com/blog/claude-md-agents-md-deep-dive/); [Claude Code & Agent memory: best practices for 2026](https://orchestrator.dev/blog/2026-04-06--claude-code-agent-memory-2026/)
Team application: This is effectively the pattern this repo already uses (dated, append-only reference files read in full before each research run) — worth keeping deliberately short per file and periodically pruning stale/superseded entries rather than letting any one file grow without bound, per the "alive, not just static" principle.
