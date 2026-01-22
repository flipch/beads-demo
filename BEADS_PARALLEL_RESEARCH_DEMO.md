# Beads Parallel Research Demo

A hands-on introduction to using beads for parallel deep dives and research tasks.

---

## Step 1: Discover Your Research Topics

Before creating issues, identify the areas you want to explore. For a deep wiki dive, browse the wiki structure to find distinct topic areas worth investigating.

```bash
# Example: Check what exists
curl -s https://deepwiki.com/steveyegge/beads | head -100
```

---

## Step 2: Create Parallel Research Issues

Create one issue per topic area. Each becomes an independent workstream:

```bash
# Create research issues (adjust topics to your needs)
bd create --title="Research: Beads Architecture & Core Concepts" --type=task --priority=2
bd create --title="Research: CLI Commands & Workflows" --type=task --priority=2
bd create --title="Research: Sync & Collaboration Features" --type=task --priority=2
bd create --title="Research: Integration Patterns (Claude Code, etc)" --type=task --priority=2
bd create --title="Research: Data Model & JSONL Format" --type=task --priority=2
```

---

## Step 3: Add a Summary Epic

Create an epic that consolidates all findings. Use dependencies to ensure the summary waits for all research to complete:

```bash
# Create the summary epic
bd create --title="Deep Wiki Research Summary" --type=feature --priority=1

# Make the summary depend on all research tasks
bd dep add <summary-id> <research-id-1>
bd dep add <summary-id> <research-id-2>
bd dep add <summary-id> <research-id-3>
# ... add all research issues as dependencies
```

---

## Step 4: Work in Parallel

Check what's ready and claim issues to work on:

```bash
# See all available work
bd ready

# View the tree structure
bd list --tree

# Claim issues you're starting
bd update beads-xxx --status=in_progress
bd update beads-yyy --status=in_progress
```

---

## Step 5: Parallel Claude Code Sessions

For each research task, spawn a dedicated Claude Code session:

1. **Get the issue description:**
   ```bash
   bd show beads-xxx
   ```

2. **Copy the description/title**

3. **Start a new Claude Code session (new terminal):**
   ```bash
   claude
   ```

4. **Paste the task as your prompt:**
   ```
   Research: Beads Architecture & Core Concepts

   <paste full description here>
   ```

5. **Repeat for each parallel workstream** - open multiple terminals, each with its own Claude Code session working on a different research issue.

---

## Step 6: Claude Executes the Task

When you paste the issue into Claude Code, the magic happens:

1. **Context loads automatically** - Issue metadata, status, priority, and blocking relationships display at the top of the session

2. **Description becomes instructions** - Claude treats your issue description as the task specification and begins executing immediately

3. **Autonomous research begins** - Based on the description, Claude will:
   - Fetch URLs you specified
   - Search for additional information
   - Navigate subsections systematically
   - Create output files as directed

4. **Output goes where specified** - If your description says "output findings as `docs/research/`", that's where the results land

**Example session flow:**
```
◐ beads-demo-4gf · Research: Beads Agents Deepwiki session    [● P2 · IN_PROGRESS]

DESCRIPTION
Please head over to https://deepwiki.com/steveyegge/beads/9 this should have 3 sections 9.1 9.2
and 9.3. Output your findings as a docs/research/ how agents should use beads.

BLOCKS
← ○ beads-demo-tco: Deep Wiki research Summary ● P1
← ○ beads-demo-9n9: Research: Beads Advanced Topic ● P2
```

Claude then autonomously:
```
● Fetch(https://deepwiki.com/steveyegge/beads/9)
  └ Received 1.7MB (200 OK)

● Fetch(https://deepwiki.com/steveyegge/beads/9-1)
● Fetch(https://deepwiki.com/steveyegge/beads/9-2)
● Fetch(https://deepwiki.com/steveyegge/beads/9-3)
  └ Received 1.7MB each (200 OK)

● Web Search("site:deepwiki.com steveyegge beads agents...")
  └ Found 10 results
```

The agent works independently while you spin up other sessions for parallel issues.

---

## Example Session Tree

```
❯ bd list --tree
○ beads-demo-tco ● P1 Deep Wiki Research Summary
  ├── ○ beads-demo-9n9 ● P2 Research: Beads Advanced Topic
  └── ◐ beads-demo-4gf ● P2 Research: Beads Agents Deepwiki session

Status: ○ open  ◐ in_progress  ● blocked  ✓ closed  ❄ deferred
```

---

## Closing the Loop

When research is complete:

```bash
# Close completed research issues
bd close beads-xxx beads-yyy beads-zzz

# Once all dependencies close, the summary epic becomes unblocked
bd ready  # Summary should now appear

# Work on the summary, then close it
bd update beads-summary --status=in_progress
# ... consolidate findings ...
bd close beads-summary

# Export your work
bd sync --flush-only
```

---

## Why This Works

- **Parallelism**: Multiple Claude sessions = multiple researchers working simultaneously
- **Tracking**: Beads keeps each workstream organized and visible
- **Dependencies**: Summary epic auto-unblocks when all research completes
- **Persistence**: All context survives session boundaries via JSONL export

---

# Part 2: Agent-Driven Beads

> **Key insight**: We shouldn't interact with beads manually. Our agents should.

## The Beads Skill

Instead of running `bd` commands yourself, Claude Code can manage beads directly:

```bash
# One-time setup - installs the beads skill into Claude Code
bd setup claude
```

Now Claude can create, update, and close beads issues autonomously as it works.

---

## Demo: From Idea to Parallel Execution

Watch an agent take a single prompt and decompose it into tracked, parallel workstreams.

### The Prompt

```
Design me a blog about Formula 1.

First, output a PRD (Product Requirements Document).
From this PRD, create different beads tasks for the different things that need to be completed.
```

### What Happens

1. **PRD Generation** - Claude analyzes the request and produces a structured PRD covering:
   - Target audience
   - Core features (race coverage, driver profiles, historical data, etc.)
   - Technical requirements
   - Content strategy

2. **Task Decomposition** - Claude reads its own PRD and creates beads issues:
   ```bash
   bd create --title="Set up blog infrastructure (Next.js/Astro)" --type=task --priority=1
   bd create --title="Design homepage and race calendar UI" --type=task --priority=2
   bd create --title="Build driver profile component" --type=task --priority=2
   bd create --title="Implement race results data integration" --type=task --priority=2
   bd create --title="Create article/news CMS structure" --type=task --priority=2
   bd create --title="Design F1 team pages with livery colors" --type=task --priority=3
   ```

3. **Dependency Wiring** - Claude establishes the execution order:
   ```bash
   bd dep add <ui-task> <infrastructure-task>  # UI needs infra first
   bd dep add <data-task> <infrastructure-task>  # Data needs infra first
   ```

4. **Parallel Handoff** - Multiple agents can now pick up independent tasks:
   ```
   ❯ bd ready
   ○ beads-f1-xxx ● P1 Set up blog infrastructure (Next.js/Astro)
   ```

### The Result

One prompt → PRD → Multiple tracked issues → Parallel agent execution

The human provides direction. The agents handle decomposition, tracking, and execution.

---

## Why Agent-Driven Beads?

| Manual Beads | Agent-Driven Beads |
|--------------|-------------------|
| Human runs `bd create` | Agent decomposes work automatically |
| Human tracks dependencies | Agent wires dependencies from understanding |
| Human assigns work | Agents self-coordinate via `bd ready` |
| Human remembers context | Beads persists context across sessions |

The human becomes the architect. The agents become the builders.
