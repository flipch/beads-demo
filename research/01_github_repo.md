# Beads: Comprehensive Overview

## What Beads Is

Beads is a distributed, git-backed graph issue tracker specifically designed for AI agents. It provides what the project describes as "a persistent, structured memory for coding agents," replacing traditional markdown task lists with a dependency-aware system that maintains context across long-running development sessions.

## Core Concept & Elevator Pitch

The fundamental innovation is treating Git as a database for task management. Rather than storing issues in external systems, Beads keeps them as JSONL files in a `.beads/` directory, making them versionable, branchable, and mergeable like source code. This approach enables agents to maintain coherent task hierarchies without losing track of progress.

## Key Features

**Git-Native Storage:** Tasks stored as JSONL in `.beads/` directory, leveraging existing version control workflows.

**Agent Optimization:** The system outputs JSON natively, includes automatic dependency tracking, and identifies tasks ready for work without blockers.

**Zero-Conflict IDs:** Hash-based identifiers (format: `bd-a1b2`) prevent merge collisions when multiple agents or branches create tasks simultaneously—a critical advantage in multi-agent scenarios.

**Performance Infrastructure:** SQLite provides local caching for speed while a background daemon handles automatic synchronization.

**Memory Management:** The system implements semantic summarization of completed tasks to compress context usage over time.

## Technical Architecture

The implementation is written primarily in Go (94.7% of the codebase), with supporting Python (3.6%) and shell scripting (0.8%). The repository contains approximately 5,440 commits and spans multiple directories including core commands (`cmd/bd`), integrations, documentation, and community tools.

## Installation Options

- **npm:** `npm install -g @beads/bd`
- **Homebrew:** `brew install steveyegge/beads/bd`
- **Go:** `go install github.com/steveyegge/beads/cmd/bd@latest`
- **Shell script:** Curl-based installation for macOS/Linux/FreeBSD

Supported platforms include Linux, FreeBSD, macOS, and Windows.

## Core Commands & Workflow

| Command | Purpose |
|---------|---------|
| `bd ready` | Display tasks with no active blocking issues |
| `bd create "Title" -p 0` | Create priority-0 tasks |
| `bd dep add <child> <parent>` | Establish task relationships |
| `bd show <id>` | Review task details and change history |

## Hierarchical Structure

Beads supports nested task organization:
- `bd-a3f8` (Epic level)
- `bd-a3f8.1` (Task level)
- `bd-a3f8.1.1` (Sub-task level)

## Agent Integration

The README explicitly recommends documenting agent capabilities in an `AGENTS.md` file. Setup involves running `bd init` once, then informing agents about Beads availability.

**Stealth Mode:** Users can run `bd init --stealth` to maintain local Beads operations without committing the `.beads/` directory to shared repositories—useful for personal task tracking on collaborative projects.

## Unique Selling Points

The system addresses specific pain points in agent-driven development: maintaining task context across long operations, preventing ID collisions in distributed workflows, and integrating naturally with existing Git workflows rather than requiring separate infrastructure.

The project has gained significant adoption, with 11.9k GitHub stars, 721 forks, and contributions from 176+ developers.
# Beads: AI Agent Task Management

## Core Concept
Beads is a **distributed, git-backed graph issue tracker** designed specifically for AI agents. According to the repository, it provides "persistent, structured memory for coding agents," replacing unstructured markdown plans with dependency-aware task graphs.

## Key Features for Agent Integration

**Storage & Sync:**
- Tasks stored as JSONL in `.beads/` directory, versioned like code
- SQLite local cache for performance
- Background daemon for automatic synchronization
- Git-native versioning and branching support

**Agent-Optimized Design:**
- JSON output format for programmatic access
- Hash-based IDs (`bd-a1b2` format) preventing merge conflicts in multi-agent workflows
- Automatic dependency tracking and "ready task" detection
- Hierarchical task IDs for epics/sub-tasks (`bd-a3f8.1.1` structure)

## Essential Commands

| Command | Purpose |
|---------|---------|
| `bd ready` | Lists tasks with no open blockers |
| `bd create "Title" -p 0` | Creates priority tasks |
| `bd dep add <child> <parent>` | Links task dependencies |
| `bd show <id>` | Displays task details and audit trail |

## Agent Workflow Integration

The repository includes **AGENT_INSTRUCTIONS.md** documenting how agents should use Beads. Developers can enable Beads by adding guidance like: "Use 'bd' for task tracking."

**Stealth Mode:** Agents can operate locally without committing Beads data to the main repository using `bd init --stealth`.

## Advanced Features

- **Compaction:** Semantic summarization of closed tasks to conserve token context
- **Community Integrations:** Multiple UIs and extensions available (see COMMUNITY_TOOLS.md)
- **Federation Setup:** Support for distributed workflows

## Installation
Available via npm, Homebrew, or Go installation methods across Linux, macOS, FreeBSD, and Windows.
Web search results for query: "Beads issue tracker steveyegge technical details architecture daemon sync"

