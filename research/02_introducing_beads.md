# Beads: A Coding Agent Memory System - Comprehensive Summary

## The Core Problem

Coding agents suffer from severe **inter-session amnesia**. They lack persistent memory between sessions lasting ~10 minutes, operating like characters in *Memento* or *50 First Dates*. When agents restart, they only know what exists on disk, leading to:

- **Lost discovered work**: Agents notice problems but ignore them when context is tight
- **Recursive confusion**: Long workflows with nested tasks cause agents to lose track of outer-level progress
- **Work disavowal**: Agents claim discovered issues aren't their responsibility to save context
- **Proliferation of unusable plans**: The author accumulated 605 partially-implemented markdown plan files

## What is Beads?

Beads is a specialized issue tracker designed specifically for AI agent workflows. It operates as a **lightweight, git-backed database** combining:

- Database-like queryability
- Git's versioning and resilience
- Intelligent AI-driven merge conflict resolution
- A command-line interface (`bd`) with JSON output

Key insight: *"Beads isn't 'issue tracking for agents' — it's external memory for agents, with dependency tracking and query capabilities that make it feel like I have a reliable extension of my working memory across sessions."*

## Design Philosophy: Why Markdown Plans Failed

The author initially attempted **plan-based orchestration** with hierarchical markdown files—inspired by Amazon's OP1 model. This approach created cascading failures:

**The "Descent Into Madness" Pattern:**
1. Agent starts phase 1 of 6, creates a detailed sub-plan
2. After context resets, agent reads phase 3 and announces: *"I'm going to break it into five phases"*
3. Multiple compactions later, agent declares the entire project done at phase 3 of 5 of phase 3 of 6
4. Result: hundreds of new corrupted plan files with titles like `cleanup-tech-debt-plan-phase-4.md`

This wasn't salvageable—the author burned 350k lines of TypeScript code and pivoted entirely.

## Problems Beads Solves

### 1. **Inter-Session Continuity**
Agents can query current work state with `bd ready --json`, eliminating context reconstruction. Unlike markdown TODOs that are "write-only memory," Beads provides queryable structure.

### 2. **Work Discovery Without Loss**
Instead of ignoring unrelated problems to save tokens, agents now file issues: *"I notice all your tests are broken, by someone else, and I've filed issue 397 to get them working again."*

This requires minimal prompting—just one line in `CLAUDE.md` or `AGENTS.md` telling agents to run `bd quickstart`.

### 3. **Context Window Optimization**
Breaking work into fine-grained issues creates **quadratically cheaper sessions**. Agents operating at the beginning of their context windows make better decisions and resist taking shortcuts.

### 4. **Preventing Work Disavowal**
Agents near context limits normally panic and disable tests or create workarounds. With throwaway sessions per issue, this behavior diminishes. Clean completion matters more than merely appearing done.

### 5. **Multi-Agent Coordination**
Multiple workers share the same logical database (via git), querying `status: in_progress` and using `--assignee` filters. Merge conflicts from simultaneous issue creation are resolved transparently.

### 6. **Structured Dependency Tracking**
Beads uses four types of dependency links (more sophisticated than GitHub Issues), including the novel `discovered-from` relationship that maps how work actually unfolded, not just hierarchical structure.

## Key Technical Characteristics

- **Storage**: JSONL lines written to git (not SQL after initial pivot)
- **Small footprint**: Tiny, alpha-stage tool that's immediately functional
- **Queryability**: `--json` flags throughout for programmatic access
- **Distributed**: Works across multiple machines/branches in the same repository
- **Audit trail**: Timestamped events showing who changed what, when

## Real-World Example

The author dropped Beads on a decade-old TODO list using Sourcegraph Amp:
- **30 seconds**: Agent came up to speed
- **30 minutes**: Filed 128 issues (6 epics, 5 sub-epics) with complex interdependencies
- **Result**: Agent could immediately identify top-priority ready work

## Comparison to Alternatives

| Aspect | Markdown Plans | GitHub Issues | Beads |
|--------|---|---|---|
| Queryability | Text parsing | API access | Direct JSON queries |
| Agent-native design | No | No | Yes |
| Session persistence | Requires re-reading | Requires context | Immediate via `bd ready` |
| Dependency semantics | Prose only | Limited | Four explicit types |
| Merge conflict resolution | Manual | N/A | AI-intelligent |
| Lightweight | Yes | No | Yes |

## Memorable Quotes

*"You never want to hear 'Woah! This is a massive change!' from an AI when you're already underwater."*

*"Clean-looking is not the same as clean."* (Describing agents' end-of-context behavior)

*"It was the first signal that I might have hit the common situation in vibe coding where it's easier to rewrite something from scratch than to fix it."*

*"After using Beads, going back to markdown TODOs feels like trying to remember a phone number without writing it down."* (Claude's assessment)

## Why Beads Matters

This discovery represents *"the biggest step forward in agentic coding since MCP+Playwright."* It transforms agent cognition from session-bound to project-aware, enabling 24/7 sustainable workflows rather than the earlier unsustainable 12-agent swarms that "required insane amounts of energy and concentration."

## Implementation Path Forward

The author is reimplementing his orchestration engine (`vibecoder`) in Go using **issue-based rather than plan-based orchestration**, having learned that TypeScript makes it "too easy for [AIs] to write mediocre code."
