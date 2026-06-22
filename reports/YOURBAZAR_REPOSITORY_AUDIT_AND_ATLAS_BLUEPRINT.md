# YourBazar Repository Audit and Atlas Blueprint

## A. Executive Summary

YourBazar is an early-stage local-business launch and management platform. Its current implementation provides the foundation for user signup, signin, role portals, businesses, and business profiles. Its product documentation describes a much broader marketplace: Investors discover and fund local business opportunities, Partners contribute skills and operations, Providers supply property or services, and Admins coordinate verification, setup, payments, support, and platform governance.

This repository is organized as an umbrella Git repository with submodules under `services/`. The current submodules are:

| Submodule | Responsibility |
| --- | --- |
| `cygnus` | React frontend |
| `centaurus` | FastAPI backend |
| `ApiSpecs` | OpenAPI and API planning documents |
| `MarkDowns` | Product, business, flow, and requirements documents |
| `Dockyard` | Docker Compose local infrastructure |
| `atlas` | AI context, planning, architecture, and handoff memory |

Atlas was created because the repository has accumulated important context across source code, docs, CI, sync scripts, security remediation, deployment conventions, and future product planning. It is not a runtime service. It is the durable knowledge base for Codex, future AI agents, and maintainers.

## B. Repository Map

### Root Repository

Observed files:

| Path | Purpose |
| --- | --- |
| `README.md` | Umbrella overview, submodule map, setup notes |
| `.gitmodules` | Submodule declarations |
| `.github/workflows/security-audit.yml` | Root security audit across submodules |
| `build.sh` | Root build orchestration for service build scripts |
| `docker_cleanup.sh` | Broad Docker cleanup helper |
| `services/` | Submodule directory |

The root repo does not own runtime application code directly. It coordinates submodule snapshots and shared operational scripts.

### `services/cygnus`

Cygnus is the React frontend.

Important observed paths:

| Path | Purpose |
| --- | --- |
| `src/App.js` | Wraps app routes with `AuthProvider` |
| `src/pages/app/Route.js` | Root React Router route declaration |
| `src/pages/auth/Signup.js` | Signup page and backend call |
| `src/pages/auth/Signin.js` | Signin page and backend call |
| `src/pages/investor/Route.js` | Investor role route group |
| `src/pages/partner/Route.js` | Partner role route group |
| `src/pages/provider/Route.js` | Provider role route group |
| `src/configs/Urls.js` | Backend endpoint URL map |
| `src/helpers/ApiCaller.js` | Axios wrapper |
| `src/helpers/AuthContext.js` | Local auth state context |
| `package.json` | React dependencies and scripts |
| `.github/workflows/` | Build, deploy, and security workflows |
| `sync.sh` | Main-to-deploy sync flow for personal deployment |

### `services/centaurus`

Centaurus is the FastAPI backend.

Important observed paths:

| Path | Purpose |
| --- | --- |
| `app/app.py` | FastAPI app, CORS, OpenAPI YAML, middleware, server manager |
| `app/urls/urls.py` | Central route registration |
| `app/views/*_handler.py` | Request handlers |
| `app/controllers/*_controller.py` | Business logic |
| `app/serializers/*_serializers.py` | Pydantic schemas |
| `app/models/models.py` | SQLAlchemy models |
| `app/connectors/sql_connector.py` | Async SQL connector |
| `app/decorators/` | Auth and payload validators |
| `tests/integrations/` | Pytest integration coverage |
| `requirements.txt` | Python dependencies |
| `.github/workflows/` | Test, lint, Docker, and security workflows |
| `sync.sh` | Main-to-deploy sync flow for personal deployment |

### `services/ApiSpecs`

ApiSpecs contains API contracts and planned API documents.

| Path | Purpose |
| --- | --- |
| `centaurus.yaml` | Stale/currently incomplete Centaurus OpenAPI document |
| `BusinessInvestor.yaml` | Planned investor business option/setup contract |
| `sample.yaml` | Reference OpenAPI sample |
| `README.md` | API spec status and drift notes |

### `services/MarkDowns`

MarkDowns contains product and requirement documentation.

