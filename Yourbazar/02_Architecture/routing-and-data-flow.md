# Routing and Data Flow

## Frontend Routes

- `/`
- `/home`
- `/about`
- `/signup`
- `/signin`
- `/signout`
- `/investor/*`
- `/partner/*`
- `/provider/*`

## Backend Route Groups

- auth;
- portals;
- businesses;
- business profiles.

## Data Flow

Cygnus sends requests to Centaurus through `BACKEND_URLS` and `ApiCaller`. Centaurus validates payload checksums, validates auth for protected flows, calls controllers, and persists through SQLAlchemy.
