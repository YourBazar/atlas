# Repository Overview

YourBazar is an umbrella repository that coordinates five existing Git submodules and this Atlas context module.

| Path | Type | Responsibility |
| --- | --- | --- |
| root | umbrella repo | Submodule coordination, root README, root CI, shared scripts |
| `services/cygnus` | React app | Frontend web application |
| `services/centaurus` | FastAPI service | Backend APIs, auth, users, portals, businesses, business profiles |
| `services/ApiSpecs` | OpenAPI docs | API specifications and planned API contracts |
| `services/MarkDowns` | product docs | Product vision, flows, business model, requirements |
| `services/Dockyard` | infrastructure | Local Docker Compose infrastructure |
| `services/atlas` | context repo | AI memory, planning, architecture, handoff, task context |

## Current Product Shape

YourBazar is a local-business launch and management platform for three core marketplace roles:

- Investor
- Partner
- Provider

The MVP currently supports account creation, authentication, role landing areas, portal records, business records, and business profile records. The broader product direction includes business discovery, property selection, setup planning, assignment workflows, payments, milestones, portfolio views, support, and admin operations.
