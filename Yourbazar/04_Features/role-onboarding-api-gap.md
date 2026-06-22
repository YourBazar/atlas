# Role Onboarding API Gap

## Status

Observed frontend gap. Backend verification required.

## Evidence

Cygnus has role onboarding routes for:

- `services/cygnus/src/pages/investor/Join.js`
- `services/cygnus/src/pages/partner/Join.js`
- `services/cygnus/src/pages/provider/Join.js`

Each page currently waits briefly and redirects to the matching protected role workspace. There is no frontend API call in those screens to persist role selection, confirm onboarding, or retrieve role status.

## Required Backend Agent Task

The Backend Service Agent should inspect Centaurus for existing user role, profile, permission, or membership APIs. If none exists, the agent should design and implement a minimal role onboarding API that supports Cygnus without introducing broad product assumptions.

## Proposed Minimal Contract

The exact contract should be confirmed against Centaurus conventions, but Cygnus likely needs:

- `POST` endpoint to enroll the current user in one role path.
- `GET` endpoint to return current user role enrollment status.
- Standard Centaurus auth, checksum, logging, tracing, and response shape.

## Risks

- Cygnus currently presents role onboarding as successful even when nothing is persisted.
- The authenticated dashboards may eventually need role-specific authorization checks.
- Any backend schema change must be coordinated with existing user models and migrations.
