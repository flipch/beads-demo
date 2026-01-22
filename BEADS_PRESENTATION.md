# Beads: A Memory System for Coding Agents

**Presentation for [Organization Name]**

---

## What is Beads?

> **Beads is a distributed, git-backed issue tracker designed specifically for AI coding agents—giving them persistent memory across sessions.**

### The Elevator Pitch

Beads treats Git as a database for task management. Tasks stored as **JSONL files in `.beads/`** making them versionable, branchable, and mergeable like source code.

### Key Stats (January 2026)

| Metric | Value |
|--------|-------|
| GitHub Stars | 11.9k+ |
| Contributors | 176+ |
| Language | Go (94.7%) |

---

## The Problem It Solves

### Agent "Amnesia"

Coding agents suffer from **severe inter-session amnesia**:
- Sessions last ~10 minutes before context limits
- When agents restart, they only know what exists on disk
- Discovered work gets lost
- Long workflows cause agents to lose track

### The "Descent Into Madness" Pattern

Steve Yegge accumulated **605 partially-implemented markdown plan files** before creating Beads.

Without external memory, agents:
1. Start phase 1 of 6, create detailed sub-plan
2. After context reset, read phase 3 and say: *"I'm going to break it into five phases"*
3. Declare entire project done at phase 3 of 5 of phase 3 of 6
4. Result: hundreds of corrupted plan files

### What Beads Fixes

| Problem | Without Beads | With Beads |
|---------|---------------|------------|
| Session continuity | Re-read everything | Query `bd ready --json` |
| Work discovery | Agents ignore problems | Agents file issues |
| Context efficiency | Monolithic sessions | Fine-grained sessions |
| Multi-agent coordination | Conflicts everywhere | Shared database via git |

---

## How It Works

### Architecture

```
┌─────────────────────────────────────────────┐
│  .beads/                                    │
│  ├── issues.jsonl  (all issues as JSON)    │
│  ├── config.json   (settings)              │
│  └── hooks/        (git hooks)             │
└─────────────────────────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────────┐
│  SQLite Local Cache (fast queries)          │
│  (Auto-synced with JSONL)                   │
└─────────────────────────────────────────────┘
```

### Key Design Decisions

- **Git-Native**: Tasks stored as JSONL, versioned like code
- **Dual Storage**: SQLite for speed + JSONL for portability
- **Zero-Conflict IDs**: Hash-based (`bd-a1b2`) prevents merge collisions
- **Background Daemon**: Automatic synchronization

### Hierarchical Structure

```
bd-a3f8          (Epic)
├── bd-a3f8.1    (Task)
│   ├── bd-a3f8.1.1  (Sub-task)
│   └── bd-a3f8.1.2
└── bd-a3f8.2
```

---

## Getting Started

### Installation

```bash
# npm (recommended)
npm install -g @beads/bd

# Homebrew (Mac)
brew install steveyegge/beads/bd

# Go
go install github.com/steveyegge/beads/cmd/bd@latest
```

### Initialize

```bash
cd your-project
bd init
```

### Tell Your Agent

Add to `CLAUDE.md` or `AGENTS.md`:

```markdown
This project uses Beads for issue tracking.
- Use `bd ready` to find available work
- Use `bd create` to file new issues
- Use `bd close <id>` when completing tasks
```

---

## Core Commands

### Finding Work

```bash
bd ready                  # Issues ready to work (no blockers)
bd list --status=open     # All open issues
bd show <id>              # Detailed view
```

### Creating & Managing

```bash
bd create --title="Fix bug" --type=bug --priority=2
bd update <id> --status=in_progress   # Claim it
bd close <id>                          # Complete it
bd close <id1> <id2> <id3>            # Close multiple
```

### Dependencies

```bash
bd dep add <issue> <depends-on>   # Add dependency
bd blocked                         # See blocked issues
```

### Maintenance

```bash
bd doctor        # Check health (run daily)
bd cleanup       # Remove old issues
bd sync --flush-only   # Export to JSONL
```

**Priority Scale**: 0=critical, 1=high, 2=medium, 3=low, 4=backlog

---

## Real-World Benefits

### From Internal Team Discussions

| Benefit | Experience |
|---------|------------|
| **Staying on track** | "Worked well for building something new from scratch" |
| **Auto bug filing** | "When agent discovered an issue, it filed a detailed bug report to fix later" |
| **Better starting points** | "Breaking PRDs into tasks gave better starting points" |
| **Dependency resolution** | "Ensures blocking issues resolved before dependent work" |
| **Easier review** | "Small tasks = small commits = easier code review" |

### Example

Beads dropped on a decade-old TODO list:
- **30 seconds**: Agent came up to speed
- **30 minutes**: Filed 128 issues (6 epics, 5 sub-epics)
- **Result**: Immediately identify top-priority ready work