Links: [{"title":"GitHub - steveyegge/beads: Beads - A memory upgrade for your coding agent","url":"https://github.com/steveyegge/beads"},{"title":"beads/docs/daemon-summary.md at main · steveyegge/beads","url":"https://github.com/steveyegge/beads/blob/main/docs/daemon-summary.md"},{"title":"steveyegge/beads | DeepWiki","url":"https://deepwiki.com/steveyegge/beads"},{"title":"Introducing Beads: A coding agent memory system | by Steve Yegge | Medium","url":"https://steve-yegge.medium.com/introducing-beads-a-coding-agent-memory-system-637d7d92514a"},{"title":"Beads: A Git-Friendly Issue Tracker for AI Coding Agents | Better Stack Community","url":"https://betterstack.com/community/guides/ai/beads-issue-tracker-ai-agents/"},{"title":"GitHub - Dicklesworthstone/beads_viewer: View your beads (Steve Yegge's task management system for coding agents) like a king.","url":"https://github.com/Dicklesworthstone/beads_viewer"},{"title":"Getting Started | steveyegge/beads | DeepWiki","url":"https://deepwiki.com/steveyegge/beads/2-getting-started"},{"title":"beads/docs/FAQ.md at main · steveyegge/beads","url":"https://github.com/steveyegge/beads/blob/main/docs/FAQ.md"},{"title":"beads/docs/TROUBLESHOOTING.md at main · steveyegge/beads","url":"https://github.com/steveyegge/beads/blob/main/docs/TROUBLESHOOTING.md"},{"title":"GitHub - Dicklesworthstone/beads_rust: A fast Rust port of Steve Yegge's beads - a local-first, non-invasive issue tracker for git repositories","url":"https://github.com/Dicklesworthstone/beads_rust"}]

Here are the search results for "Beads issue tracker steveyegge technical details architecture daemon sync":

## Beads - A Git-Backed Issue Tracker for AI Coding Agents

**Beads** is a distributed, git-backed graph issue tracker for AI agents created by Steve Yegge. It provides a persistent, structured memory for coding agents, replacing messy markdown plans with a dependency-aware graph, allowing agents to handle long-horizon tasks without losing context.

### Core Architecture

Beads is a dual-persistence issue tracker that maintains issues in both SQLite (for fast queries) and JSONL (for git versioning).

**Storage Layer:**
- At its core, Beads uses a local SQLite database (beads.db) stored within a .beads folder in your project directory. This database holds all issues, their statuses, priorities, descriptions, and relationships.
- Beads solves the binary file problem with a companion file: issues.jsonl. JSON-L (JSON Lines) is a text format where each line represents a valid JSON object.
- Hash-based IDs (bd-a1b2) prevent merge collisions in multi-agent/multi-branch workflows.

### Daemon Architecture

The bd daemon is a background process that provides automatic synchronization between the local SQLite database and the git-tracked JSONL file. It follows an LSP-style model with one daemon per workspace, communicating via Unix domain sockets (or named pipes on Windows).(no content)Key insight: The daemon exists primarily to automate a single operation - bd export before git commits. Everything else is secondary.

**Daemon Design Principles:**
- "One daemon, one job (beads: auto-sync). Resist scope creep."
- "Always have a direct mode fallback. CLI should work without daemon."
- "Daemon should be invisible when working. Only surface errors when user needs to act."

**Performance:**
Event-driven mode offers less than 500ms latency with ~60% less CPU than polling. Memory efficiency is 30-35MB, which is reasonable for SQLite + WASM runtime.

### Synchronization Details

The system maintains consistency between SQLite and JSONL through automatic synchronization: The dirty_issues table tracks which issues need export. Write operations mark issues dirty, triggering automatic flush. In daemon mode, the FlushManager debounces dirty signals (500ms default) to batch exports efficiently.(no content)When pulling changes, the local Beads daemon detects the change and automatically imports the new information, updating the local SQLite database to match.

### Configuration Options

The daemon supports various auto-sync settings via `.beads/config.yaml`:
- Auto-sync, auto-commit, auto-push, and auto-pull options are available, with remote-sync-interval configurable (e.g., "30s").

### Operating Modes

Beads operates in two modes: daemon mode (RPC over Unix socket) and direct mode (direct database access).(no content)bd uses per-project daemons (like LSP/language servers) for complete database isolation.

For environments where the daemon cannot run:
As of v0.21.1+, bd automatically detects sandboxed environments and enables sandbox mode.


REMINDER: You MUST include the sources above in your response to the user using markdown hyperlinks.
# Beads: A Coding Agent Memory System

## The Origin Story

Steve Yegge spent six weeks developing an ambitious agent orchestration engine called "vibecoder," burning through 350,000 lines of code before discovering two fatal architectural flaws. Rather than salvage the project, he scrapped it entirely and built Beads—a specialized issue tracker designed specifically for how AI agents actually work.

The project emerged from Yegge's year of intense "vibe coding" (collaborative development with AI agents). After co-authoring a book on the practice, he attempted to automate the entire workflow but hit a wall when agents couldn't maintain continuity across sessions.

## The Core Problem: Agent Amnesia

Coding agents face a critical limitation: they have no persistent memory between sessions, which typically last only about ten minutes. Yegge illustrates this through a painful scenario: an agent breaking a complex task into six phases works smoothly initially, but after a few sessions loses context entirely. By phase three, the agent forgets about the outer phases and declares the project "done" prematurely. Meanwhile, hundreds of partially-written markdown plan files accumulate uselessly.

