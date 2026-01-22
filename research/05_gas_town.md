# Gas Town Decoded: Summary

Based on the provided article, here's what the content covers:

## What Gas Town Is

Gas Town is an AI agent orchestration tool by Steve Yegge that manages workflows through coordinated autonomous agents. The article describes it as a system for organizing agent-based projects, though notably complex and expensive.

## Relationship to Beads

Beads is mentioned as "System for tracking work history across the system." The article distinguishes two Beads variants:
- **Rig Beads**: Project-specific work tracking
- **Town Beads**: Workspace-level work tracking

## Key Terminology Decoded

The article provides a translation table for Gas Town's specialized vocabulary, including:
- **Town**: Workspace (top-level folder)
- **Rig**: Individual project
- **Mayor**: Managing agent coordinating other agents
- **Polecat**: Worker agents executing tasks
- **Refinery**: Merge agent handling merge requests
- **Witness**: Fixer agent addressing stuck agents
- **Deacon/Dogs**: Maintenance agents running looped workflows

## Content Limitations

The article does **not** discuss the MEOW stack, molecules concepts, comparisons between basic vs. advanced Beads implementations, or detailed pros/cons analysis. It focuses primarily on terminology decoding rather than comprehensive system architecture.

The decoder's stated purpose: making Steve Yegge's "Welcome to Gas Town" intelligible through simplified definitions.
# Gas Town Decoded: Summary

Andrew Lilley Brinker's article breaks down Steve Yegge's January 1st introduction to Gas Town, an AI agent orchestration system that sparked significant discussion due to its scale and cost.

## Core Purpose

The author created a decoder because Yegge's original article, though imaginative, used heavily specialized terminology that made understanding the actual system difficult. As Brinker notes: "His article itself offers definitions, but those definitions reuse his insular terms, making by-hand decoding tedious."

## Key Terminology Decoded

The article provides a translation table converting Gas Town's whimsical terms to conventional software concepts:

**Infrastructure Terms:**
- **Town** = Workspace (top-level folder managing projects)
- **Rig** = Individual project with its own Git repository

**Agent Roles:**
- **Mayor** = Manager agent coordinating project work
- **Polecat** = Worker agent executing tasks and submitting requests
- **Refinery** = Merge agent handling merge requests
- **Witness** = Fixer agent monitoring and repairing stuck workers
- **Deacon** = Maintenance manager running continuous workflows

**Tracking System:**
- **Beads** = Work history tracker across the system
- **Rig Beads** = Project-specific work tracking
- **Town Beads** = Workspace-wide work tracking

## Critical Assessment

Brinker acknowledges Gas Town remains somewhat unnecessarily complex, questioning whether certain hierarchical layers (like Maintenance Manager Checker Agents) are truly essential.
Web search results for query: ""Gas Town" "MEOW stack" Steve Yegge"

Links: [{"title":"Welcome to Gas Town. Happy New Year, and Welcome to Gas… | by Steve Yegge | Jan, 2026 | Medium","url":"https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04"},{"title":"Stevey’s Birthday Blog. My birthday’s coming up, Jan 20th… | by Steve Yegge | Jan, 2026 | Medium","url":"https://steve-yegge.medium.com/steveys-birthday-blog-34f437139cb5"},{"title":"Welcome to Gas Town","url":"https://rhettg.com/post/2026-01-09-gas-town/"},{"title":"Welcome to Gas Town | daily.dev","url":"https://app.daily.dev/posts/welcome-to-gas-town-eydqaql8h"},{"title":"Then God told Steve Yegge to build an Ark: Gastown and the Industrialization of the Merge Queue | by Johnny Clem | Jan, 2026 | Medium","url":"https://medium.com/@johnnyclem/then-god-told-steve-yegge-to-build-an-ark-gastown-and-the-industrialization-of-the-merge-queue-0b7ad30e17f6"}]

Based on my search, here's what I found about "Gas Town" and the "MEOW stack" by Steve Yegge:

