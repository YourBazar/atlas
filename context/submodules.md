# Submodules

## `services/cygnus`

React frontend using Create React App via `react-app-rewired`, Bootstrap, React Router, Axios, and local storage auth state.

Primary routes:

- `/`
- `/home`
- `/about`
- `/signup`
- `/signin`
- `/signout`
- `/investor/*`
- `/partner/*`
- `/provider/*`

## `services/centaurus`

FastAPI backend using SQLAlchemy async, Pydantic serializers, JWT helpers, checksum payload validation, and pytest integration tests.

Primary route groups:

- `/users/signup`
- `/users/signin`
- `/users/{user_identifier}/portals`
- `/users/{user_identifier}/portals/{portal_type}`
- `/users/{user_identifier}/portals/{portal_identifier}`
- `/businesses`
- `/businesses/{business_identifier}`
- `/users/{user_identifier}/businesses`
- `/users/{user_identifier}/businesses/{business_identifier}`
- `/businesses/{business_identifier}/profiles`
- `/businesses/{business_identifier}/profiles/{business_profile_identifier}`

## `services/ApiSpecs`

Contains OpenAPI documents. `centaurus.yaml` is auth-focused and stale relative to current backend code. `BusinessInvestor.yaml` describes planned investor setup APIs.

## `services/MarkDowns`

Contains product, flow, business, requirements, revenue, and rule documents. This is the strongest source of future product direction.

## `services/Dockyard`

Local Docker infrastructure for PostgreSQL, MySQL, Redis, Kafka, monitoring, and related exporters. Service activation is environment-driven.

## `services/atlas`

AI context and planning module. It should be updated whenever architecture, roadmap, CI, deployment, or meaningful implementation state changes.