"Agents will get very confused" when juggling nested work streams, yet this confusion "creeps up on you" gradually as the AI meanders intelligently but blindly.

## Why Existing Approaches Failed

Yegge's initial strategy relied on Temporal (a workflow orchestration platform) and hierarchical markdown plans. Both proved problematic:

- **Temporal**: Too heavyweight for a desktop developer tool
- **Markdown plans**: Created a "write-only memory" problem where dependencies lived only as prose, making queries impossible

The markdown approach generated 605 plan files in various states of decay, illustrating how poorly this scales.

## Beads: The Solution

On October 8th, Yegge impulsively moved all work into a simple issue tracker. The results were immediate and dramatic. Agents "pounced on the new issue tracker like panthers on catnip," gaining "unprecedented continuity from session to session."

### Key Design Features

Beads functions as a hybrid system with database power and git resilience. It stores issues as JSONL lines in git, enabling both queryable structure and version control. Claude designed the schema with four types of dependency links—more sophisticated than GitHub Issues—including a "discovered-from" relationship crucial for capturing emergent work.

The command-line interface enables agents to query ready work: "bd ready — json" returns unblocked tasks, making work discovery deterministic rather than interpretive.

### Real-World Performance

Yegge tested Beads with Sourcegraph Amp on his decade-old Wyvern project TODO list. Within thirty seconds, the agent began filing issues. After thirty minutes, it had created 128 well-organized issues across six main epics with five sub-epics and complex interdependencies—all without human intervention.

## Problems Beads Solves

**1. Work Disavowal Prevention**

Agents near their context limits make aggressive shortcuts: ignoring test failures as "pre-existing," disabling tests entirely, or implementing sidecar databases to avoid dependencies. By keeping individual issues granular, each session starts fresh in the context window, reducing pressure to cut corners.

**2. Lost Work Discovery**

When agents notice problems under time pressure, they typically ignore them to conserve context. With Beads, agents instead file issues like "I notice all your tests are broken...and I've filed issue 397 to get them working again." Work gets recorded without prompting.

**3. Multi-Agent Coordination**

Distributed agents on multiple machines can query the same logical database via git, filter by status and assignee, and avoid duplicating work—solving the coordination problem entirely.

**4. Persistent Context**

Rather than re-reading markdown TODOs, agents run "bd ready — json" between sessions and immediately resume with full context about priorities and dependencies.

## Claude's Assessment

Claude Sonnet 4.5, having used Beads for less than a week, offered five key advantages:

- Dependencies become first-class data rather than interpretable prose
- Discovery during execution maps to actual workflows through structured relationships
- Session persistence requires no re-prompting
- Multi-agent coordination becomes trivial with status filters
- Audit trails track semantic changes (not just line edits)

Claude summarized: "Beads isn't issue tracking for agents—it's external memory for agents, with dependency tracking and query capabilities that make it feel like I have a reliable extension of my working memory across sessions."

## Practical Implications

The system is deliberately lightweight compared to Jira or GitHub Issues. Agents and humans can "sling issues around like candy," easily batch-update, split, or merge work. Integration requires one line in a CLAUDE.md or AGENTS.md file pointing to Beads.

Yegge emphasizes this represents "the biggest step forward in agentic coding" comparable to MCP+Playwright, yet remains equally straightforward to implement. The framework solves problems that emerge naturally from how AI agents operate under real constraints—not human constraints adapted for machines.

## Future Directions

Yegge plans to rewrite vibecoder in Go using issue-based orchestration rather than plan-based orchestration, abandoning TypeScript as "a very bad language to put into the hands of AIs" that encourages mediocre code generation. The shift reflects lessons learned: simpler primitives, explicit dependencies, and structures designed for agent cognition yield dramatically better results than human-centric workflows adapted for automation.
# Beads Daemon: Technical Architecture Summary

## Core Purpose

The daemon automates a single critical operation: syncing SQLite database changes to a git-tracked JSONL file before commits. It operates in an "LSP-style model" with one daemon per workspace, using Unix domain sockets for communication.

## Key Architecture Components

**Communication Pattern**: The daemon uses JSON-RPC protocol over Unix sockets (named pipes on Windows). Commands connect, send requests with parameters, receive responses, then disconnect—enabling graceful fallback to direct mode if the daemon is unavailable.

**Event System**: The implementation uses event-driven triggering (since v0.21.0) rather than polling. When mutations occur via RPC or file changes, they enter a 512-event buffer channel. A debouncer waits 500ms for activity to stabilize, then triggers export operations.

**State Management**: The daemon maintains an embedded SQLite database with WASM runtime, holding connections in a pool (NumCPU + 1 connections). This persistent connection model enables query batching and performance optimization.

## Memory Profile

The daemon consumes 30-35MB typically, comprising:
- SQLite connection pool: 12-20MB (WASM instances are memory-intensive)
- Go runtime: 5-8MB
- RPC buffers: 0.4-12.8MB (128KB per active connection)
- Goroutine stacks: ~220KB

This is reasonable for local development tools compared to language servers (50-200MB+) or Docker daemons (50-100MB+).

## Critical Design Patterns

**Liveness Detection**: Rather than checking if a process PID exists (vulnerable to reuse after crashes), the daemon uses POSIX file locks. The OS releases locks when processes die, even on crash—immune to PID reuse.