**Gas Town** is an orchestration system for managing 20-30 AI coding agents (like Claude Code) simultaneously.(no content)The current version is a Go "port" of Python Gas Town, only 17 days old at the time of the article.(no content)In 17 days, it produced 75k lines of code and 2000 commits.

**The MEOW Stack** stands for "Molecular Expression of Work." It consists of:

- **Beads**: Atomic units of work (git-backed issues). **Molecules**: Durable, sequenced workflows that survive crashes.(no content)Beads originated in October when Yegge told Claude in frustration to put all his work in a lightweight issue tracker. He wanted Git for it, Claude wanted SQLite, and they compromised on both, creating Beads in about 15 minutes.(no content)In Gas Town, operating on the MEOW stack, all work is expressed as molecules. Molecules are workflows that can have complex shapes, loops, and gates, and are Turing-complete. Each step of the workflow is executed by a superintelligent AI.

The system also features:
- A hierarchy of specialized agent roles including Mayor, Polecats, Refinery, Witness, and Deacon to coordinate parallel development.
- GUPP (Gas Town Universal Propulsion Principle) to keep agents working continuously across session restarts.
- A principle called "Nondeterministic Idempotence" (NDI) where the outcome must be idempotent—if an agent dies halfway through a task, the system spins up a fresh agent that looks at the persistent state and finishes the job.(no content)Yegge considers the MEOW stack, Nondeterministic Idempotence, and Beads as a Universal Data Plane to be critically important differentiators for Gas Town that will yield unexpected emergent benefits over time because of the audit trail.


REMINDER: You MUST include the sources above in your response to the user using markdown hyperlinks.
Web search results for query: "Steve Yegge "Welcome to Gas Town" original article"

Links: [{"title":"Welcome to Gas Town. Happy New Year, and Welcome to Gas… | by Steve Yegge | Jan, 2026 | Medium","url":"https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04"},{"title":"Welcome to Gas Town | Hacker News","url":"https://news.ycombinator.com/item?id=46458936"},{"title":"Welcome to Gas Town - Weaving News | Weaving News","url":"https://www.weaving.news/news/019b903c-94c2-764a-9751-89f9185d5611"},{"title":"All Gas Town, No Brakes Town","url":"https://www.todayintabs.com/p/all-gas-town-no-brakes-town"},{"title":"Steve Yegge on X: \"Happy New Year! I've just launched my coding agent orchestrator, Gas Town, for anyone crazy enough to try it. https://t.co/xWJLZzmpZH\" / X","url":"https://x.com/Steve_Yegge/status/2006835043503845445"},{"title":"Welcome to Gas Town | Steve Yegge | 42 comments","url":"https://www.linkedin.com/posts/steveyegge_welcome-to-gas-town-activity-7412601550629437440-hpeJ"},{"title":"Welcome to Gas Town | MyHub.AI","url":"https://myhub.ai/items/welcome-to-gas-town"},{"title":"Welcome to Gas Town [Article] <> No-Code Supply Co.","url":"https://www.nocodesupply.co/item/welcome-to-gas-town"},{"title":"Welcome to Gas Town | Lobsters","url":"https://lobste.rs/s/vd8q8g"},{"title":"Gas Town Decoded — Andrew Lilley Brinker","url":"https://www.alilleybrinker.com/mini/gas-town-decoded/"}]

Here are the search results for Steve Yegge's "Welcome to Gas Town" article:

**Original Article:**
The original article is published on Medium at steve-yegge.medium.com. It opens with "Happy New Year, and Welcome to Gas Town!" and introduces Gas Town as "a new take on the IDE for 2026" that "helps you with the tedium of running lots of Claude Code instances."

**What is Gas Town?**
Gas Town is described as "a complex agent orchestrator and IDE concept aimed at coordinating 20–30 concurrent AI coding agents (e.g., Claude Code and similar CLIs)."(no content)Yegge compares Gas Town to Kubernetes and Temporal, noting that it's complicated because he "had to keep adding components until it was a self-sustaining machine," resulting in something that "looks a lot like Kubernetes mated with Temporal and they had a very ugly baby together."

