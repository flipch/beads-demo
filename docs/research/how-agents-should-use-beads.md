# How Agents Should Use Beads

> Research findings from [DeepWiki: steveyegge/beads](https://deepwiki.com/steveyegge/beads/9-ai-agent-integration)

## Overview

**Beads** is a distributed, git-backed issue tracker designed specifically as a memory upgrade for AI coding agents. It solves the "agent amnesia" problem by providing persistent, structured memory that survives context window compactions and session changes.

Rather than relying on ephemeral markdown task lists, Beads uses a dependency-aware graph structure that enables agents to handle long-horizon, multi-session tasks without losing context.

---

## Section 9: AI Agent Integration

### Three Integration Points

| Integration | Token Cost | Best For |
|-------------|-----------|----------|
| **Direct CLI + Hooks** | ~1-2k | Shell-enabled environments (Claude Code, Cursor, Windsurf, Aider) |
| **MCP Server** | ~10-50k | MCP-only environments (Claude Desktop) |
| **Claude Plugin** | ~1-2k | Claude Code/Desktop with automatic context injection |

### Dual-Persistence Architecture

Beads maintains issues in two synchronized layers:

- **SQLite Database** (`.beads/beads.db`): Fast local queries, sub-millisecond performance, gitignored
- **JSONL File** (`.beads/issues.jsonl`): Git-tracked source of truth, line-oriented, merge-friendly

### Key Design Principles

- **Hash-Based IDs**: Issues use content-derived identifiers (e.g., `bd-a1b2`) to prevent merge collisions
- **Dependency-Aware Graph**: Tasks linked via blocks, parent-child, related, and discovered-from relationships
- **Agent-Optimized**: JSON output, auto-ready task detection, structured for minimal token usage
- **Git-Native**: Issues are versioned, branched, and merged like code

---

## Section 9.1: MCP Server

### Architecture

The `beads-mcp` Python package implements a Model Context Protocol (MCP) server that exposes beads functionality to AI agents.

**Components:**
- **beads-mcp**: Python package implementing MCP server
- **FastMCP Framework**: Handles MCP communication protocol
- **bd CLI**: Core tool invoked by MCP server
- **Per-Project Daemons**: Auto-started, isolated database per workspace

**LSP-Style Architecture:**
- One MCP server instance serves unlimited projects
- Automatic workspace detection from working directory
- Routes to correct per-project daemon at `.beads/bd.sock`
- Complete database isolation between projects

### Installation

```bash
# Using uv (recommended)
uv tool install beads-mcp

# Or using pip
pip install beads-mcp
```

### Claude Desktop Configuration

Add to `~/Library/Application Support/Claude/claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "beads": {
      "command": "beads-mcp"
    }
  }
}
```

### Available MCP Tools

- `beads_init`: Initialize beads in a repository
- `beads_create_issue`: Create new issues
- `beads_list_issues`: List issues with filters
- `beads_ready_work`: Get unblocked tasks
- `beads_show_issue`: Display issue details
- `beads_update_issue`: Modify issue properties
- `beads_close_issue`: Complete tasks
- `beads_add_dependency`: Create dependency links
- `beads_blocked_issues`: Show blocked work
- `beads_stats`: Project statistics

### When to Use MCP

**Use MCP when:**
- MCP-exclusive environments (e.g., Claude Desktop)
- Shell access is unavailable
- Standardized tool interfaces across MCP clients

**Avoid MCP when:**
- Shell access available (use CLI instead)
- Token-constrained scenarios
- Latency-sensitive operations

---

## Section 9.2: Claude Plugin

### Architecture

The Claude Plugin provides **passive context injection** rather than interactive tools. It focuses on automatic context priming at strategic lifecycle moments.

**Hook Integration:**
1. **SessionStart Hook**: Executes `bd prime` at session beginning
2. **PreCompact Hook**: Executes `bd prime` before context compaction

### Installation

```bash
# 1. Install bd CLI
curl -sSL https://raw.githubusercontent.com/steveyegge/beads/main/scripts/install.sh | bash

# 2. Install plugin in Claude Code
/plugin marketplace add steveyegge/beads
/plugin install beads
```

### Context Injection via `bd prime`

The `bd prime` command provides curated context including:
- Current ready tasks (unblocked work)
- Recently updated issues
- Blocked work summary
- Project statistics

This context (~1-2k tokens) is automatically injected:
1. When starting a new conversation
2. After context window compaction

### Slash Commands

- `/beads:ready` - Find tasks with no blockers
- `/beads:create` - Create new issues
- `/beads:show` - Display issue details
- `/beads:update` - Modify issue status
- `/beads:close` - Complete tasks
- `/beads:list` - List issues with filters
- `/beads:dep` - Manage dependencies

---

## The Agent Work Cycle

```
1. Check for Ready Work  →  bd ready --json
2. Select Task           →  Choose highest priority (P0→P4)
3. Claim Work            →  bd update <id> --status in_progress --json
4. Track Progress        →  Add notes, update descriptions
5. Discover New Work     →  Create linked issues for bugs/tasks found
6. Complete Task         →  bd close <id> --reason "..." --json
7. Sync Changes          →  bd sync (before ending session)
```

---

## Core Commands Reference

### CRUD Operations

```bash
# Initialize beads in a repository
bd init

# Create issues (always use --json for agent consumption)
bd create "Task title" -t task -p 1 --json
bd create "Bug description" -t bug -p 0 --json
bd create "Feature name" -t feature -p 2 --json

# Query for ready work
bd ready --json                    # All ready tasks
bd ready --priority 0 --json       # Critical tasks only

# Show issue details
bd show <issue-id> --json

# List issues with filters
bd list --status open --json
bd list --status in_progress --json
bd list --type bug --json

# Update issues
bd update <id> --status in_progress --json
bd update <id> -d "Updated description" --json
bd update <id> --priority 0 --json

# Close issues (always include reason)
bd close <id> --reason "Completed: brief description" --json

# Get project statistics
bd stats --json
```

### Dependency Management

```bash
# Add dependencies (A depends on B)
bd dep add <child-id> <parent-id> --type blocks
bd dep add <task-id> <epic-id> --type parent-child
bd dep add <issue-a> <issue-b> --type related
bd dep add <new-id> <discovered-during-id> --type discovered-from

# Remove dependencies
bd dep rm <child-id> <parent-id>

# Visualize dependency tree
bd dep tree <issue-id>

# Show blocked issues
bd blocked --json
```

### Synchronization

```bash
# Sync changes (bypass debounce)
bd sync

# Install git hooks for automatic sync
bd hooks install
```

---

## Dependency Types Explained

| Type | Description | Example |
|------|-------------|---------|
| **blocks** | Task B cannot start until Task A completes | API must exist before frontend consumes it |
| **parent-child** | Hierarchical relationships (epics → tasks) | "Auth Epic" contains "Login Task" |
| **related** | Informational links without blocking | Two bugs in the same module |
| **discovered-from** | New work found during execution | Found session timeout bug while implementing login |

---

## Best Practices for Agents

### 1. Always Use JSON Output
All commands support `--json` flag for structured, parseable output. Essential for programmatic consumption.

### 2. Write Notes for Future Agents
Assume zero conversation context when writing descriptions. Critical for context survival across compactions.

### 3. Session Protocol
- **Start**: Check `bd ready --json` for available work
- **During**: Update status, create discovered issues, maintain notes
- **End**: Always run `bd sync` before ending session

### 4. Use discovered-from Dependencies
When finding bugs/tasks during implementation, create linked issues to maintain discovery context.

### 5. Priority Management
- P0 = Critical (work on first)
- P1 = High
- P2 = Medium
- P3 = Low
- P4 = Backlog

Use numbers (0-4), not words.

### 6. Issue Types
- `task`: Standard work items
- `bug`: Defects requiring fixes
- `feature`: New functionality
- `epic`: Parent container for related tasks
- `question`: Research or clarification needed
- `docs`: Documentation work

### 7. Avoid Common Pitfalls
- Never use `bd edit` (requires interactive editor) - use `bd update` instead
- Don't forget to `bd sync` before session end
- Include issue IDs in commit messages: `git commit -m "Fix bug (bd-123)"`

---

## Beads vs TodoWrite

| Aspect | TodoWrite | Beads |
|--------|-----------|-------|
| **Scope** | Short-term (this hour) | Long-term (this week/month) |
| **Persistence** | Single session | Multi-session, survives compaction |
| **Storage** | Ephemeral | Git-backed, distributed |
| **Dependencies** | None | Full dependency graph |
| **Use for** | Immediate action items | Complex workflows, discovery tracking |

**Both tools complement each other at different timescales.**

---

## Common Patterns

### Pattern: Epic Breakdown

```bash
# Create epic
EPIC=$(bd create "Auth System" -t epic -p 1 --json | jq -r '.id')

# Create subtasks
bd create "Login endpoint" -p 2 --deps parent-child:$EPIC --json
bd create "Signup endpoint" -p 2 --deps parent-child:$EPIC --json
bd create "Session management" -p 2 --deps parent-child:$EPIC --json
```

### Pattern: Discovery During Work

```bash
# Working on task
bd update bd-abc123 --status in_progress --json

# Discover a bug
BUG=$(bd create "Auth validation missing" -t bug -p 1 \
  --deps discovered-from:bd-abc123 --json | jq -r '.id')

# Bug blocks original work
bd dep add bd-abc123 $BUG --type blocks
```

### Pattern: Multi-Agent Coordination

```bash
# Agent A claims task
bd update bd-xyz789 --status in_progress --assignee agent-a --json

# Agent B checks for available work
bd ready --json | jq '.[] | select(.assignee == null)'

# Agent A completes and syncs
bd close bd-xyz789 --reason "Completed: Feature implemented" --json
bd sync
```

---

## Integration Decision Tree

```
Do you have shell access?
├─ Yes → Use CLI + Hooks (recommended)
│         Token cost: ~1-2k
│         Latency: Low
│         Setup: bd setup <editor>
│
└─ No → Use MCP Server
          Token cost: ~10-50k
          Latency: Medium
          Setup: uv tool install beads-mcp
```

---

## Quick Start Checklist

1. **Install**: `curl -sSL https://raw.githubusercontent.com/steveyegge/beads/main/scripts/install.sh | bash`
2. **Initialize**: `bd init` in your repository
3. **Setup Integration**: `bd setup claude` (or cursor/aider)
4. **Install Hooks**: `bd hooks install`
5. **Start Working**: `bd ready --json` to find work

---

## Golden Rules

1. **Always use `--json`** for all commands
2. **Always `bd sync`** before ending session
3. **Never use `bd edit`** (use `bd update` instead)
4. **Always provide `--reason`** when closing tasks
5. **Write notes for future agents** (assume zero context)
6. **Use discovered-from** to maintain context lineage
7. **Include issue IDs in commits**: `git commit -m "Fix (bd-123)"`

---

## Sources

- [AI Agent Integration | steveyegge/beads | DeepWiki](https://deepwiki.com/steveyegge/beads/9-ai-agent-integration)
- [MCP Server | steveyegge/beads | DeepWiki](https://deepwiki.com/steveyegge/beads/9.1-mcp-server)
- [Claude Plugin | steveyegge/beads | DeepWiki](https://deepwiki.com/steveyegge/beads/9.2-claude-plugin)
- [GitHub - steveyegge/beads](https://github.com/steveyegge/beads)
