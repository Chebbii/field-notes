---
name: reference_ai_workflow_research
description: Proven AI-agent/workflow orchestration techniques (subagent patterns, memory conventions, review loops) a small AI team can adopt.
type: reference
modified: 2026-09-02
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

## New, verified 2026-09-02

### LLM-as-Judge Review Pattern
A second LLM call evaluates a first model's output against an explicit rubric (task description, the output or full trajectory, and optionally a reference answer) rather than relying on a human to check every result. Two distinct uses: an offline evaluation pattern for measuring quality on a sample of runs, and a real-time safety/quality gate inserted before a consequential action is allowed to proceed. Best practice is to calibrate the judge against a set of human-labeled examples first, since an uncalibrated judge is only a guess in a lab coat, and to periodically re-check it as the underlying task or model changes.
Source: [How to Calibrate LLM-as-Judge with Human Corrections (LangChain)](https://www.langchain.com/resources/llm-as-a-judge); [LLM as a Judge: Evaluate LLM and Agent Outputs (Mastra)](https://mastra.ai/articles/llm-as-a-judge)
Team application: For content pieces drafted by one agent (e.g. a social caption or newsletter blurb), route the draft through a second, separately-prompted "judge" pass scored against a short rubric (on-brand tone, no unverified claims, source cited) before it reaches a human for final approval — catches obvious misses without adding a manual review step for every single piece.

### Context Compaction for Long-Running Agent Sessions
As a single agent session's conversation grows, irrelevant or stale tool output and old intermediate state accumulate and silently degrade the model's attention to what actually matters ("context rot") — without throwing an error, so it's easy to miss. The 2026 industry-converged fix, alongside prompt caching, is compaction: summarizing the work done so far and continuing in a fresh window, done as a deliberate response to context growing too large rather than as a default habit (with caching, keeping full context is often cheaper and more accurate than compacting). Complementary techniques include structured note-taking (the agent writes intermediate findings to files instead of holding them in context) and delegating bounded sub-tasks to fresh subagents.
Source: [Context engineering for AI agents in 2026: write, select, compress, isolate, and the four ways long contexts fail (Reactify Solutions)](https://www.reactify-solutions.com/articles/context-engineering-ai-agents-2026); [Compaction: How Long-Running Agents Beat the Context Rot Problem (Medium)](https://medium.com/@pankaj_pandey/compaction-how-long-running-agents-beat-the-context-rot-problem-fc12d4cdeb7b)
Team application: For any recurring research or content-review agent (like this repo's own standing research runs), prefer writing durable findings straight to a memory file over trying to hold the whole research trail in one long session — matches this repo's own append-only file convention, and is the reason a single session should stay scoped to one bounded pass rather than accumulating unrelated work across many runs.
