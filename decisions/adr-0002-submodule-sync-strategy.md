# ADR 0002: Submodule Sync Strategy

## Status

Accepted.

## Decision

Cygnus and Centaurus keep deploy sync scripts that push `main` into personal `deploy` branches when deployment is intended.

## Rationale

This matches the user's existing deployment flow.

## Consequence

Sync scripts are deployment-affecting and should be treated as release actions, not ordinary validation commands.
