# Beads: Synthesis for Presentation

## 1. Core Value Propositions (From Real Usage)

### Session Memory & Task Management
Beads provides coding agents with persistent memory across sessions, enabling them to:
- Handle complex, multi-step projects autonomously
- Work for extended periods without losing context
- Maintain a structured work queue with clear priorities

### Structured Alternative to "Markdown Chaos"
- Replaces ad-hoc documentation with an issue-based workflow
- Helps agents avoid "dementia" caused by conflicting or obsolete documents
- Tracks issues, dependencies, and priorities in a systematic way

### Hierarchical Task Organization
- **Epics** → **Tasks** → **Sub-tasks**
- Todo list with explicit dependencies between tasks
- Tasks can be discovered from another (no functionality blocker) or blocked by another (functional dependency)

### Dual Storage Architecture
- Stored in both GitHub (`.beads/*`) and SQLite
- Kept synced to help with merges and collaboration

---

## 2. Practical Benefits Mentioned by Users

| Benefit | Context |
|---------|---------|
| **Staying on track** | Works well for building something new from scratch |
| **Automatic bug filing** | When agent discovers an issue mid-session, it files a detailed bug report to fix later |
| **Better starting points** | Having agent break down PRDs and epics into individual tasks |
| **Context attachment** | Useful context attached to each issue |
| **Dependency resolution** | Ensures blocking issues resolved before dependent work begins |
| **Work Dependency Chain** | Especially valuable with 200k context window constraints |
| **Easier code review** | Small tasks → small targeted changes in sequence |
| **Epic-based workflow** | Give it an epic, it churns through all tasks |

---

## 3. Best Practices & Workflow Tips

### "Landing the Plane" Technique (Steve Yegge)
Scripted cleanup command at the end of every session that:
- Handles commit syncing
- Cleans Git branches
- Identifies next session's work

### Workflow Instructions Are Critical
> "Workflow instructions needed to prevent agent from just churning through tasks"

Without guardrails, agents may work without stopping to verify or check in.

### Task Granularity Matters
- Break work into small tasks
- Small tasks make reviewing easier
- Produces small, targeted changes in sequence

### Multimodal Verification
- Use Playwright to take screenshots
- Feed images back to agent for visual bug verification
- Enables fixing visual bugs through iterative prompting

---

## 4. Common Concerns & Limitations

| Concern | Details |
|---------|---------|
| **Complexity** | *"Insanely complex and lets you turn every single knob"* |
| **Documentation** | Initial setup was confusing; docs aren't great |
| **Context window limits** | Problem for complex work that exceeds token limits |
| **Requires workflow discipline** | Need explicit instructions to prevent uncontrolled task churning |
| **Learning curve** | Setup and configuration not intuitive |

---

## 5. How Beads Compares to Alternatives

### vs. Markdown-based tracking
- Beads provides structure vs. "markdown chaos"
- Prevents conflicting/obsolete document problems
- Explicit dependency tracking

### vs. Traditional issue trackers
- Agent-native: designed for AI workflows
- Dual storage (Git + SQLite) enables both human review and agent efficiency
- Tight integration with coding agents (Claude Code, Codeex, AMP)

### vs. Manual agent prompting
- Persistent memory across sessions
- Automated task discovery and prioritization
- "Work Dependency Chain" prevents wasted context window

---

## 6. GasTown: The Evolution of Beads

### What It Is
- Described as **"Beads on literal steroids"**
- Represents the next evolution of the Beads concept
- More autonomous, more aggressive workflow execution

### Why It Matters
GasTown illustrates both the promise and peril of autonomous agent workflows:

**The Promise:**
> "Working effectively involves committing to vibe coding. Work becomes fluid..."

**The Risk:**
- Caused a SEV at another company by "doing random things"
- Quote: *"Most work gets done; some work gets lost."*

### Implication for Beads
GasTown shows where autonomous agent tooling is heading—and why Beads' more structured, controllable approach may be the safer middle ground for production use.

---

## 7. Key Quotes for Presentation

### On the Future of Engineering
> **Steve Yegge:** The future of engineering involves guiding and supervising AI rather than just writing code.

### On the Core Concept
> "A todo list with dependencies between tasks, so a task can be discovered from another (no functionality), or blocked by another."

### On Complexity (The Criticism)
> "Insanely complex and lets you turn every single knob."

### On GasTown's Double-Edged Nature
> "Working effectively involves committing to vibe coding. Work becomes fluid... Most work gets done; some work gets lost."

### On Practical Value
> "When agent discovered an issue during session, it filed a detailed bug report to fix later."

---

## Summary Slide Concept

```
BEADS IN ONE SENTENCE:
━━━━━━━━━━━━━━━━━━━━━
Persistent, structured task memory for coding agents—
replacing markdown chaos with dependency-aware workflows.

BEST FOR:
✓ Multi-session projects
✓ Complex builds from PRDs
✓ Teams wanting small, reviewable changes

WATCH OUT FOR:
✗ Steep learning curve
✗ Requires workflow guardrails
✗ Context window constraints remain
```