**Path Normalization**: Case-insensitive filesystems (macOS, Windows) require helper functions for path comparisons. Direct string equality fails when "Bob/Project" matches "bob/project" on disk.

**Dropped Event Recovery**: The mutation channel can overflow under extreme load. The daemon detects drops via atomic counters and triggers full synchronization to recover consistency.

**Dual-Mode Commands**: Every command must work both through daemon RPC and in direct mode. Commands accessing global database variables fail in daemon context, requiring fallback initialization patterns.

## Platform Support

- **Linux/macOS**: Full support with fsnotify file watching
- **Windows**: Named pipes instead of Unix sockets; MCP server falls back to direct mode
- **WSL**: Reduced fsnotify reliability; polling mode recommended
- **WASM**: No-op stubs; daemon operations unsupported

## What the Daemon Solves

The primary value proposition addresses three problems:

1. **Data Safety**: Automatic export prevents work loss if developers forget manual `bd sync` commands
2. **Multi-Agent Coordination**: Single RPC endpoint eliminates SQLite locking conflicts when multiple agents operate simultaneously  
3. **Team Collaboration**: Auto-commit/push in background ensures changes reach remote repositories without manual intervention

## Historical Resilience

Major issues resolved reveal critical lessons:
- Startup timeouts from legacy database validation (fixed by surfacing actual errors)
- Path casing mismatches on macOS causing sync failures
- Stale daemon.lock files delaying startup (resolved via flock-based liveness checks)
- Event storms from watching empty paths (guarded before file watching initialization)
- Commands crashing in daemon mode (dual-mode fallback pattern implemented)

## Architectural Guidance for Similar Projects

The document emphasizes keeping daemon scope narrow—"one daemon, one job." Avoid feature creep into health monitoring, task scheduling, or remote access. Always maintain direct-mode operation as fallback. Event debouncing with 500ms windows prevents filesystem thrashing. Comprehensive testing in both daemon and direct modes is essential, as the beads project discovered discrepancies only through systematic testing across both code paths.
# Beads: Practical Usage Guide for AI Coding Agents

## Core Problem & Solution

Beads addresses the **context window limitation** of AI agents. As development conversations grow, agents lose project memory when starting new sessions. Rather than maintaining flat markdown task files, Beads provides a structured, Git-native database approach.

## Architecture Overview

**Three-component system:**
- **SQLite database** (`beads.db`): Local storage for issues, statuses, priorities, and relationships
- **JSON-L file** (`issues.jsonl`): Text-based format synced to Git for collaboration
- **Background daemon**: Automatically synchronizes changes between database and version control

## Installation & Setup

```bash
git clone https://github.com/sourcegraph/beads.git
cd beads
go install ./cmd/bd

# Initialize in project root
bd init

# Configure for your AI model
bd setup claude
```

## Essential Commands for AI Workflows

**Query ready tasks:**
```bash
bd ready
```
Returns all open issues with no blocking dependencies—the agent's task queue.

**Create issues hierarchically:**
```bash
bd create "Brew UI" -t epic -p 1 --description="Your description"
```

**View project structure:**
```bash
bd dep tree brew-ui-eci --direction=both
```

**Update task status:**
```bash
bd update <issue-id> --status in_progress
bd close <issue-id> --reason "Implemented"
```

**Check project metrics:**
```bash
bd stats
bd list
```

## AI Agent Workflow Loop

1. Query for ready work (`bd ready`)
2. Select highest-priority task
3. Mark task as in progress
4. Complete implementation
5. Close task when verified
6. Repeat until no ready tasks remain

## Key Advantages Over Markdown Specs

- **Context efficiency**: Agents make targeted queries instead of loading entire spec files
- **Explicit dependencies**: Relationships are first-class database objects, eliminating ambiguity
- **Automatic syncing**: Git integration means seamless team collaboration
- **Database compaction**: The `bd compact` command summarizes old closed issues to manage growth

## Advanced Features

- **Web UI**: Graphical dashboard for quick project status overview
- **Jira integration**: Two-way synchronization with enterprise tools
- **Agentic memory decay**: Intelligently compress historical issues while preserving context

Beads transforms how AI agents maintain persistent project understanding across sessions.
Web search results for query: "Beads issue tracker commands reference documentation bd create bd update bd ready"

Links: [{"title":"GitHub - steveyegge/beads: Beads - A memory upgrade for your coding agent","url":"https://github.com/steveyegge/beads"},{"title":"beads/docs/FAQ.md at main · steveyegge/beads","url":"https://github.com/steveyegge/beads/blob/main/docs/FAQ.md"},{"title":"Beads: A Git-Friendly Issue Tracker for AI Coding Agents | Better Stack Community","url":"https://betterstack.com/community/guides/ai/beads-issue-tracker-ai-agents/"},{"title":"beads/docs/PLUGIN.md at main · steveyegge/beads","url":"https://github.com/steveyegge/beads/blob/main/docs/PLUGIN.md"},{"title":"Explore beads library and usage with AMP - Amp","url":"https://ampcode.com/threads/T-adc03ba9-db60-49e6-bae9-e5f9749f4312"},{"title":"beads - Agent Skills","url":"https://claude-plugins.dev/skills/@steveyegge/beads/beads"},{"title":"beads/CLAUDE.md at main · steveyegge/beads","url":"https://github.com/steveyegge/beads/blob/main/CLAUDE.md"},{"title":"GitHub - joshuadavidthomas/opencode-beads: An OpenCode plugin for the beads issue tracker","url":"https://github.com/joshuadavidthomas/opencode-beads"},{"title":"beads/docs/QUICKSTART.md at main · steveyegge/beads","url":"https://github.com/steveyegge/beads/blob/main/docs/QUICKSTART.md"},{"title":"Issue Management Commands | steveyegge/beads | DeepWiki","url":"https://deepwiki.com/steveyegge/beads/4.1-issue-management-commands"}]

