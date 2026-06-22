# ADR 0003: Atlas as AI Context Repository

## Status

Accepted.

## Decision

Create `services/atlas` as the AI context, architecture, planning, and handoff repository.

## Rationale

YourBazar has multiple submodules, evolving product scope, CI/security context, deployment-specific knowledge, and long-running AI collaboration. A durable context repository prevents repeated rediscovery.

## Consequence

Atlas must be updated when major implementation, security, deployment, or roadmap changes occur.
