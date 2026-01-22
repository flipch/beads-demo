# Speaker Notes

## Slide 1: Title
**Beads: A Memory System for Coding Agents**

- "Today I'm going to show you a tool that's changing how I work with AI agents"
- "The tagline says it all: you become the architect, agents become the builders"
- "Created by Steve Yegge, 11.9k GitHub stars, 176+ contributors"
- "Let's dive into what it actually is"

---

## Slide 2: What is Beads?
**Git-backed JSONL files in .beads/**

- "At its core, Beads is dead simple"
- "It stores issues as JSON lines in a `.beads/` folder in your repo"
- "No server, no accounts, no API limits—just files"
- "The key insight: when your agent's context resets, those files are still there"
- "That's the whole magic. Persistence through simplicity."

---

## Slide 3: How It Works
**AGENT → bd CLI → STORAGE**

- "Any agent that can run CLI commands can use Beads"
- "Claude Code, Cursor, Copilot, AMP—doesn't matter"
- "The agent calls `bd` commands: create, update, close, list"
- "Everything gets stored in `.beads/issues.jsonl`"
- "Because it's Git-backed, your team syncs via normal push/pull"
- "No new infrastructure needed"

---

## Slide 4: The Problem
**AI agents have amnesia**

- "If you've used Claude Code or Copilot for anything complex, you've felt this"
- "Sessions last maybe 10 minutes before context fills up"
- "When context resets, the agent forgets everything"
- "You're left picking up the pieces: what was it working on? What's done? What's left?"
- "For multi-step projects, this becomes chaos"

---

## Slide 5: The Descent into Madness
**"I accumulated 605 partially-implemented markdown plan files."**

- "This is Steve Yegge's origin story for Beads"
- "He tried using markdown files to track agent work"
- "Agent starts phase 1 of 6, creates a detailed sub-plan"
- "Context resets. Agent reads phase 3 and says 'I'm going to break this into 5 phases'"
- "Eventually declares victory at phase 3 of 5 of phase 3 of 6"
- "Result: 605 corrupted plan files. Nothing actually done."
- "This is what happens without external memory"

---

## Slide 6: Google Maps for Your Plan
**A data structure with maps and labels for pieces of your work**

- "Here's the key mental model"
- "Beads is NOT a planning tool—it's navigation"
- "Google Maps doesn't plan your road trip. It shows you where you are."
- "Beads shows your agent: here's the work, here's what's done, here's what's blocked"
- "Issues, dependencies, status—all queryable"

---

## Slide 7: Where Beads Lives
**PAST WORK → BEADS → FUTURE WORK**

- "Think of work as a spectrum"
- "Left side: past work. Done. In your git history."
- "Right side: future work. Vague ideas. Your backlog."
- "Beads lives in the middle—the active edge"
- "Not vague future stuff. Not finished stuff. The work in flight."
- "When an agent asks 'what should I do?'—Beads answers that question"

---

## Slide 8: Demo #1 - Core Commands
**bd create, bd dep add, bd list --tree, bd update, bd close**

- "Let me show you the basics"
- "I'll create some tasks manually so you see the mechanics"
- [SWITCH TO TERMINAL - run the demo]
- "You now understand how it works. But here's the thing..."
- "You shouldn't be the one running these commands"

---

## Slide 9: The Key Insight
**You're a human. You use JIRA/Linear. Agents use Beads.**

- "This is the mental shift"
- "Yes, you CAN run bd commands yourself"
- "But why? You have JIRA. You have Linear."
- "Beads is for agents to manage THEIR work"
- "You define the high-level goals. They break it down. They track it. They close it."
- "You're the architect. They're the builders."

---

## Slide 10: Interlude - The Claude Plugin
**SessionStart hook runs bd prime automatically**

- [SHOW Ctrl+O in fresh Claude session]
- "See this? I didn't type anything. The plugin injected this."
- "SessionStart hook: every new session automatically runs `bd prime`"
- "That injects beads context—ready tasks, blocked items, workflow"
- "PreCompact hook: when context gets compressed, beads context survives"
- "This is why agents 'just know' about beads. Zero prompting needed."
- "Install with: `bd setup claude`"

---

## Slide 11: Part 2 - Agent-Driven Beads
**The real power: agents managing beads autonomously**

- "Now let's see the real magic"
- "No manual commands from us"
- "The agent creates issues, tracks progress, closes when done"
- "We're going to build something together—live"

---

## Slide 12: Demo #2 - Build me a Blog about F1
**Option A: Autopilot / Option B: Interview**

- "We're building an F1 blog featuring the top 3 greatest drivers of all time"
- "Quick poll: do you want to get interviewed..."
- "...or should we let Claude decide who the GOATs are?"
- [AUDIENCE PICKS]
- [IF INTERVIEW]: "Claude's going to ask you questions. Shout out answers."
- [IF AUTOPILOT]: "Let's see who Claude picks. Prepare your objections."
- [RUN THE DEMO - show bv updating in real-time]
- "Look at that—a full project breakdown with dependencies, from one prompt"

---

## Slide 13: Best Practices
**One task per session, Landing the Plane, Task granularity, Guardrails, Maintenance**

- "A few quick tips if you start using this"
- "One task per session: complete it, close it, start fresh. Beads is the memory between sessions."
- "Landing the Plane: Steve's end-of-session ritual. Commit, sync, clean branches, identify next work."
- "Task granularity: if it takes more than 2 minutes, make it a bead. Small tasks = easier review."
- "Guardrails: without instructions, agents just churn. Tell them to verify before moving on."
- "Maintenance: `bd doctor` daily, `bd cleanup` weekly to keep issues manageable"

---

## Slide 14: The Vision
**Before Beads vs With Beads**

- "Here's the transformation"
- "Before: 'What was I working on?' After: `bd list --status=in_progress`"
- "Before: 'What's blocking what?' After: `bd list --tree`"
- "Before: context lost on compaction. After: JSONL persists everything"
- "Before: single-threaded agent work. After: parallel agents, coordinated"
- "The human becomes the architect. The agents become the builders."

---

## Slide 15: Questions
**go/beads**

- "That's Beads. Questions?"
- [HAVE QA.md OPEN for tough questions]
- "If you want to try it: go/beads has everything—install instructions, videos, docs"
- "Install is just: `npm install -g @beads/bd` then `bd init` in your project"
- "Thanks for your time"
