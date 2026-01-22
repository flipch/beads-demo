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
7. **We're going to skip forward** (pretend)
   - Pretend it's over and we have an output
   - You now close the loop -> bd close beads-ID -> bd sync --flush-only

### Key Takeaway

> "You now understand the mechanics. But you shouldn't be the one running these commands..."

---

## Part 2: Agent-Driven Beads

**Goal**: Show the real power - agents managing beads autonomously, visualized in real-time.

### Setup (Before Demo)

```bash
# Install beads-ui (if not already)
npm i beads-ui -g

# In your project directory, start the UI
bdui start --open
```

This opens a local web interface with:
- **Kanban board** — Blocked, Ready, In Progress, Closed columns
- **Real-time sync** — Tasks appear as they're created
- **Epics view** — Progress tracking

### Screen Layout

```
┌─────────────────────────────────┬─────────────────────────────────┐
│                                 │                                 │
│      Claude Code Terminal       │        beads-ui (Browser)       │
│                                 │                                 │
│   Agent creating tasks...       │   Kanban board updating live    │
│                                 │                                 │
└─────────────────────────────────┴─────────────────────────────────┘
```

### Live Demo: F1 Blog

**The prompt:**

```
Design me a blog about Formula 1.

First, output a PRD (Product Requirements Document).
From this PRD, create different beads tasks for the different things that need to be completed.
```

### What to Watch For

1. **PRD Generation** — Claude produces structured requirements
2. **Tasks Appearing** — Watch the Kanban board populate in real-time
3. **Dependency Wiring** — Blocked tasks appear in "Blocked" column
4. **Ready State** — Independent tasks land in "Ready" column

### The Visual Payoff

As Claude works, the audience sees:
- Empty board → Tasks appearing one by one
- Some tasks in "Ready" (can start now)
- Some tasks in "Blocked" (waiting on dependencies)
- Epic progress bars filling in

**No terminal output to parse. Just a live dashboard.**

### Optional: Show Task Movement

If time permits, have Claude start working on a task:
```
Work on the first ready task.
```

Watch it move: **Ready → In Progress → Closed**

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

- [ ] Terminal with beads-demo directory
- [ ] `bd list --tree` shows existing issues (or start fresh)
- [ ] Second terminal ready for parallel Claude session
- [ ] beads-ui running (`bdui start --open`)
- [ ] Browser window with Kanban board visible
- [ ] Screen split: Claude terminal + beads-ui side by side
- [ ] F1 blog prompt ready to paste

---

## Timing Estimate

| Section               | Duration |
| --------------------- | -------- |
| Part 0: What is Beads | 3-4 min  |
| Part 1: Manual Beads  | 5-7 min  |
| Part 2: Agent-Driven  | 5-7 min  |
| Best Practices        | 1-2 min  |
| Q&A                   | flexible |

**Total: ~15-20 minutes**
