# Beads Best Practices: Comprehensive Guide

## Core Philosophy

Beads is positioned as an **execution tool** rather than a planning system. As Yegge notes, "Beads is an execution tool. It's like putting your coding agent on waxed skis." The system maintains intentional scope limitations—no built-in UI or planning capabilities—allowing it to serve as focused shared memory for agent workflows.

## Key Workflows & Patterns

### Planning and Import Strategy
- Use external planning tools (like OpenSpec) to create initial plans
- Have agents file detailed Beads epics and issues tracking work dependencies, designs, and parallelization opportunities
- Request agent review cycles (up to 5 iterations) on both the plan and resulting Beads structures before implementation begins
- This separation prevents Beads from becoming bloated with planning artifacts

### Agent Session Management
Agents should follow a single-task-per-session model:
- Complete one task, then terminate the process
- Start fresh agents for new work
- Beads serves as persistent working memory between sessions
- This approach reduces costs and improves model performance

### File Issuance Guidelines
- Create Beads issues for work exceeding approximately 2 minutes
- Request agents file issues during code reviews for actionable results
- Models often spontaneously file Beads issues; gentle encouragement increases frequency

## Maintenance & System Health

### Regular Diagnostics
Run "bd doctor" daily to diagnose and auto-fix issues, handle migrations, and update metadata including git hooks and configurations.

### Database Optimization
- Execute "bd cleanup" every few days
- Target: maintain 200-500 active issues maximum
- Yegge personally sets cleanup to 2 days given his high throughput
- Deleted issues remain accessible in git history—this is safe cleanup
- Keeps "issues.jsonl" under 25k token limits for agent jQuery searches

### Upgrade Discipline
- Upgrade at least bi-weekly using "bd upgrade"
- Run "bd doctor [--fix]" periodically
- Beads complexity sometimes creates merge conflicts; expect agent cleanup assistance

## Integration Patterns

### Beads + MCP Agent Mail = "Agent Village"
This emerging pattern combines:
- **Beads** providing shared memory for agents
- **Agent Mail** enabling agent-to-agent messaging
- Result: agents autonomously coordinate without extensive setup, self-elect leaders, distribute work

Yegge is testing a git worktree-based village with Emacs frontend (powered by Efrit) managing 30+ agent workers across vterm buffers.

### Orchestration Boundaries
Orchestration "doesn't belong in Beads" despite requests. Beads remains intentionally passive, expecting agents to use it as a tool rather than actively managing workflows.

## Practical Recommendations

### Issue Prefix Strategy
Short prefixes enhance readability. Yegge uses: `bd-`, `vc-`, `wy-`, `ef-`, `gt-`. Beads supports prefix changes via agent commands (e.g., `my-long-project-` → `mlp-`).

### Community Engagement
- File bug reports and feature requests on GitHub Issues
- Post questions in Discussions
- Share Beads usage on social media to encourage adoption

### Stability Trajectory
Beads has reached "nearly 6 weeks" maturity at 130k lines of Go code (roughly 50% tests). Still experiencing edge cases requiring agent cleanup, but quality improves weekly.

## What Newcomers Should Know

- Beads is **100% vibe coded** with tens of thousands using it daily
- People repurpose it for personal TODO lists beyond original scope
- No human team experience documented yet—author seeks team-based best practices
- The system embraces "crummy architecture" that "requires AI" to function—AI "hydrates" design limitations into workability
- Models independently recognize Beads' utility and often suggest its use

## Ecosystem Support

Gene Kim built a Java Swing Beads UI; at least four to five community-created interfaces exist as passion projects. This modular ecosystem approach keeps core Beads lightweight.
