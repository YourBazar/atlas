# API Sync Plan

1. Generate current OpenAPI from Centaurus `/openapi.yaml`.
2. Compare generated routes with `services/ApiSpecs/centaurus.yaml`.
3. Replace stale auth and portal paths.
4. Add businesses and business profile routes.
5. Mark BusinessInvestor APIs as planned, not implemented.
6. Add a CI check that fails when backend routes change without spec updates.
