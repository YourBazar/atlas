# System Boundaries

| Area | Owner Repo | Boundary |
| --- | --- | --- |
| Frontend | `services/cygnus` | React routes, pages, auth state, API calls |
| Backend | `services/centaurus` | FastAPI routes, controllers, models, persistence |
| API Specs | `services/ApiSpecs` | OpenAPI contracts and planned APIs |
| Product Docs | `services/MarkDowns` | Product requirements and flows |
| Infrastructure | `services/Dockyard` | Local Docker services |
| AI Context | `services/atlas` | Planning, handoffs, agent operating model |

Agents must not cross ownership boundaries without recording the handoff and updating Atlas.