---

## Best Practices

### 1. Single Task Per Session

```
Complete one task → Terminate → Fresh agent for new work
```

Beads is the persistent memory between sessions.

### 2. "Landing the Plane"

End-of-session cleanup:
- Commit syncing
- Clean Git branches
- Identify next session's work

> *"Generally applicable even without Beads"* — Steve Yegge

### 3. Task Granularity

Create issues for work **exceeding ~2 minutes**.

### 4. Workflow Guardrails

Without instructions, agents churn through tasks without verification.

Add to agent instructions:
```
- Complete one task at a time
- Verify before moving on
- Stop for review after completing an epic
```

### 5. Regular Maintenance

```bash
bd doctor   # Daily
bd cleanup  # Every few days (target 200-500 active issues)
bd upgrade  # Bi-weekly
```

---

## Concerns & Limitations

| Concern | Details |
|---------|---------|
| **Complexity** | *"Insanely complex and lets you turn every single knob"* |
| **Documentation** | Initial setup confusing; docs improving |
| **Context limits** | Beads helps but doesn't eliminate token limits |
| **Learning curve** | Expect 1-2 sessions to get comfortable |
| **Stability** | Beta quality (v0.22+); run `bd doctor` when issues arise |

### Mitigation

- Start simple: `bd create`, `bd ready`, `bd close`
- Add dependencies later
- Explore advanced features as needed

---

## Gas Town: Where Beads is Heading

### What is Gas Town?

Steve Yegge's **multi-agent orchestrator**—runs 20-30 Claude Code instances simultaneously.

> *"Beads on literal steroids"*

### The MEOW Stack (Molecular Expression of Work)

| Component | Description |
|-----------|-------------|
| **Beads** | Atomic work units (git-backed issues) |
| **Epics** | Hierarchical containers |
| **Molecules** | Durable workflows surviving crashes |
| **Formulas** | Template definitions |

### Agent Roles

- **Mayor**: Coordinates work
- **Polecats**: Worker agents
- **Refinery**: Handles merges
- **Witness**: Fixes stuck workers
- **Deacon**: Maintenance workflows

### Why This Matters

> *"Working effectively involves committing to vibe coding. Work becomes fluid... Most work gets done; some work gets lost."*

**Risk**: Gas Town reportedly caused a SEV at another company by "doing random things."

**Implication**: Beads' structured approach may be the safer middle ground.

---

## Should We Use It?

### Best For

- Multi-session projects (days/weeks)
- Complex builds from PRDs
- Teams wanting small, reviewable changes
- Developers already using AI agents daily

### Not Ideal For

- Simple, single-session tasks
- Teams not using AI agents
- Zero learning curve requirement

### Comparison

| Aspect | Markdown | GitHub Issues | Beads |
|--------|----------|---------------|-------|
| Agent-native | No | No | **Yes** |
| Queryability | Text parsing | API | **JSON** |
| Session persistence | Re-read | Context | **Immediate** |
| Merge conflicts | Manual | N/A | **AI-resolved** |

---

## Key Quotes

> *"After using Beads, going back to markdown TODOs feels like trying to remember a phone number without writing it down."*
> — Claude

> *"The future of engineering involves guiding and supervising AI rather than just writing code."*
> — Steve Yegge

> *"Beads isn't 'issue tracking for agents'—it's external memory for agents."*
> — Steve Yegge

---

## Quick Reference

```
┌─────────────────────────────────────────────────┐
│              BEADS QUICK REF                    │
├─────────────────────────────────────────────────┤
│  SETUP                                          │
│    npm install -g @beads/bd                     │
│    bd init                                      │
│                                                 │
│  DAILY WORKFLOW                                 │
│    bd ready           # Find work               │
│    bd show <id>       # Review                  │
│    bd update <id> --status=in_progress          │
│    bd close <id>      # Complete                │
│                                                 │
│  CREATE ISSUES                                  │
│    bd create --title="..." --type=task -p 2    │
│    bd dep add <issue> <depends-on>              │
│                                                 │
│  MAINTENANCE                                    │
│    bd doctor / bd cleanup / bd sync             │
└─────────────────────────────────────────────────┘
```

---

## Resources

- **GitHub**: https://github.com/steveyegge/beads
- **Introducing Beads**: https://steve-yegge.medium.com/introducing-beads-a-coding-agent-memory-system-637d7d92514a
- **Best Practices**: https://steve-yegge.medium.com/beads-best-practices-2db636b9760c
- **Beads Blows Up**: https://steve-yegge.medium.com/beads-blows-up-a0a61bb889b4
- **Video**: https://www.youtube.com/watch?v=s96O9oWI_tI
- **Gas Town Decoded**: https://www.alilleybrinker.com/mini/gas-town-decoded/

---

## Q&A

*Questions?*
