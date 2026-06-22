# Gap Analysis

## High-Impact Gaps

- ApiSpecs are stale relative to Centaurus.
- Frontend auth response parsing needs verification.
- Backend logging needs security hardening.
- Role pages are not yet functional dashboards.
- Future business setup models and flows lack API implementation.
- Cygnus has visible UI dead ends: unresolved `/community`, `/events`, and `/resources` links, inert primary CTAs, simulated role join redirects, and no actionable role dashboards.
- Cygnus shared layout and UX states need hardening: fixed footer overlap risk, header collapse accessibility mismatch, broad global CSS overrides, generic auth errors, and incomplete field validation.
- Dockyard local infrastructure needed hardening around committed `.env` defaults, Postgres init variables, exporter credentials, Kafka port mapping, localhost binding, and script portability.

## Next Analysis Tasks

- Generate current Centaurus OpenAPI.
- Compare current backend routes to ApiSpecs.
- Map every frontend page to backend data needs.
- Convert product flow docs into feature specs.
- Execute the Cygnus UI gap closure sequence from `reports/CYGNUS_UI_GAP_AUDIT.md`.
- Validate Dockyard Compose startup on a Docker-capable host after the first hardening pass.
