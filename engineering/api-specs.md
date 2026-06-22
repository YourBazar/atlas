# API Specs

## Current State

`services/ApiSpecs` contains:

- `centaurus.yaml`
- `BusinessInvestor.yaml`
- `sample.yaml`

## Drift

`centaurus.yaml` documents older auth-focused routes and is not synchronized with the current Centaurus route map.

`BusinessInvestor.yaml` documents planned business investor option/setup APIs that are not fully implemented.

## Recommendation

Before new runtime API work, update ApiSpecs to reflect current Centaurus routes, then use it as the source of future API review.
