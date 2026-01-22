# Beads Demo Outline

A presentation introducing beads and demonstrating manual → agent-driven workflows.

---

## Part 0: What is Beads? (~3-4 min)

**Goal**: Quick context, then get to the demo.

---

### Slide 1: Title + Credibility

**ON SLIDE:**
```
Beads: A Memory System for Coding Agents

github.com/steveyegge/beads
⭐ 11.9k stars  •  👥 176+ contributors  •  📦 Go
```

**TALK ABOUT:**
- "This is Beads, created by Steve Yegge"
- "It's gotten significant traction in the AI coding community"
- "Written in Go, open source, actively maintained"

---

### Slide 2: What is Beads?

**ON SLIDE:**
```
Git-backed JSONL files in .beads/

Survives context loss because it's just files on disk.
```

**TALK ABOUT:**
- "At its core, Beads is dead simple"
- "It stores issues as JSON lines in your repo"
- "No server, no auth, no API limits—just files"
- "When your agent's context resets, the files are still there"

---

### Slide 3: The Problem

**ON SLIDE:**
```
AI agents have amnesia.

• Sessions last ~10 minutes
• Context resets, work is forgotten
• Multi-step projects become chaos
```

**TALK ABOUT:**
- "If you've used Claude Code or Copilot for anything complex, you've felt this"
- "Agent starts strong, then context fills up, session ends"
- "New session: agent has no idea what happened before"

---

### Slide 4: The Descent into Madness

**ON SLIDE:**
```
"I accumulated 605 partially-implemented markdown plan files."
                                        — Steve Yegge
```

**TALK ABOUT:**
- "Steve tried using markdown files to track agent work"
- "Agent starts phase 1 of 6, creates a sub-plan"
- "Context resets. Agent reads phase 3, says 'I'm going to break this into 5 phases'"
- "Eventually declares victory at phase 3 of 5 of phase 3 of 6"
- "Result: hundreds of corrupted plan files, nothing actually done"
- "This is what happens without external memory"

---

### Slide 5: Google Maps for Your Plan

**ON SLIDE:**
```
"Google Maps for your Plan"

A data structure with maps and labels
for pieces of your work.
```

**TALK ABOUT:**
- "Beads isn't a planning tool—it's navigation"
- "Like Google Maps doesn't plan your trip, it shows you where you are"
- "Issues, dependencies, status—all queryable by the agent"

---

### Slide 6: Where Beads Lives

**ON SLIDE:**
```
FUTURE WORK         →  BEADS  ←         PAST WORK
   (vague)         "What matters      (done)
                    right now?"
```

**TALK ABOUT:**
- "Beads isn't for vague future ideas—that's your backlog"
- "It's not for finished work—that's your git history"
- "Beads is the active edge. The work in flight."
- "When an agent asks 'what should I do?'—Beads answers"

---

### Slide 7: The Key Insight

**ON SLIDE:**
```
You CAN use Beads commands yourself.

But why?

You're a human. You use JIRA/Linear.
Agents use Beads.
```

**TALK ABOUT:**
- "This is the mental shift"
- "Beads isn't for you to manage your work"
- "It's for your agents to manage THEIR work"
- "You're the architect. They're the builders."
- "Let me show you what that looks like..."

*→ Transition to Part 1: Manual Beads*

---

## Part 1: Manual Beads (Hands-On)

**Goal**: Teach the mechanics by doing it manually first.

### Demo Flow

1. **Create research issues**

   ```bash
   bd create --title="Research: Beads Agents Deepwiki session" --type=task --priority=2
   bd create --title="Research: Beads Advanced Topic" --type=task --priority=2
   ```
2. **Create a summary epic with dependencies**

   ```bash
   bd create --title="Deep Wiki Research Summary" --type=feature --priority=1
   bd dep add <summary-id> <research-id-1>
   bd dep add <summary-id> <research-id-2>
   ```
3. **Show the tree**

   ```bash
   bd list --tree
   ```

   ```
   ○ beads-demo-tco ● P1 Deep Wiki Research Summary
     ├── ○ beads-demo-9n9 ● P2 Research: Beads Advanced Topic
     └── ○ beads-demo-4gf ● P2 Research: Beads Agents Deepwiki session
   ```
4. **Claim and start work**

   ```bash
   bd update beads-demo-4gf --status=in_progress
   bd show beads-demo-4gf
   ```
5. **Spawn parallel Claude sessions**

   - Open new terminal
   - Run `claude`
   - Paste the issue description as the prompt
   - Watch Claude execute autonomously (fetch URLs, create docs)
6. **Show Claude working** (screenshot moment)

   - Context header displays automatically
   - BLOCKS relationships visible
   - Autonomous fetching and research

7. **Skip forward with pre-cooked result**
   - "Rather than wait, let me show you what this looks like when done"
   - Switch to a terminal with completed output (pre-run)
   - Show the generated artifact (research doc, code, etc.)
   - Close the loop: `bd close <id>` → show it move in bv (`r` to refresh)

### Key Takeaway

> "You now understand the mechanics. But you shouldn't be the one running these commands..."

---

## Interlude: The Claude Plugin

**LIVE DEMO:**

1. Open a fresh Claude Code terminal in the beads-demo project
2. Press `Ctrl+O` to expand and show the session context
3. Point out the injected beads context

**TALK ABOUT (while showing):**
- "See this? I didn't type anything. The plugin injected this."
- "SessionStart hook runs `bd prime` automatically"
- "Shows ready tasks, blocked items, workflow instructions"
- "PreCompact hook preserves this when context gets compressed"
- "This is why agents 'just know' about beads—zero prompting needed"