| Path | Purpose |
| --- | --- |
| `FLOW.md` | Current and planned product flow |
| `Backend_requirments.md` | Backend capability requirements |
| `Frontend_requirements.md` | Frontend roadmap and technology wishlist |
| `RULE_BASED.md` | Product and operational rules |
| `Revenue_Expenses.md` | Commercial model |
| `BUSINESS.md` | Business model notes |
| `Everything.md` | Broad product vision |
| `About.md` | Product description |

### `services/Dockyard`

Dockyard provides local infrastructure.

| Path | Purpose |
| --- | --- |
| `docker-compose.yaml` | Docker service definitions |
| `.env` | Development service toggles and default values |
| `build_setup.sh` | Start enabled infrastructure |
| `pause_setup.sh` | Pause services |
| `start_setup.sh` | Start existing services |
| `rebuild_setup.sh` | Rebuild services |
| `delete_setup.sh` | Delete Dockyard-managed services |
| `conf_generator.sh` | Generates service configs |

### `services/atlas`

Atlas is the new AI context and planning repository.

| Path | Purpose |
| --- | --- |
| `context/` | Current state, architecture, known issues, security, deployment, sync context |
| `product/` | Product vision, roles, features, future direction |
| `engineering/` | Technical notes by submodule |
| `plans/` | Active plan, backlog, roadmaps, remediation plans |
| `decisions/` | Architecture decision records |
| `prompts/` | Prompts for future AI-agent work |
| `templates/` | Task, feature, bug, and release templates |
| `reports/` | This comprehensive audit and blueprint |

## C. Architecture Analysis

### Observed Runtime Architecture

YourBazar currently has two runtime applications:

1. Cygnus frontend.
2. Centaurus backend.

Dockyard provides optional local infrastructure. ApiSpecs and MarkDowns are non-runtime documentation repositories. Atlas is non-runtime context memory.

### Request Flow

Observed signup/signin flow:

1. User submits Cygnus signup or signin form.
2. Cygnus computes `x-payload-auth` as a checksum over the request data.
3. Cygnus sends a JSON request to Centaurus.
4. Centaurus payload decorator recomputes and validates checksum.
5. Auth controller creates or authenticates user.
6. Centaurus returns response containing signed data.
7. Cygnus sets signed-in state and stores token/profile fields locally.

Observed protected backend flow:

1. Caller sends `x-user-id`.
2. Caller sends `x-authorization`.
3. Auth decorator extracts Bearer token.
4. Token payload is validated against the requested user ID.
5. Controller performs requested database operation.

### Backend Service Boundaries

Centaurus layer responsibilities:

| Layer | Responsibility |
| --- | --- |
| `app.py` | Application setup, middleware, OpenAPI YAML endpoint, server management |
| `urls` | Route registration |
| `views` | HTTP request/response coordination |
| `decorators` | Payload and auth validation |
| `controllers` | Business logic |
| `connectors` | Database access |
| `models` | Persistence schema |
| `serializers` | Request and response contracts |
| `tests` | Integration behavior validation |

### Frontend Service Boundaries

Cygnus responsibilities:

| Layer | Responsibility |
| --- | --- |
| `App.js` | Auth context wrapper |
| `pages/app/Route.js` | Top-level browser routes |
| `pages/auth` | Signup, signin, signout |
| `pages/investor` | Investor route and content |
| `pages/partner` | Partner route and content |
| `pages/provider` | Provider route and content |
| `helpers` | Auth context, API caller, local storage helpers |
| `configs/Urls.js` | Backend endpoint map |

### Persistence Layer

Centaurus uses SQLAlchemy async models inheriting from `CentaurusModel`, which adds:

- internal numeric `id`;
- external `identifier`;
- `is_active`;
- timestamps;
- JSON `meta`.

Implemented route coverage exists for users, portals, businesses, and business profiles. Models also exist for investments, expenses, earnings, transactions, business investors, business partners, and business providers.

### Observability

Centaurus includes OpenTelemetry setup and tracing decorators. Logging is present across request middleware, controllers, decorators, and connector operations.

