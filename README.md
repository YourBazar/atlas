# Atlas

Atlas is the AI context, architecture, planning, and handoff repository for YourBazar.

It is not a runtime service. It exists so Codex, other AI agents, and human maintainers can resume work without rediscovering repository context.

## Purpose

Atlas records:

- repository and submodule architecture;
- current implementation state;
- current and upcoming product features;
- route, model, API, CI, deployment, and sync context;
- known issues, risks, and security findings;
- active plans, backlog, and decision records;
- reusable prompts and templates for future work.

## Main Report

Read the full repository audit and implementation blueprint first:

- `reports/YOURBAZAR_REPOSITORY_AUDIT_AND_ATLAS_BLUEPRINT.md`

For AI-agent coordination, start here:

- `Yourbazar/00_Overview/ai-operating-model.md`
- `Yourbazar/06_Agents/agent-roster.md`
- `Yourbazar/10_Artifacts/communication-protocol.md`
- `Yourbazar/10_Artifacts/execution-checklist.md`

## Directory Map

| Path | Purpose |
| --- | --- |
| `context/` | Repository memory, current state, deployment, sync, and known issues |
| `product/` | Product vision, roles, feature inventory, and future feature direction |
| `engineering/` | Submodule-specific engineering notes and technical strategy |
| `plans/` | Active work, backlog, roadmaps, and remediation plans |
| `decisions/` | Architecture decision records |
| `prompts/` | Prompts for future AI-agent work |
| `reports/` | Large shareable audits and blueprints |
| `templates/` | Reusable planning and handoff templates |
| `Yourbazar/` | AI HR, multi-agent operating model, agent contexts, MCP policy, and coordination artifacts |

## Usage Rule

When meaningful work changes repository behavior, architecture, deployment, security posture, or roadmap, update Atlas in the same development cycle.