**Development History:**
Yegge started building his own orchestrator in August since "nobody else seemed to care." The first version failed, leading to v2 (which produced Beads), then v3 (Python Gas Town), and finally the current Go-based Gas Town as his "fourth complete, functioning orchestrator of 2025."

**Naming Theme:**
"Gas Town" itself is a reference to the oil refinery citadel in Mad Max. The Mad Max theme continues in the names of various components like the Mayor (the AI agent users communicate with), Rigs (projects under development), and workflow concepts like the Refinery and the Convoy.

**Launch:**
Steve Yegge announced the launch on X (Twitter) on January 1, 2026, saying "Happy New Year! I've just launched my coding agent orchestrator, Gas Town, for anyone crazy enough to try it."

The original article can be found at: https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04


REMINDER: You MUST include the sources above in your response to the user using markdown hyperlinks.
Web search results for query: "Steve Yegge Gas Town MEOW stack molecules Beads 2025"

Links: [{"title":"Welcome to Gas Town. Happy New Year, and Welcome to Gas… | by Steve Yegge | Jan, 2026 | Medium","url":"https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04"},{"title":"GitHub - steveyegge/gastown: Gas Town - multi-agent workspace manager","url":"https://github.com/steveyegge/gastown"},{"title":"Welcome to Gas Town","url":"https://scribe.rip/welcome-to-gas-town-4f25ee16dd04"},{"title":"Then God told Steve Yegge to build an Ark: Gastown and the Industrialization of the Merge Queue | by Johnny Clem | Jan, 2026 | Medium","url":"https://medium.com/@johnnyclem/then-god-told-steve-yegge-to-build-an-ark-gastown-and-the-industrialization-of-the-merge-queue-0b7ad30e17f6"},{"title":"The Future of Coding Agents. It has been three days since I launched… | by Steve Yegge | Jan, 2026 | Medium","url":"https://steve-yegge.medium.com/the-future-of-coding-agents-e9451a84207c"},{"title":"Gas Town Emergency User Manual. It’s been a busy 12 days since I… | by Steve Yegge | Jan, 2026 | Medium","url":"https://steve-yegge.medium.com/gas-town-emergency-user-manual-cf0e4556d74b"},{"title":"The Brownian Ratchet and the Chimpanzee Factory - DEV Community","url":"https://dev.to/aronchick/the-brownian-ratchet-and-the-chimpanzee-factory-583n"},{"title":"Welcome to Gas Town | MyHub.AI","url":"https://myhub.ai/items/welcome-to-gas-town"},{"title":"Welcome to Gas Town","url":"https://rhettg.com/post/2026-01-09-gas-town/"},{"title":"Steve Yegge Releases Gas Town, Multi-Agent Orchestrator For Claude Code | ASCII News","url":"https://ascii.co.uk/news/article/news-20260102-190a5f9f/steve-yegge-releases-gas-town-multi-agent-orchestrator-for-c"}]

Based on my search, here's what I found about Steve Yegge's Gas Town, the MEOW stack, Molecules, and Beads:

## Overview

Steve Yegge released GasTown, a multi-agent orchestrator he describes as "an industrialized coding factory manned by superintelligent chimpanzees."(no content)Gas Town is a Go-based orchestrator enabling developers to manage 20-30 parallel AI coding agents productively using tmux, built on the Beads framework.

## The MEOW Stack

Gas Town decouples the work from the worker using the Molecular Expression of Work (MEOW) stack. The components include:

- **Beads**: Atomic units of work (git-backed issues). **Molecules**: Durable, sequenced workflows that survive crashes.(no content)Soon after Beads there were Epics: Beads with children, which could in turn be epics themselves. This gave you a lot of flexibility to build top-down plans. The children of epics are parallel by default, but you can put in explicit dependencies between them in order to force them to be sequenced.(no content)In Gas Town, operating on the MEOW stack, all work is expressed as molecules. There is a bit of an algebra to it. Molecules are workflows. They can have complex shapes, and loops, and gates, and are in fact Turing-complete.

## Origin of Beads

In October, Yegge told Claude in frustration to put all his work in a lightweight issue tracker. He wanted Git for it. Claude wanted SQLite. They compromised on both, and Beads was born, in about 15 minutes of mad design. These are the basic work units.

## Development Timeline

In August he started building his own orchestrator. Eventually it failed, and he threw it out and started over on v2, which also failed, but Beads came out of it. Then v3 (Python Gas Town), which lasted a good six or eight weeks. Gas Town (in Go) is his fourth complete, functioning orchestrator of 2025.(no content)The past 2 weeks has seen the invention and implementation of the entire MEOW stack, wisps, patrols, convoys, agents and identities as beads, swarms as beads, roles as beads, the "feed as the signal" innovations, and the addition of the Refinery, the Deacon, and the Dogs.

## Agent Roles

GasTown takes the comprehensive approach, with seven distinct agent roles that divide responsibilities across the system. The Mayor coordinates overall work, Polecats handle ephemeral tasks, the Refinery manages merge queues, and so on through Witness, Deacon, Dogs, and Crew.

## Community Response

It's been a busy 12 days since Yegge launched Gas Town. He's merged over 100 PRs from nearly 50 contributors, adding 44k lines of code. This has grown Gas Town's total size to 189k lines of Go code since its first commit on Dec 15th, which is now 2684 commits ago.


REMINDER: You MUST include the sources above in your response to the user using markdown hyperlinks.
# Gas Town: An AI Agent Orchestration System

## Overview

Gas Town is a sophisticated orchestration platform designed to manage multiple Claude Code instances simultaneously. Built by Steve Yegge in just 17 days, it represents a new approach to AI-assisted development that prioritizes throughput and automation over traditional safety-first paradigms.

## Core Architecture

### The MEOW Stack (Molecular Expression of Work)

The system's foundation comprises four evolutionary layers:

**Beads**: The atomic work unit. These are Git-backed JSON issues that persist indefinitely, storing task metadata, assignments, and history. Created in October 2025 when Yegge sought a lightweight, version-controlled issue tracker combining Git persistence with database functionality.

**Epics**: Hierarchical task containers that organize Beads into parent-child relationships. Children execute in parallel by default but can have explicit sequential dependencies, enabling both traditional top-down and inverted planning structures.

**Molecules**: Chained workflows composed of Beads that agents execute step-by-step. Unlike Epics, molecules support arbitrary shapes and runtime composition, surviving across agent crashes through persistent Git storage.

**Formulas**: Template definitions in TOML format that specify complete workflows with loops and conditional gates. These are "cooked" into protomolecule templates, then instantiated into executable wisps or molecules.

### Worker Roles (Seven Agent Personas)

Gas Town assigns Claude Code instances specific roles:

- **Mayor**: Primary interface handling user requests and work delegation
- **Polecats**: Ephemeral workers spawned on-demand to produce merge requests in swarms
- **Refinery**: Intelligent merge queue manager handling conflict resolution and rebasing
- **Witness**: Supervisor monitoring polecat health and providing tactical support
- **Deacon**: Daemon running continuous patrol workflows with exponential backoff
- **Dogs**: Deacon assistants handling maintenance and plugin execution
- **Crew**: Long-lived workers chosen by users for sustained design/implementation tasks

### Infrastructure Components

**Towns & Rigs**: A Town represents your orchestration headquarters; Rigs are individual Git repositories managed within it. Town-level workers (Mayor, Deacon, Dogs) oversee all Rigs, while per-Rig workers (Witness, Polecats, Refinery, Crew) focus on specific projects.

