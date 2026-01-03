# Gas Town Terminology Guide

This document maps Gas Town's domain-specific terminology to conventional software engineering concepts. Use this as a quick reference when onboarding or when the Mad Max theming gets in the way of understanding.

## Core Components

| Gas Town     | Conventional Term          | Description                                                                         |
| ------------ | -------------------------- | ----------------------------------------------------------------------------------- |
| **Town**     | Workspace / Environment    | The root directory containing all projects and coordination infrastructure          |
| **Mayor**    | Orchestrator / Supervisor  | The primary AI coordinator with full workspace context                              |
| **Rig**      | Project                    | A project container with its own repo, workers, and configuration                   |
| **Deacon**   | Daemon / Process Manager   | Background service managing agent processes and system health                       |
| **Witness**  | Watchdog / Process Monitor | Per-project agent that monitors worker lifecycle and handles crashes                |
| **Refinery** | Merge Queue / CI Pipeline  | Per-project processor that handles code review and merges to main                   |
| **Hook**     | Task Queue / Inbox         | Message queue that agents check on wakeup for assigned work                         |
| **Beads**    | State Store / Work Ledger  | Git-backed persistence layer tracking all work items and state                      |
| **Convoy**   | Job Batch / Work Group     | A collection of related work items tracked together                                 |
| **Swarm**    | Ephemeral Worker Set       | (Deprecated) The polecats currently assigned to a convoy's issues; no persistent ID |

## Worker Types

| Gas Town    | Conventional Term | Description                                                        |
| ----------- | ----------------- | ------------------------------------------------------------------ |
| **Polecat** | Ephemeral Worker  | Transient task runner managed by Witness, works on branches        |
| **Crew**    | Persistent Worker | Long-lived worker with direct human management, pushes to main     |
| **Dog**     | System Helper     | Short-lived Deacon helper for infrastructure tasks (not user work) |

## Workflow Lifecycle

Gas Town uses a state-based model for workflows, with terminology inspired by states of matter:

| Gas Town          | State Name | Conventional Term | Description                                        |
| ----------------- | ---------- | ----------------- | -------------------------------------------------- |
| **Formula**       | Ice-9      | Workflow Template | Source TOML defining a reusable workflow           |
| **Protomolecule** | Solid      | Compiled Template | Frozen, validated template ready for instantiation |
| **Molecule**      | Liquid     | Running Workflow  | Active workflow instance with persistent state     |
| **Wisp**          | Vapor      | One-off Task      | Ephemeral, transient work without persistence      |

## Workflow Operators

| Gas Town   | Conventional Term   | What It Does                                         |
| ---------- | ------------------- | ---------------------------------------------------- |
| **cook**   | compile / build     | Validates and freezes a Formula into a Protomolecule |
| **pour**   | instantiate / start | Creates a running Molecule from a Protomolecule      |
| **wisp**   | run-once            | Executes transient work without persistence          |
| **squash** | merge / consolidate | Combines multiple work items or branches             |
| **burn**   | terminate / cleanup | Destroys a workflow and cleans up resources          |

## Key Principles Translated

### The Propulsion Principle

> "If your hook has work, RUN IT."

In conventional terms: **Pull-based task execution with autonomous agents.** Workers poll their inbox and immediately execute any assigned work without waiting for confirmation. This is similar to worker processes in a distributed task queue (like Celery or Sidekiq), but with AI agents as the workers.

### Identity and Attribution

All work is attributed to the actor who performed it, tracked in Beads. This is equivalent to:

- Audit logging with actor identity
- Git commit authorship
- Work item ownership in project management tools

### Cross-Rig Work

When an agent needs to work on another project, Gas Town uses git worktrees to maintain identity while accessing different codebases. This is similar to:

- Multi-repo development with consistent identity
- Service mesh patterns where identity follows the request

## Mental Model Summary

If you're familiar with distributed systems and job orchestration, think of Gas Town as:

```
Town        = Kubernetes namespace / Docker Compose project
Mayor       = Orchestrator / Scheduler (like Airflow scheduler)
Rig         = Service / Application
Deacon      = systemd / supervisord
Witness     = Health check + restart policy
Refinery    = GitHub Actions merge queue
Polecat     = K8s Job / Celery task
Crew        = K8s Deployment (long-running)
Beads       = Persistent state store (like etcd)
Convoy      = Job group / Batch
Hook        = Message queue consumer
```

The key difference: the "jobs" are AI coding agents with natural language interfaces, not traditional compute workloads.

## See Also

- [Understanding Gas Town](understanding-gas-town.md) - Architectural overview
- [Propulsion Principle](propulsion-principle.md) - Core execution model
- [Molecules](molecules.md) - Workflow system details
- [Convoy](convoy.md) - Work tracking system
