# Known Issues

## Security and Data Handling

- Dockyard includes environment-style defaults that should be treated as development-only and not production secrets.
- Centaurus has request and environment logging patterns that should be hardened before production.
- Cygnus stores access tokens in local storage.
- CORS defaults in Centaurus need explicit production origins.

## API and Integration

- ApiSpecs are stale relative to current Centaurus routes.
- Cygnus auth response parsing should be verified against Centaurus response shape.
- Frontend route pages are mostly content pages rather than functional dashboards.

## Dependency and Tooling

- Cygnus production dependency audit has been cleaned, but full dev audit still reflects Create React App ecosystem risk.
- Centaurus dependency pins were updated based on pip-audit output; CI should confirm compatibility.
- Some local Git submodule commands fail in this Windows shell because Git Unix helper tools are unavailable on PATH.

## Product Completeness

- The product model is broader than the MVP implementation.
- Several backend models imply future workflows without endpoints.
- MarkDowns define flows that are not yet represented as contracts or executable code.
