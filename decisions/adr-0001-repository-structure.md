# ADR 0001: Repository Structure

## Status

Accepted.

## Decision

YourBazar uses an umbrella repository with Git submodules under `services/`.

## Rationale

The existing repositories are independently owned and pushed, while the root repo preserves an integration snapshot.

## Consequence

Any submodule change requires two commits: one in the submodule and one in the root repo for the updated pointer.
