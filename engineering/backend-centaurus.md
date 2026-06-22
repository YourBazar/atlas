# Backend: Centaurus

## Stack

- FastAPI
- SQLAlchemy async
- Pydantic
- Uvicorn
- OpenTelemetry
- Pytest

## Main Patterns

- `app/urls/urls.py` registers routes with `router.add_api_route`.
- `app/views/*_handler.py` handles FastAPI request details.
- Decorators validate payload checksum and auth headers.
- `app/controllers/*_controller.py` owns business logic.
- `app/connectors/sql_connector.py` owns database access.
- `app/models/models.py` owns SQLAlchemy models.
- `app/serializers/*_serializers.py` owns request/response schemas.

## Current Routes

- Auth: signup, signin.
- Portals: list, create, get, update, delete.
- Businesses: list, create, get, update, delete.
- Business profiles: list, create, get, update, delete.

## Risks

- Sensitive logging needs hardening.
- CORS defaults should be production-specific.
- Some future models lack controller/route coverage.
- API specs are stale.