Security caveat: observed logging patterns include sensitive-risk areas such as full URL logging, request payload debug logging, validation response body echoing, and environment dumps.

## D. Feature Inventory

### Existing Features

| Feature | Implementation | Notes |
| --- | --- | --- |
| Signup | Centaurus auth route/controller, Cygnus Signup page | Implemented |
| Signin | Centaurus auth route/controller, Cygnus Signin page | Implemented |
| Auth tokens | Centaurus JWT helper | Implemented |
| Password hashing | Centaurus utility helper | Implemented |
| Payload checksum | Cygnus helper, Centaurus decorator | Implemented |
| Role portals | Centaurus portal CRUD | Implemented |
| Business CRUD | Centaurus business CRUD | Implemented |
| Business profile CRUD | Centaurus business profile CRUD | Implemented |
| Role route areas | Cygnus investor/partner/provider routes | Implemented as content pages |
| Local infrastructure | Dockyard Docker Compose | Implemented |
| Security audit CI | Root and submodules | Implemented |

### Partially Implemented Features

| Feature | Evidence | State |
| --- | --- | --- |
| Investments | `Investments` model | No observed route/controller |
| Expenses | `Expenses` model | No observed route/controller |
| Earnings | `Earnings` model | No observed route/controller |
| Transactions | `Transactions` model | No observed route/controller |
| Investor business participants | `BusinessInvestors` model | No observed route/controller |
| Partner assignments | `BusinessPartners` model | No observed route/controller |
| Provider assignments | `ProviderPortals` model | No observed route/controller |
| Business setup | ApiSpecs `BusinessInvestor.yaml`, docs | Planned contract only |

## E. Future Feature Inventory

### Strong Evidence

These features are directly named in docs, specs, or model names.

| Feature | Evidence |
| --- | --- |
| Business options | `BusinessInvestor.yaml`, `FLOW.md`, backend requirements |
| Business setup plan | `BusinessInvestor.yaml`, `FLOW.md` |
| Property selection | `FLOW.md`, `RULE_BASED.md` |
| Partner assignment | `FLOW.md`, `BusinessPartners` model |
| Provider assignment | `FLOW.md`, `ProviderPortals` model |
| Payments | `Revenue_Expenses.md`, `Transactions` model |
| Portfolio | `FLOW.md`, frontend requirements |
| Support tickets | `FLOW.md`, backend requirements |
| Admin | `FLOW.md`, frontend/backend requirements |

### Probable Features

These are implied by current direction but need explicit specification:

- saved business drafts;
- option comparison;
- property reservation;
- setup milestone proof uploads;
- document verification;
- notification center;
- subscriptions;
- payout management.

### Suggested Features

These are engineering suggestions, not observed product commitments:

- OpenAPI drift CI.
- Generated frontend API client.
- Dedicated role-based authorization layer.
- Vite migration for Cygnus.
- Atlas-driven AI task registry.

## F. Atlas Submodule Design

### Chosen Name

`atlas`

### Purpose

Atlas is a map and memory for the YourBazar codebase. It is designed for AI agents and maintainers who need to understand the current system, active plans, risks, decisions, and next work.

### Public Interface

Atlas public interface is Markdown:

- `README.md` for entry.
- `reports/YOURBAZAR_REPOSITORY_AUDIT_AND_ATLAS_BLUEPRINT.md` for full context.
- `plans/active-plan.md` for current work.
- `prompts/implementation-agent-prompt.md` for future agents.

### Internal Modules

Atlas organizes knowledge into:

- context;
- product;
- engineering;
- plans;
- decisions;
- prompts;
- reports;
- templates.

### Extension Points

Future Atlas improvements can include:

- generated route maps;
- CI status snapshots;
- machine-readable YAML or JSON task registry;
- automated OpenAPI drift reports;
- release notes;
- deployment runbooks.

## G. Implementation Plan

Completed implementation sequence for Atlas:

1. Create `YourBazar/atlas` GitHub repository.
2. Create `services/atlas` local repository.
3. Add documentation structure.
4. Add security audit workflow.
5. Add full audit report.
6. Commit and push Atlas.
7. Add Atlas as a root submodule.
8. Update root README and `.gitmodules`.
9. Commit and push root submodule pointer.