**Setup:** `bd setup claude` (installs the hooks)

---

## Part 2: Agent-Driven Beads

**Goal**: Show the real power - agents managing beads autonomously, with audience participation.

### Setup (Before Demo)

```bash
# Install bv (beads_viewer) if not already
brew install dicklesworthstone/tap/bv
```

### Screen Layout

```
┌─────────────────────────────────┬─────────────────────────────────┐
│                                 │                                 │
│      Claude Code Terminal       │      bv (Terminal UI)           │
│                                 │                                 │
│   Agent interviewing you...     │   Kanban/tree/graph view        │
│                                 │                                 │
└─────────────────────────────────┴─────────────────────────────────┘
```

**bv features to show:**
- `k` — Kanban board view
- `t` — Tree view (hierarchy)
- `g` — Dependency graph
- `j/k` — Vim-style navigation
- `/` — Fuzzy search

### The Hook

> "We're building a blog about Formula 1, including a section on the **top 3 drivers of all time**."

### Audience Poll

> "Do you want to get interviewed... or should we let Claude decide who the GOAT drivers are?"

**Option A: Interview** — Claude asks YOU who the top 3 are (prepare for debate)
**Option B: Autopilot** — Claude decides... let's see if you agree

---

### Option A: The Interview

**Step 1: Fresh start**
```bash
cd `mktemp -d`
git init --quiet
bd init --quiet
```

**Step 2: Create the epic**
```bash
bd create --type=epic --quiet --title="Build an F1 blog featuring the top 3 drivers of all time"
```

**Step 3: Create interview prompt**
```bash
echo "Interview me about this epic to make a simple PRD. Use bd command to load it. ultrathink and use AskUserQuestion for a few rounds. When you are done, create a prd.md, and use the bd command to decompose it into epics/features/tasks" > interview.md
```

**Step 4: Launch Claude with limited tools**
```bash
cat interview.md | claude --allowedTools "AskUserQuestion,Bash(bd:*)"
```

**The Interview (~3-4 min):**
Claude asks questions, audience shouts answers. Their input becomes the PRD.

---

### Option B: Autopilot

**The prompt:**
```
Design me a blog about Formula 1, featuring a section on the top 3 greatest drivers of all time.

First, output a PRD (Product Requirements Document).
From this PRD, create different beads tasks for the different things that need to be completed.
```

Claude decides everything—including who the GOATs are. Let's see if you agree.

---

### The Payoff (Either Option)

After Claude finishes:

```bash
bd list --tree
```

**In bv (second terminal):**
- Press `r` to refresh
- Press `k` for Kanban view — tasks in Ready/Blocked columns
- Press `t` for Tree view — epic/feature/task hierarchy
- Press `g` for Graph view — dependency visualization

### What the Audience Sees

- Empty project → Fully structured task breakdown
- Epics/features/tasks with dependencies
- (If interview) Their input reflected in the PRD and task titles

**They just watched Claude turn a prompt into a project plan.**

---

## Closing: The Vision

| Before Beads               | With Beads                       |
| -------------------------- | -------------------------------- |
| "What was I working on?"   | `bd list --status=in_progress` |
| "What's blocking what?"    | `bd list --tree`               |
| "What can I do next?"      | `bd ready`                     |
| Context lost on compaction | JSONL persists everything        |
| Single-threaded agent work | Parallel agents, coordinated     |

**The human becomes the architect. The agents become the builders.**

---

## Best Practices (Quick Reference)

**ON SLIDE:**
```
✓ One task per session — complete, close, fresh agent
✓ "Landing the Plane" — end-of-session cleanup ritual
✓ Task granularity — if it takes >2 min, make it a bead
✓ Guardrails — tell agents to verify before moving on
✓ Maintenance — bd doctor daily, bd cleanup weekly
```

**TALK ABOUT:**

- **One task per session**: "Don't let agents churn through everything. One task, close it, start fresh. Beads is the memory between sessions."

- **Landing the Plane**: "Steve Yegge's technique. At the end of every session: commit, sync, clean branches, identify next work. Scripted cleanup."

- **Task granularity**: "If work takes more than ~2 minutes, it deserves a bead. Small tasks = small commits = easier review."

- **Guardrails**: "Without instructions, agents just churn. Tell them: complete one task, verify it works, then stop."

- **Maintenance**: "Run `bd doctor` daily to catch issues. Run `bd cleanup` weekly to keep your issue count manageable (200-500 active max)."

---

## Demo Checklist

**Setup:**
- [ ] Terminal with beads-demo directory
- [ ] `bd list --tree` shows existing issues (or start fresh)
- [ ] bv installed and working (`bv` in a beads project)
- [ ] Second terminal ready for bv

**Part 1 (Manual):**
- [ ] Second terminal ready for parallel Claude session
- [ ] Pre-cooked result ready (completed task output to show)

**Part 2 (Agent-Driven):**
- [ ] Screen split: Claude terminal + bv (second terminal) side by side
- [ ] bv installed (`brew install dicklesworthstone/tap/bv`)
- [ ] Commands ready for Option A (interview) and Option B (autopilot)
- [ ] Know bv keys: `k` (kanban), `t` (tree), `g` (graph), `r` (refresh)

---

## Timing Estimate

| Section               | Duration |
| --------------------- | -------- |
| Part 0: What is Beads | 3-4 min  |
| Part 1: Manual Beads  | 5-7 min  |
| Interlude: Plugin     | 1 min    |
| Part 2: Agent-Driven  | 5-7 min  |
| Best Practices        | 1-2 min  |
| Q&A                   | flexible |

**Total: ~17-22 minutes**