**Hooks**: Special pinned Beads attached to each agent. Work is "slung" onto hooks via `gt sling` commands, triggering the agent to execute queued molecules without waiting for human input.

**Convoys**: High-level work tickets wrapping tracked issues into delivery units. Unlike Epics (whose children have another parent), Convoys group existing work across multiple projects into meaningful shipping units.

## Core Principles

### GUPP (Gastown Universal Propulsion Principle)

"If there is work on your hook, YOU MUST RUN IT." This mandates autonomous agent execution: when a molecule is queued, the agent must begin immediately upon startup. This principle solves context-window exhaustion by making agents persistent identities in Git rather than ephemeral sessions.

### GUPP Nudge Workaround

Claude Code's politeness creates friction—agents often wait for user input before checking their hooks. Gas Town compensates with automated nudges via `gt nudge` every 30-60 seconds, sending tmux notifications that trigger hook execution without blocking the agent.

### Nondeterministic Idempotence (NDI)

Unlike Temporal's deterministic replay, Gas Town guarantees workflow completion through different machinery: all workflow state lives in Git-backed Beads. Even if an agent crashes mid-molecule, its successor role resumes exactly where it left off. The execution path is unpredictable, but the outcome eventually reaches completion.

### Wisps: Ephemeral Orchestration

Ephemeral Beads used by patrol agents that never persist to Git—only burned (deleted) at run's end or squashed into single-line digests. These prevent orchestration noise from polluting project history while maintaining transactional guarantees.

## Critical Requirements & Warnings

Yegge explicitly discourages casual adoption:

- **Prerequisite**: "Do not use Gas Town if you do not juggle at least five Claude Codes at once, daily"
- **Cost**: Requires multiple Claude Code accounts; effectively unlimited token burn
- **Maturity**: Three weeks old; Yegge admits, "100% vibe coded" with zero code review
- **Complexity**: Demands learning tmux and embracing "vibe coding"—chaotic, high-throughput work where some bugs get fixed multiple times and some work gets lost

## Advantages

- Enables 12-30 parallel agents handling enormous backlogs in single sessions
- Automatic workflow resumption across agent crashes via Git persistence
- Hierarchical patrol system keeps infrastructure running with minimal intervention
- Supports complex orchestration (rule-of-five reviews, 20-step release processes)
- Self-healing through agents' understanding of Beads bureaucracy

## Disadvantages

- Extremely expensive with sustained multi-agent swarms
- Steep learning curve requiring tmux mastery and paradigm shift
- Still experimental; major bugs expected
- No federation support yet (coming soon)
- UI limited to tmux; web/Emacs interfaces promised but not delivered
- Orchestration noise can pollute Git history without Wisp discipline

## Comparison to Kubernetes

Both coordinate unreliable workers toward goals using control planes (Mayor/Deacon vs. kube-scheduler), local agents (Witness vs. kubelet), and persistent state stores (Beads vs. etcd). However, Kubernetes prioritizes uptime ("Is it running?") while Gas Town optimizes for completion ("Is it done?"). Gas Town workers earn credentials; Kubernetes pods are anonymous cattle.

## Practical Workflow

The fundamental loop centers on `gt handoff`: workers accept optional self-directed work, then cleanly restart their tmux sessions. GUPP ensures they resume on the hook. For cross-rig operations, agents use `gt worktree` to grab temporary project clones. Convoys consolidate work tracking across multiple swarms.

## Vision & Future Trajectory

Yegge positions MEOW as the enduring core while Gas Town itself may have 12-month lifespan. The system scales across three dimensions: model cognition improvements, agent-friendly design evolution, and corpus inclusion in frontier model training. A "Mol Mall" marketplace for reusable formula workflows is planned. Federation for distributed towns and remote hyperscaler workers is incoming.

Human engagement remains essential—Gas Town runs "equal parts guzzoline and elbow grease," requiring active orchestration to keep workers unstuck and prevent deadlock in the merge queue.
