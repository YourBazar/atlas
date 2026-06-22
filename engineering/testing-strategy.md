# Testing Strategy

## Current Tests

Centaurus has pytest integration tests for:

- database setup;
- utility helpers;
- auth helpers;
- auth decorators;
- signup/signin;
- portal handlers;
- business handlers;
- business profile handlers.

Cygnus currently has build and test scripts, but no meaningful UI test inventory was observed.

## Required Test Expansion

- Contract tests for route response shapes.
- Authorization tests across users and roles.
- Setup-plan lifecycle tests.
- Payment state transition tests.
- Frontend form and route guard tests.
- API spec drift checks.