## H. File-Level Change Summary

Atlas files created:

| File or Directory | Reason |
| --- | --- |
| `README.md` | Entry point and usage rule |
| `context/*` | Repository memory and operational context |
| `product/*` | Product direction and feature inventory |
| `engineering/*` | Technical notes by subsystem |
| `plans/*` | Active roadmap and remediation plans |
| `decisions/*` | ADRs for repo structure and Atlas |
| `prompts/*` | Reusable AI-agent prompts |
| `templates/*` | Reusable work templates |
| `reports/YOURBAZAR_REPOSITORY_AUDIT_AND_ATLAS_BLUEPRINT.md` | Complete audit and blueprint |
| `.github/workflows/security-audit.yml` | Secret scan for Atlas |

Root files to update:

| File | Reason |
| --- | --- |
| `.gitmodules` | Register Atlas as a submodule |
| `README.md` | Add Atlas to repository layout |
| `services/atlas` gitlink | Pin Atlas submodule commit |

## I. Validation Plan

### Atlas Validation

```sh
git -C services/atlas status --short --branch
git -C services/atlas log -1 --pretty=fuller
```

Expected:

- clean working tree;
- latest commit authored by Jay Prakash Sonkar.

### Root Validation

```sh
git status --short --branch
git submodule status
```

Expected:

- clean root working tree after root commit;
- Atlas listed as submodule.

### CI Validation

Expected Atlas workflow:

- gitleaks Docker scan runs on push to `main`.

## J. Risks and Assumptions

### Observed Risks

- ApiSpecs drift from backend.
- Backend sensitive logging needs hardening.
- Cygnus local storage token handling is not ideal for production.
- CRA/react-app-rewired is a modernization risk.
- Dockyard default credentials are development-only and must not be reused.

### Assumptions

- Atlas should not alter runtime behavior.
- Atlas should be updated as part of meaningful implementation work.
- Root repo remains the integration snapshot for submodules.
- Pushes go to `main` unless the user says otherwise.

### Uncertainty

- Exact production hosting targets were inferred from sync scripts and prior context, not fully proven from local files alone.
- Some Git submodule commands fail in the current Windows shell because Git Unix helper tools are missing from PATH.

## K. Final Recommendation

Maintain Atlas as the durable memory layer for YourBazar. Before major work, future AI agents should read:

1. `services/atlas/README.md`
2. `services/atlas/context/current-state.md`
3. `services/atlas/plans/active-plan.md`
4. `services/atlas/reports/YOURBAZAR_REPOSITORY_AUDIT_AND_ATLAS_BLUEPRINT.md`

The next best engineering moves are:

1. Sync ApiSpecs with actual Centaurus routes.
2. Harden Centaurus logging and CORS.
3. Verify and fix Cygnus auth response parsing.
4. Design the business setup API contract.
5. Convert planned business setup flows into tests, API specs, backend routes, and frontend dashboards.

Atlas should be updated whenever those decisions or implementations change.

## L. AI Agent Organization

Atlas now includes a formal AI-agent operating model under `Yourbazar/`. This layer defines the AI HR system for YourBazar: agent roster, ownership boundaries, communication protocol, task routing, MCP policy, operating modes, and reusable handoff artifacts.

Primary entry points:

| Path | Purpose |
| --- | --- |
| `Yourbazar/00_Overview/ai-operating-model.md` | Overall AI HR and multi-agent operating model |
| `Yourbazar/06_Agents/agent-roster.md` | Current agent team and ownership map |
| `Yourbazar/10_Artifacts/communication-protocol.md` | Required agent-to-agent communication format |
| `Yourbazar/08_MCP/mcp-policy.md` | MCP creation policy and evaluation criteria |
| `Yourbazar/10_Artifacts/execution-checklist.md` | Start/close checklist for major tasks |

The agent model is documentation-first. No runtime behavior changes are implied by the agent roster. Agents are activated by task context and should preserve repository evidence, maintain Atlas, and hand off work through structured artifacts.