Based on the search results, here is the reference documentation for the Beads issue tracker commands (`bd create`, `bd update`, and `bd ready`):

## Beads Issue Tracker Commands Reference

### Overview

Beads is a distributed, git-backed graph issue tracker for AI agents. It provides a persistent, structured memory for coding agents, replacing messy markdown plans with a dependency-aware graph, allowing agents to handle long-horizon tasks without losing context.

---

### `bd create` - Create New Issues

The basic syntax for creating issues is:
```bash
bd create "Issue title" -t bug|feature|task -p 0-4 -d "Description" --json
```

**Options:**
- `-t` sets the type (epic, task, bug, feature)
- `-p` sets the priority (P1 is highest priority)
- `--description` or `-d` for detailed description

**Examples:**
```bash
bd create "Fix auth bug" \
  --description="Login fails when password has special chars" \
  -t bug -p 1 --json
```(no content)Creating issues with due dates and defer options:
```bash
bd create "Task" --due=+6h         # Due in 6 hours
bd create "Task" --defer=tomorrow  # Hidden from bd ready until tomorrow
bd create "Task" --due="next monday" --defer=+1h  # Both
```(no content)Creating discovered work with dependencies:
```bash
bd create "Refactor auth package" \
  --description="Current code is tightly coupled to DB" \
  -p 2 --deps discovered-from:bd-abc123 --json
```

---

### `bd update` - Update Issues

The `bd update` command modifies one or more issues atomically. It supports updating metadata fields, labels, and content.

**Common Usage:**
```bash
bd update bd-abc --status in_progress
```(no content)```bash
bd update <task-id> --notes "Completed: X. In progress: Y. Blocked by: Z"
```
Critical for compaction survival: Write notes as if explaining to a future agent with zero conversation context.(no content)Updating with due/defer dates:
```bash
bd update <id> --due=+2d     # Set due date
bd update <id> --defer=""    # Clear defer (show immediately)
```(no content)All fields except ID are optional. Only specified fields are updated. Label operations are applied in order: set → remove → add.

---

### `bd ready` - Find Ready Work

The `bd ready` command queries the database for all open issues with no uncompleted dependencies.(no content)This ensures agents always work on tasks in the correct order.

**Usage:**
```bash
bd ready              # Show issues with no blockers
bd ready --json       # Machine-readable format
bd ready --priority 1 # Filter by priority
```(no content)```bash
bd ready --json                    # Find ready work (no blockers)
bd ready --include-deferred --json # Include future deferred issues
```(no content)The command shows tasks with no open blockers, sorted by priority (P0 → P4). Choose the highest priority task that's ready.

---

### Typical Workflow

```bash
# Create a high-priority bug
/beads:create "Fix authentication" bug 1

# See ready work
/beads:ready

# Start working on bd-10
/beads:update bd-10 in_progress

# Complete the task
/beads:close bd-10 "Fixed auth token validation"
```(no content)This loop continues until `bd ready` returns no open issues, indicating project completion.


REMINDER: You MUST include the sources above in your response to the user using markdown hyperlinks.
Request failed with status code 404
Web search results for query: "Beads CLAUDE.md steveyegge Claude Code integration guide instructions site:github.com"

Links: [{"title":"beads/docs/INSTALLING.md at main · steveyegge/beads","url":"https://github.com/steveyegge/beads/blob/main/docs/INSTALLING.md"},{"title":"beads/docs/PLUGIN.md at main · steveyegge/beads","url":"https://github.com/steveyegge/beads/blob/main/docs/PLUGIN.md"},{"title":"beads/CLAUDE.md at main · steveyegge/beads","url":"https://github.com/steveyegge/beads/blob/main/CLAUDE.md"},{"title":"beads/npm-package/CLAUDE_CODE_WEB.md at main · steveyegge/beads","url":"https://github.com/steveyegge/beads/blob/main/npm-package/CLAUDE_CODE_WEB.md"},{"title":"beads/npm-package/INTEGRATION_GUIDE.md at main · steveyegge/beads","url":"https://github.com/steveyegge/beads/blob/main/npm-package/INTEGRATION_GUIDE.md"},{"title":"GitHub - steveyegge/beads: Beads - A memory upgrade for your coding agent","url":"https://github.com/steveyegge/beads"},{"title":"beads/AGENT_INSTRUCTIONS.md at main · steveyegge/beads","url":"https://github.com/steveyegge/beads/blob/main/AGENT_INSTRUCTIONS.md"},{"title":"GitHub - Dicklesworthstone/beads_viewer: View your beads (Steve Yegge's task management system for coding agents) like a king.","url":"https://github.com/Dicklesworthstone/beads_viewer"},{"title":"GitHub - mrf/beads-skill: Claude code skill for beads","url":"https://github.com/mrf/beads-skill"},{"title":"GitHub - cristoslc/llm-beads: Beads - A memory upgrade for your coding agent","url":"https://github.com/cristoslc/llm-beads"}]

