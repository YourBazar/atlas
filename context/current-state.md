# Current State

## Implemented

- Umbrella repository with Git submodules.
- Cygnus React frontend.
- Centaurus FastAPI backend.
- User signup and signin.
- JWT generation and validation helpers.
- Payload checksum decorator.
- Role portal CRUD for Investor, Partner, and Provider portal types.
- Business CRUD.
- Business profile CRUD.
- SQLAlchemy async connector.
- PostgreSQL and SQLite-style DB-less development mode.
- Integration tests for backend auth, decorators, portals, businesses, and business profiles.
- GitHub Actions security audits across root and submodules.
- Sync scripts for pushing Cygnus and Centaurus `main` into personal `deploy` branches.

## Partially Implemented

- Business, investment, expense, earning, transaction, investor, partner, and provider data model surface.
- Role home pages in Cygnus communicate product intent but do not yet provide dashboard workflows.
- API specifications exist but are not synchronized with backend.
- Local infrastructure is broad, but most services are optional and controlled by `.env`.

## Not Yet Implemented

- Business option catalog.
- Property catalog and property allocation.
- Setup plan creation.
- Milestone tracking.
- Partner/provider assignment workflow.
- Payment/refund workflows.
- Portfolio and earnings views.
- Support tickets and chat.
- Admin workflows.
- API contract automation.
