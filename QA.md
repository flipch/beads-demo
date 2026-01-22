# Beads Q&A

> **Resource**: For AI-backed questions about Beads, use [DeepWiki](https://deepwiki.com/steveyegge/beads/1-overview) — it can answer questions using the full codebase as context.

---

## Clarifying Questions

### Q1: What exactly is a "bead" - is it a data structure, a file format, or something else entirely?

A bead is an **issue** (like a JIRA ticket). It's stored as a JSON line in a `.beads/issues.jsonl` file in your repo. Each bead has:
- ID (hash-based, e.g., `bd-a3f8`)
- Title, description, type (task/bug/feature/epic)
- Status (open/in_progress/closed)
- Priority (0-4)
- Dependencies on other beads

**Mental model**: Think of beads as lightweight tickets that live in your git repo.

---

### Q2: When you say "memory" for agents, are we talking about short-term context within a session, or persistent long-term memory across sessions?

**Persistent long-term memory across sessions.** That's the whole point.

Agent sessions last ~10 minutes before context fills up. When a session ends, the agent forgets everything. Beads persist as files on disk, so the next agent session can query `bd ready` and know exactly what work is in flight.

---

### Q3: How is this different from just stuffing previous conversation history into the context window?

Three key differences:

1. **Structured vs. unstructured**: Beads are queryable (`bd ready`, `bd blocked`). Conversation history is just text.
2. **Survives context limits**: Conversation history gets truncated. Beads are files that persist forever.
3. **Dependency-aware**: Beads track what blocks what. Conversation history has no structure for this.

---

### Q4: Is Beads tied to a specific LLM provider, or is it model-agnostic?

**Model-agnostic.** Beads works with:
- Claude Code
- GitHub Copilot
- Gemini
- Cline
- Amp
- Any agent that can run CLI commands

It's just a CLI (`bd`) that outputs JSON. Any agent that can read JSON can use it.

---

### Q5: What's the relationship between a bead and a prompt? Are they the same thing?

No. A bead is a **work item** (like a ticket). A prompt is what you say to an agent.

You might prompt: "Work on the highest priority ready task."
The agent runs `bd ready`, picks a bead, and works on it.

---

### Q6: Does each agent have its own bead storage, or is memory shared across agents?

**Shared.** All agents read from the same `.beads/` directory in the repo.

This enables multi-agent coordination:
- Agent A creates a bead
- Agent B queries `bd ready` and sees it
- Agents can filter by `--assignee` to avoid stepping on each other

---

## Skeptical Questions

### Q7: Why not just use RAG with a vector database? What does Beads do that ChromaDB or Pinecone can't?

RAG retrieves *information*. Beads tracks *work*.

- RAG: "What does the authentication module do?" → retrieves relevant docs
- Beads: "What should I work on next?" → returns prioritized tasks with dependencies

They solve different problems. You might use both—RAG for knowledge retrieval, Beads for task management.

---

### Q8: We already have conversation logging - why do we need a separate "memory" system?

Conversation logs are **write-only**. You can't query "what's still in progress?" or "what's blocked?"

Beads is a **queryable database** disguised as files. The agent can ask structured questions and get structured answers.

---

### Q9: How is this better than just fine-tuning a model on our codebase?

Fine-tuning teaches patterns. It doesn't track state.

A fine-tuned model might know *how* to write code in your style. It still won't remember that it started a refactoring task yesterday and left 3 files incomplete.

Beads tracks **state**, not **knowledge**.

---

### Q10: Couldn't we achieve the same thing with well-structured system prompts?

System prompts get truncated when context fills up.

Steve Yegge tried this—he accumulated 605 partially-implemented markdown plan files. Agents would read phase 3 and say "I'm going to break this into 5 phases," creating recursive chaos.

Beads survives context resets because it's files on disk, not tokens in a window.

---

### Q11: What prevents this from becoming yet another tool that adds complexity without proportional value?

Valid concern. Mitigation:

1. **Start simple**: Just `bd create`, `bd ready`, `bd close`
2. **It's just files**: No server, no accounts, no infra
3. **Agents use it, not you**: You use JIRA/Linear. Agents use Beads.

If it adds friction without value, stop using it. But users report the opposite—agents stay on track instead of wandering.

---

### Q12: Why would I use this instead of just keeping good documentation that agents can reference?

Documentation is static. Beads is dynamic.

Docs: "Here's how our auth system works."
Beads: "Auth refactoring is 40% complete. Tasks 3-5 are blocked by task 2."

You need both. Docs for knowledge, Beads for work state.

---

### Q13: If this is so useful, why isn't it built into the major LLM providers' APIs already?

It might be eventually. But:

1. **Different problem domain**: LLM providers focus on inference, not workflow orchestration
2. **Git integration**: Beads is designed to live in your repo, version-controlled with your code
3. **Early days**: Beads launched ~2 months ago and has 11.9k stars. Providers may adopt similar patterns.

---

## Practical Implementation Questions

### Q14: How does Beads handle conflicting memories - what if the agent "remembers" something that's now outdated?

Issues can be updated or closed. If a bead references outdated info:

1. Agent (or human) closes it with a reason
2. Agent creates a new bead with current info
3. Old beads remain in git history for audit trail

Run `bd cleanup` periodically to remove stale issues.

---

### Q15: What's the storage footprint? How much disk space are we talking for a typical project?

Minimal. Beads are JSON lines—a few KB per issue.

Steve Yegge recommends keeping 200-500 active issues max via `bd cleanup`. At that scale, you're looking at maybe 1-2 MB total including SQLite cache.

---

### Q16: How do you version control beads? Do they live in the git repo?

**Yes, they live in the git repo.** The `.beads/` directory contains:
- `issues.jsonl` — all issues as JSON lines
- `config.json` — settings

Beads are committed and pushed like code. This means:
- Full history in git
- Branch-able (feature branches can have different beads)
- Merge-able (with AI-assisted conflict resolution)

---

### Q17: What happens when a bead references code that's been deleted or significantly refactored?

The bead becomes stale. Options:

1. Close it as obsolete: `bd close <id> --reason="code removed in refactor"`
2. Update it to reference new code
3. Let `bd cleanup` remove old closed issues

Beads doesn't auto-detect code changes—it's an issue tracker, not a code analyzer.

---

### Q18: How does search/retrieval work? Is it keyword-based, semantic, or something else?

**Structured queries**, not semantic search.

```bash
bd list --status=open --priority=1    # High priority open issues
bd ready                               # Unblocked issues
bd blocked                             # Issues waiting on dependencies
bd show <id>                           # Full issue details
```

For semantic search, you'd combine Beads with RAG.

---

### Q19: Can I manually edit or curate the beads, or is it purely automated?

**Both.** You can:

- Use CLI: `bd update <id> --title="New title"`
- Edit `.beads/issues.jsonl` directly (it's just JSON)
- Use bv terminal UI for visual editing (kanban, tree, graph views)
- Let agents manage everything

Most users let agents handle it, but human curation is supported.

---

### Q20: What's the latency impact? How much slower are agent responses when Beads is involved?

**Negligible.** Beads is local-first:

- SQLite cache for fast queries
- No network calls (unless syncing to remote git)
- `bd ready` returns in milliseconds

The bottleneck is always the LLM, not Beads.

---

## Adoption and Team Workflow Concerns

### Q21: If we adopt this, what's the migration path for our existing agent setups?

1. `npm install -g @beads/bd`
2. `bd init` in your project
3. Add one line to your agent instructions: "Use `bd ready` to find work"

That's it. Beads is additive—it doesn't replace anything, just adds memory.

---

### Q22: How do beads work in a multi-developer environment - do we get merge conflicts?

**Rarely, and they auto-resolve.**

- Hash-based IDs (e.g., `bd-a3f8`) prevent collision
- Multiple devs creating issues simultaneously won't conflict
- If conflicts occur, Beads has AI-assisted merge resolution

The daemon auto-syncs changes via git.

---

### Q23: What's the learning curve for the team? How long before we see productivity gains?

**Minimal for devs.** You barely interact with Beads—agents do.

Learning curve:
- 5 minutes: Basic commands (`bd ready`, `bd close`)
- 1-2 sessions: Comfortable with workflow
- 1 week: Muscle memory

Agents pick it up instantly from instructions in `AGENTS.md`.

---

### Q24: Does this require any infrastructure changes, or is it purely local/client-side?

**Purely local.** No servers, no cloud, no infrastructure.

- CLI runs locally
- Data stored in git repo
- Optional daemon for auto-sync (local process)

The only "infrastructure" is git, which you already have.

---

### Q25: How do we handle beads that contain sensitive information like API keys that got accidentally logged?

Same as any git content:

1. Remove the sensitive bead: `bd close <id>` + `bd cleanup`
2. Scrub git history if needed (git filter-branch or BFG)
3. Rotate the exposed credentials

Prevention: Train agents not to include secrets in issue descriptions.

---

### Q26: What happens if different team members have different beads for the same project?

If you're on different branches, you'll have different beads—that's expected (like code).

If you're on the same branch, run `bd sync` to pull latest. The daemon can auto-sync.

---

## Security and Privacy Questions

### Q27: Are beads ever sent to external servers, or do they stay entirely local?

**Entirely local.** Beads only go where your git repo goes.

- Local disk: always
- GitHub/GitLab: only if you push
- No Beads servers, no telemetry endpoints

---

### Q28: How do we ensure proprietary code patterns stored in beads don't leak across projects?

Beads are per-repo. Each project has its own `.beads/` directory.

- No cross-project sharing by default
- No central Beads server aggregating data
- Your repo permissions = your beads permissions

---

### Q29: Is there any telemetry or data collection we should know about?

**No.** Beads is open source (MIT license), written in Go. No phone-home.

You can audit the source: https://github.com/steveyegge/beads

---

## Edge Cases and Failure Modes

### Q30: A task is taking too long and context is about to compact. How does Beads help?

**Honest answer: Beads doesn't solve this automatically.**

**What Beads does:**
- The issue stays marked `in_progress`
- After compaction, agent can see "I was working on bd-xyz"
- Context about *what* the task is survives

**What Beads doesn't do:**
- Monitor context usage
- Auto-spawn new instances
- Break down tasks mid-flight
- Checkpoint progress within a task

**The gap:** If Claude is 70% through a task and context compacts, that 70% progress is lost. The bead just says "align homepage to Figma" — not "I already fixed the header and nav, still need footer."

**How to mitigate:**

1. **Task granularity** — Break big tasks into smaller beads upfront
2. **Agent instructions** — Tell agents: "If a task is taking many iterations, break it into sub-tasks"
3. **Progress checkpointing** — Agent can update the issue description with progress notes
4. **Human oversight** — Check in on long-running tasks (`bd list --status=in_progress`)

**Bottom line:** Beads solves "what was I working on" — not "save my mid-task progress." That requires discipline (smaller tasks) or tooling that doesn't exist yet.

---

### Q31: What happens when the accumulated beads exceed the model's context window - which memories get dropped?

Beads are **queried, not loaded in full**.

When an agent runs `bd ready`, it gets a short list of actionable items—not the entire issue database. The agent works on one task at a time.

For this to work well:
- Run `bd cleanup` to keep issue count manageable (200-500)
- Use fine-grained tasks (one bead = one focused task)
- Agents query specific views, not the whole database

If `issues.jsonl` gets too large, `bd cleanup` prunes old closed issues (they remain in git history).
