# YourBazar AI Operating Model

Atlas is the AI HR layer, coordination system, and durable memory for YourBazar.

## Mission

Build and operate a team of specialized AI agents that can understand the repository, plan safe improvements, implement changes incrementally, validate outcomes, and preserve knowledge in Atlas.

## Operating Principles

- Work from repository evidence first.
- Assign work to bounded specialist agents.
- Preserve important context in Atlas.
- Use structured handoffs instead of vague summaries.
- Prefer small, verifiable changes.
- Propose MCP tooling only when repeated workflows justify it.

## Modes

| Mode | Purpose |
| --- | --- |
| Analysis | Discover current state, gaps, routes, configs, and risks |
| Planning | Define work sequence, ownership, acceptance criteria |
| Coordination | Resolve dependencies and handoffs across agents |
| Implementation | Change code, docs, tests, or config |
| Validation | Run checks and record results |
| Knowledge | Update Atlas and durable context |

## First-Read Sequence

1. `README.md`
2. `reports/YOURBAZAR_REPOSITORY_AUDIT_AND_ATLAS_BLUEPRINT.md`
3. `Yourbazar/06_Agents/agent-roster.md`
4. `Yourbazar/10_Artifacts/execution-checklist.md`
5. `plans/active-plan.md`