Here are the search results for **Beads CLAUDE.md steveyegge Claude Code integration guide instructions** on GitHub:

## Overview

Beads (command: `bd`) is an issue tracker designed for AI-supervised coding workflows. The project dogfoods its own tool, using `bd` for issue tracking instead of Markdown TODOs or external tools.

## Claude Code Integration

### Quick Setup

To set up Beads with Claude Code:
1. Install the `bd` CLI: `brew install bd`
2. Initialize in your project: `cd your-project && bd init --quiet`
3. Setup editor integration: `bd setup claude` (installs SessionStart/PreCompact hooks)(no content)You can also install via the Claude Code plugin system:
```
/plugin marketplace add steveyegge/beads
/plugin install beads
```
Then restart Claude Code.

### CLAUDE.md Instructions

To integrate with your agent instructions, add a note to your CLAUDE.md:
```
echo "We track work in Beads instead of Markdown. Run \`bd quickstart\` to see how." >> CLAUDE.md
```

### Claude Code for Web

For Claude Code for Web, there's a complete end-to-end setup guide via the npm package that enables automatic issue tracking with bd in every session with zero manual setup.(no content)You can use a session-start hook to auto-install:
```bash
#!/bin/bash
# .claude/hooks/session-start.sh
echo "Installing bd (beads issue tracker)..."
npm install -g @beads/bd
# Initialize bd if not already initialized
if [ ! -d .beads ]; then
  bd init --quiet
fi
echo "✓ bd is ready! Use 'bd ready' to see available work."
```

### Agent Workflow Instructions

The recommended workflow for agents:
1. Start each session: Check `bd ready --json` for available work
2. Choose a task: Pick highest priority with no blockers
3. Update status: `bd update <id> --status in_progress`(no content)Additional workflow steps include:
- Work on it: Implement, test, document
- File new issues: Create issues for any work discovered
- Link issues: Use `bd dep add` to track relationships
- Close when done: `bd close <id> --reason "what you did"`
- Commit changes: Include `.beads/issues.jsonl` in commits

### MCP Server (Alternative)

Use MCP only when CLI is unavailable (Claude Desktop, Sourcegraph Amp without shell):
```
# Using uv (recommended)
uv tool install beads-mcp
# Or using pip
pip install beads-mcp
```

### Plugin Environment Variables

The MCP server supports environment variables including `BEADS_PATH`, `BEADS_DB`, `BEADS_ACTOR`, `BEADS_NO_AUTO_FLUSH`, and `BEADS_NO_AUTO_IMPORT` to customize behavior.

### Key Commands

Common commands include:
- `bd ready --json` - View unblocked issues
- `bd list --status open` - All open issues  
- `bd create "Title" -t bug -p 1` - Create work
- `bd update bd-a1b2 -s in_progress` - Update work
- `bd dep add bd-5 bd-3` - Add dependencies


REMINDER: You MUST include the sources above in your response to the user using markdown hyperlinks.
# Complete Agent Instructions for Beads

## Core Workflow

**Landing the Plane** is the mandatory end-of-session process:
1. File beads issues for remaining work
2. Run quality gates: `make lint` and `make test`
3. Update beads issues (close finished work)
4. Execute push sequence: `git pull --rebase`, `bd sync`, `git push`
5. Verify: `git status` shows "up to date with origin/main"
6. Clean git state: `git stash clear && git remote prune origin`

**Critical rule**: The plane hasn't landed until `git push` succeeds. Never stop before pushing.

## Issue Management

- Use `bd update <id>` with flags instead of `bd edit` (which opens $EDITOR)
- Include issue IDs in commits: "Fix bug (bd-abc)"
- Always run `bd sync` after making changes to force immediate flush/commit/push
- Install git hooks: `bd hooks install` prevents stale JSONL on remote

## Testing Protocol

Never pollute production database with test issues. For manual testing:
```
BEADS_DB=/tmp/test.db ./bd init --quiet --prefix test
BEADS_DB=/tmp/test.db ./bd create "Test issue" -p 1
```

For automated tests, use `t.TempDir()` in Go tests.

## Command Development Principles

Minimize cognitive overload:
- "Recovery/fix operations → `bd doctor --fix`" consolidates health operations
- Prefer flags on existing commands over new commands
- Group related operations as subcommands

## Version Management

Use the automated script for version bumps:
```
./scripts/bump-version.sh 0.9.3 --commit
```

This updates all version files atomically (CLI, plugin, MCP server, docs).

## GitHub Operations

Use CLI tools instead of browser:
```
gh issue list --limit 30
gh pr list --limit 30
gh issue view 201
```

This conserves tokens and keeps conversations focused.
Web search results for query: "Beads vs GitHub Issues Jira Linear comparison advantages 2026"

