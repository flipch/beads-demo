# Beads Presentation Slides

Copy-paste ready content for your slide deck.

---

## Slide 1: Title + Credibility

```
Beads: A Memory System for Coding Agents

github.com/steveyegge/beads
⭐ 11.9k stars  •  👥 176+ contributors  •  📦 Go
```

**Speaker notes:**
- Created by Steve Yegge
- Significant traction in AI coding community
- Written in Go, open source, actively maintained

---

## Slide 2: What is Beads?

```
Git-backed JSONL files in .beads/

Survives context loss because it's just files on disk.
```

**Speaker notes:**
- At its core, dead simple
- Stores issues as JSON lines in your repo
- No server, no auth, no API limits—just files
- When agent's context resets, files are still there

---

## Slide 3: The Problem

```
AI agents have amnesia.

• Sessions last ~10 minutes
• Context resets, work is forgotten
• Multi-step projects become chaos
```

**Speaker notes:**
- If you've used Claude Code or Copilot for anything complex, you've felt this
- Agent starts strong, context fills up, session ends
- New session: agent has no idea what happened before

---

## Slide 4: The Descent into Madness

```
"I accumulated 605 partially-implemented markdown plan files."
                                        — Steve Yegge
```

**Speaker notes:**
- Steve tried using markdown files to track agent work
- Agent starts phase 1 of 6, creates a sub-plan
- Context resets. Agent reads phase 3, says "I'm going to break this into 5 phases"
- Eventually declares victory at phase 3 of 5 of phase 3 of 6
- Result: hundreds of corrupted plan files, nothing actually done
- This is what happens without external memory

---

## Slide 5: Google Maps for Your Plan

```
"Google Maps for your Plan"

A data structure with maps and labels
for pieces of your work.
```

**Speaker notes:**
- Beads isn't a planning tool—it's navigation
- Like Google Maps doesn't plan your trip, it shows you where you are
- Issues, dependencies, status—all queryable by the agent

---

## Slide 6: Where Beads Lives

```
FUTURE WORK         →  BEADS  ←         PAST WORK
   (vague)         "What matters      (done)
                    right now?"
```

**Speaker notes:**
- Beads isn't for vague future ideas—that's your backlog
- It's not for finished work—that's your git history
- Beads is the active edge. The work in flight.
- When an agent asks "what should I do?"—Beads answers

---

## Slide 7: The Key Insight

```
You CAN use Beads commands yourself.

But why?

You're a human. You use JIRA/Linear.
Agents use Beads.
```

**Speaker notes:**
- This is the mental shift
- Beads isn't for you to manage your work
- It's for your agents to manage THEIR work
- You're the architect. They're the builders.
- "Let me show you what that looks like..."

---

## Slide 8: Best Practices

```
✓ One task per session — complete, close, fresh agent
✓ "Landing the Plane" — end-of-session cleanup ritual
✓ Task granularity — if it takes >2 min, make it a bead
✓ Guardrails — tell agents to verify before moving on
✓ Maintenance — bd doctor daily, bd cleanup weekly
```

**Speaker notes:**
- One task per session: Don't let agents churn. One task, close it, start fresh.
- Landing the Plane: Steve Yegge's technique. Commit, sync, clean branches, identify next work.
- Task granularity: If work takes >2 minutes, it deserves a bead. Small tasks = easier review.
- Guardrails: Without instructions, agents just churn. Tell them to verify before moving on.
- Maintenance: `bd doctor` daily, `bd cleanup` weekly (keep 200-500 active issues max).

---

## Slide 9: The Vision

```
Before Beads               →    With Beads
─────────────────────────────────────────────────────
"What was I working on?"        bd list --status=in_progress
"What's blocking what?"         bd list --tree
"What can I do next?"           bd ready
Context lost on compaction      JSONL persists everything
Single-threaded agent work      Parallel agents, coordinated
```

```
The human becomes the architect.
The agents become the builders.
```

**Speaker notes:**
- This is the transformation
- From confusion to clarity
- From manual tracking to queryable state
- From single agent to coordinated swarm

---

## Slide 10: Q&A

```
Questions?


Resources:
• GitHub: github.com/steveyegge/beads
• DeepWiki: deepwiki.com/steveyegge/beads
• bv: github.com/Dicklesworthstone/beads_viewer
```

**Speaker notes:**
- DeepWiki can answer AI-backed questions about the codebase
- bv for terminal UI (kanban, tree, graph views)
- QA.md has 30 prepared Q&A if needed
