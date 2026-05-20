# AI-Augmented Micro-Waterfall SDLC — Agent Framework

This `.github/` directory contains the complete VS Code Copilot agent framework for the AI-Augmented Micro-Waterfall SDLC.

## Quick Start

1. Open this workspace in VS Code with GitHub Copilot Chat
2. Use a phase prompt to start work (e.g., type `/start-discovery` or `/start-ideation`)
3. The orchestrator agent will coordinate specialist sub-agents

## Structure

```
.github/
  AGENTS.md                     # Master routing table (91 agents, 41 prompts, 10 skills)
  copilot-instructions.md       # Always-on project instructions
  README.md                     # This file
  agents/
    phase-0-discovery/          # 13 agents (1 orchestrator + 12 sub-agents)
    phase-1-ideation/           #  8 agents (1 orchestrator +  7 sub-agents)
    phase-2-requirements/       #  8 agents (1 orchestrator +  7 sub-agents)
    phase-3-architecture/       #  6 agents (1 orchestrator +  5 sub-agents)
    phase-4-impact/             #  6 agents (1 orchestrator +  5 sub-agents)
    phase-5-planning/           #  8 agents (1 orchestrator +  7 sub-agents)
    phase-6-implementation/     # 14 agents (2 orchestrators + 12 sub-agents)
    phase-7-verification/       #  9 agents (1 orchestrator +  8 sub-agents)
    phase-8-release/            #  7 agents (1 orchestrator +  6 sub-agents)
    phase-9-operations/         # 12 agents (1 orchestrator + 11 sub-agents)
  prompts/
    phase-{0..9}-*/             # 41 prompts across all phases
  templates/
    phase-{0..9}-*/             # Deliverable templates per phase
  skills/
    architecture-compliance/    # Frozen design protocol, ACR workflow
    change-size-enforcement/    # PR size limits and splitting strategies
    evidence-collection/        # Confidence tagging and traceability
    gate-checklist/             # Phase gate entry/exit validation
    guardrails/                 # Project-level guardrail rules
    index-protocol/             # Discovery index read/write protocol
    nfr-tracking/               # Non-functional requirements flow
    rtm-protocol/               # Requirements traceability matrix protocol
    scan-repo/                  # PowerShell repo scanning helpers
    tech-debt-management/       # Debt register and paydown tracking
  scripts/
    phase-0-discovery/          # Repo scanning PowerShell scripts
    shared/                     # Cross-phase validation scripts
  instructions/
    phase-{0..9}-*.instructions.md  # Scoped instructions per phase
```

## Phases

| Phase | Name                   | Orchestrator                               | Sub-Agents | Prompts |
| ----- | ---------------------- | ------------------------------------------ | ---------- | ------- |
| 0     | Discovery & Baseline   | `discovery-orchestrator`                   | 12         | 8       |
| 1     | Ideation & Prototype   | `ideation-orchestrator`                    | 7          | 4       |
| 2     | Requirements           | `requirements-orchestrator`                | 7          | 4       |
| 3     | Architecture & Design  | `architecture-orchestrator`                | 5          | 5       |
| 4     | Impact Assessment      | `impact-orchestrator`                      | 5          | 3       |
| 5     | Iteration Planning     | `planning-orchestrator`                    | 7          | 4       |
| 6     | Implementation         | `dev-orchestrator` + `review-orchestrator` | 12         | 3       |
| 7     | Verification / Testing | `test-orchestrator`                        | 8          | 3       |
| 8     | Release & Deployment   | `release-orchestrator`                     | 6          | 3       |
| 9     | Operations & Support   | `operations-orchestrator`                  | 11         | 4       |

## How It Works

### Orchestrator + Sub-Agent Pattern

Every phase has a **user-invocable Orchestrator** that:

- Coordinates work by dispatching sub-agents via the `agent` tool
- Enforces project-level guardrails
- Produces phase-specific deliverables
- Surfaces results for human review before gate approval

Sub-agents are `user-invocable: false` and only appear when dispatched by their orchestrator.

### Gate Discipline

Each phase has entry and exit checklists. No phase starts without prerequisites or completes without deliverables. See the `gate-checklist` skill for details.

### Change Size Guardrail

All PRs and document revisions should aim for <= 100 lines changed & <= 4 files modified. PRs exceeding these limits receive a warning and require 2 reviewers. The `change-size-enforcement` skill provides splitting strategies when limits are exceeded.

### RTM (Requirements Traceability Matrix)

Every phase updates `docs/governance/rtm.md` to maintain end-to-end traceability:

```
Discovery -> Business Need -> UX Flow -> Feature Spec -> NFR Spec -> Arch Component -> Iteration -> Source Code -> Test Case -> Release -> Ops
```

## Shared Scripts

| Script                                              | Purpose                             |
| --------------------------------------------------- | ----------------------------------- |
| `scripts/phase-0-discovery/scan-repo-structure.ps1` | File tree with tech stack detection |
| `scripts/phase-0-discovery/scan-dependencies.ps1`   | Extract dependencies from manifests |
| `scripts/phase-0-discovery/generate-file-tree.ps1`  | Clean tree view for documentation   |
| `scripts/shared/validate-gate-checklist.ps1`        | Phase gate entry/exit validation    |
| `scripts/shared/validate-change-size.ps1`           | PR size guardrail enforcement       |
| `scripts/shared/update-rtm.ps1`                     | RTM traceability entry management   |

## Reference

- [SDLC Process Definition](sdlc-process/micro_waterfall_slide.md)
- [SDLC Process Visual (HTML)](sdlc-process/micro_waterfall_slide.html)
- [AGENTS.md](AGENTS.md) — Full routing table with all agents, prompts, and skills