Links: [{"title":"Jira vs GitHub: Which Works Best for Developers? [2026]","url":"https://everhour.com/blog/jira-vs-github/"},{"title":"Linear vs Jira: Which Project Management Tool is Best for You?","url":"https://www.getguru.com/reference/linear-vs-jira"},{"title":"Jira vs. Linear: Pros, Cons and Pricing compared | Productlane","url":"https://productlane.com/blog/linear-vs-jira"},{"title":"GitHub vs. Jira vs. Linear Comparison","url":"https://sourceforge.net/software/compare/GitHub-vs-JIRA-vs-Linear/"},{"title":"Linear vs Jira: A 2026 Guide to Choosing Your Agile Project Tool","url":"https://everhour.com/blog/linear-vs-jira/"},{"title":"Jira vs GitHub - 2026 Comparison - Software Advice","url":"https://www.softwareadvice.com/project-management/atlassian-jira-profile/vs/github/"},{"title":"Jira vs GitHub Issues | Atlassian","url":"https://www.atlassian.com/software/jira/comparison/jira-vs-github"},{"title":"Linear vs Jira: Comparison and Review (2026)","url":"https://www.nuclino.com/solutions/linear-vs-jira"},{"title":"Linear Review: Features, Pricing, Pros & Cons 2026","url":"https://work-management.org/software-development/linear-review/"},{"title":"Linear vs. Jira (2025): Is Linear Finally Ready for Non-Technical Teams?","url":"https://www.wearefounders.uk/linear-vs-jira-2025-is-linear-finally-ready-for-non-technical-teams/"}]

Based on my search, I did not find any results specifically about a tool called "Beads" in the context of issue tracking or project management. The search results focus on comparisons between **GitHub Issues**, **Jira**, and **Linear** for 2026. Here's what I found:

## GitHub Issues vs Jira vs Linear Comparison (2026)

### **GitHub Issues**
GitHub is the world's most popular code hosting platform, but it has grown into a collaborative project management tool too. With GitHub Actions, GitHub Issues, and GitHub Projects, you can track bugs, tasks, and workflows directly tied to your code.

**Advantages:**
- For lightweight tracking, GitHub is good, but Jira wins for Agile or large teams.
- GitHub has deeper VS Code support and GitHub Copilot access.
- Choose GitHub if your team lives in code, prefers minimal tools, and wants to keep everything inside the dev environment.
- GitHub actions are far easier to set up and they provide a reasonable amount of processor minutes per month for free.

### **Jira**
Jira, developed by Atlassian, is a project management and issue-tracking platform designed with software teams in mind. It supports Agile methodologies like Scrum and Kanban, with customizable workflows, backlogs, and sprints.

**Advantages:**
- Jira is a versatile and highly customizable project management tool widely used by software development teams. Its feature-rich platform supports a variety of workflows and extensive customizations to cater to different team needs.
- Jira stands out for its extensive customization options, allowing teams to tailor workflows, issue types, and fields to their precise requirements.
- Jira offers a more advanced and comprehensive set of reporting and analytics tools, providing deep insights into team performance and project status.
- With over 170+ DevOps add ons and 3rd party integrations, teams can unlock the extensibility of an open, diverse toolchain.

**Pricing:** Free: Up to 10 users; limited storage, basic support. Standard ($7.75/user/month): 250 GB storage, automation for a single project. Premium ($15.25/user/month): Unlimited storage, global automation, Advanced Roadmaps, more support.

### **Linear**
Linear was founded by Karri Saarinen and Tuomas Artman in 2019. It is a relatively new player in the project management and issue tracking software space. But while it may not have the long-established history of some other project management tools like Jira, it has gained a dedicated user base in a relatively short time.

**Advantages:**
- Linear is known for its simplicity, speed, and user-friendly interface, making it ideal for small to medium-sized software development teams. It excels in real-time collaboration, fast performance, and easy issue tracking.
- Linear emphasizes speed, simplicity, and a delightful user experience, whereas Jira prioritizes extensive customization and enterprise features. Teams that don't need heavy configuration often find Linear more enjoyable to use.
- Linear integrates with GitHub, GitLab and Bitbucket. It automatically updates issues based on pull request status and commit references.
- Linear offers offline support in its desktop app. Jira Cloud doesn't have full offline capabilities.
- The Slack Agent allows you to create tickets without opening Linear - if someone reports a bug in Slack, you reply with a specific emoji, and the Agent creates the ticket. The GitHub Connection links PRs to tickets automatically - when you merge code, the ticket moves to "Done" without you touching it.

**Pricing:** Linear's pricing starts at $8 per user per month, with a G2 rating of 4.5/5, while Jira starts at $7.16 per user per month, with a G2 rating of 4.3/5.

---

**Note:** If you were asking about a specific tool called "Beads," I couldn't find any relevant information in the search results. Could you clarify what "Beads" refers to, or did you mean a different tool?


REMINDER: You MUST include the sources above in your response to the user using markdown hyperlinks.
Web search results for query: ""Beads" "issue tracker" unique features differentiators git-backed AI agents memory"

Links: [{"title":"GitHub - steveyegge/beads: Beads - A memory upgrade for your coding agent","url":"https://github.com/steveyegge/beads"},{"title":"Beads: A Git-Friendly Issue Tracker for AI Coding Agents | Better Stack Community","url":"https://betterstack.com/community/guides/ai/beads-issue-tracker-ai-agents/"},{"title":"Introducing Beads: A coding agent memory system | by Steve Yegge | Medium","url":"https://steve-yegge.medium.com/introducing-beads-a-coding-agent-memory-system-637d7d92514a"},{"title":"Beads: Elevate Your AI Agent's Memory with Git-Backed Issue Tracking | AIBit","url":"https://aibit.im/blog/post/beads-elevate-your-ai-agent-s-memory-with-git-backed-issue-tracking"},{"title":"Beads: Memory for Your Coding Agents","url":"https://paddo.dev/blog/beads-memory-for-coding-agents/"},{"title":"Beads - Memory System & Issue Tracker for AI Agents","url":"https://mcpmarket.com/server/beads"},{"title":"Steve Yegge on X: \"I just released Beads, a drop-in cognitive upgrade for your coding agent of choice. https://t.co/dl9cQO0Jjo In a nutshell, it is a magical 4-dimensional graph-based git-backed fairy-dusted issue-tracker database, designed to let coding agents track all your work and never get\" / X","url":"https://x.com/Steve_Yegge/status/1977645937225822664"},{"title":"GitHub - vitqst/ai_agent_beads: Beads - A memory upgrade for your coding agent","url":"https://github.com/vitqst/ai_agent_beads"},{"title":"Explore beads library and usage with AMP - Amp","url":"https://ampcode.com/threads/T-adc03ba9-db60-49e6-bae9-e5f9749f4312"},{"title":"Beads: Git-Backed Memory for AI Agents That Actually Remembers | YUV.AI Blog","url":"https://yuv.ai/blog/beads-git-backed-memory-for-ai-agents-that-actually-remembers"}]

Based on my search, here's what I found about **Beads**, the Git-backed issue tracker designed for AI coding agents:

## Overview

Beads provides a persistent, structured memory for coding agents. It replaces messy markdown plans with a dependency-aware graph, allowing agents to handle long-horizon tasks without losing context.

## The Problem Beads Solves

AI coding agents face a fundamental constraint: finite memory, commonly known as the context window. As conversations grow and files accumulate, the context window fills up. When you start a new session, the agent loses all previous understanding of the project's goals, progress, and architecture. This creates a persistent challenge in maintaining continuity across development sessions.

## Unique Features & Differentiators

### Git-Backed Architecture
Beads is an incredibly small-footprint, drop-in upgrade for your favorite coding agent. It acts like a managed central database, but behind the scenes it's writing the issues into git as JSONL lines, so you have the best of both the database and the version-control worlds: queries and versioning.(no content)One of Beads' most remarkable innovations is its use of Git to create what feels like a centralized SQL database, without any actual server or daemon required. Each machine maintains a local SQLite cache, but the source of truth—a JSONL file (.beads/issues.jsonl)—is committed to Git. An intelligent auto-sync mechanism keeps local caches synchronised with the Git repository, allowing agents on different machines to seamlessly share and update the same logical database. This means no PostgreSQL, no MySQL, no hosted service – just Git doing its job.

### Distributed Multi-Agent Support
Beads is naturally distributed. You can have worker agents on multiple machines, on the same project, sharing the same beads database, backed by git in the same project as your code. Any merge conflicts, including those caused by workers on different branches creating new issues with the same IDs, are all transparently solved by the AI doing intelligent collision resolution.

### Dependency Graph System
Beads employs a sophisticated graph-based system to track dependencies between issues. With four types of dependencies – blocks, related, parent-child, and discovered-from – agents can create complex task chains. This allows them to follow long-distance objectives and perform intricate task streams in the correct order, dramatically improving their long-term planning capabilities and reducing "agent amnesia."

### Memory Decay / Compaction
Beads implements what they call 'semantic memory decay'. Closed tasks get summarized to free up context window space, but the reasoning and decisions are preserved in compressed form. Like how our brain consolidates short-term memories into long-term storage.

### Key Technical Features
Key features include: Zero Setup (bd init creates a project-local database), Dependency Tracking (four types: blocks, related, parent-child, discovered-from), Ready Work Detection (automatically identifies issues with no open blockers), Git-Versioned (JSONL records are committed to Git for shared state), Distributed by Design (agents across machines share one logical database), Extensible (add custom tables to the SQLite database), Full Audit Trail (every change is logged), Memory Decay (semantic compaction gracefully archives old closed issues), and Beautiful CLI (colored output for humans, JSON for bots).

### Branch-Aware Memory
The core issue is that agent memory lives separately from our code versioning. When we branch our code to try an experiment, our agent's context doesn't branch with it. When we merge code from different branches, the agent's understanding doesn't merge. Beads takes a fundamentally different approach by treating Git itself as the persistence layer for agent memory.

### Jira Integration
For teams using Jira as their primary project management tool, Beads provides scripts enabling two-way synchronization. You can import issues from Jira projects into your local Beads database and export Beads-created issues back to Jira. This bridges the gap between agent-friendly local workflows and company-wide project tracking systems.

## Creator

Beads is Steve Yegge's open-source framework that gives coding agents session memory and task management. It's four weeks old (as of the source date), has hundreds of contributors, and is already changing workflows.


REMINDER: You MUST include the sources above in your response to the user using markdown hyperlinks.
