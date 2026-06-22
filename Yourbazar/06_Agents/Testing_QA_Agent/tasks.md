# Tasks

- Maintain test plan.
- Define acceptance scenarios.
- Identify coverage gaps.
- Record validation outcomes.

## Active Task: Centaurus Missing Test Scenarios

Status: assigned.

Assigned agent: Planck, Centaurus Testing Gap Agent.

Goal:

- Add missing Centaurus test scenarios while preserving the current test style and folder ordering.

Current test pattern to preserve:

- Use `pytest` and `pytest.mark.asyncio` for async tests.
- Keep integration tests under `tests/integrations/<ordered_area>/test_ordNNNN_<subject>.py`.
- Reuse `tests/conftest.py` fixtures such as `test_client`, `generate_random_user_data`, `random_business_data`, and `random_business_profile_data`.
- Use `httpx.AsyncClient` with ASGI transport for route tests.
- Use checksum headers for payload-protected routes, matching current auth, portal, business, and business profile handler tests.
- Keep direct connector coverage in `0000_test_db_connection_and_template`.
- Prefer deterministic assertions on response status, message, and `data` shape.

Required gap analysis areas:

- Auth: duplicate mobile/email variants, invalid checksum, malformed checksum, expired/invalid access token behavior, inactive user signin, password edge cases, and response shape consistency.
- SQL connector: not-found paths, transaction rollback behavior, invalid model/key handling, duplicate constraint handling, and SQLite/Postgres behavior differences where practical.
- Redis connector: explicit DB-less skip reason, standalone mode coverage behind env gates, and failure behavior when Redis is unavailable.
- Decorators: payload checksum missing/invalid/body mutation cases and auth header variants.
- Portal/business/business profile handlers: unauthorized access, invalid identifiers, inactive records, bulk-operation edge cases, missing optional fields, privacy/type enum validation, and delete-after-get behavior.
- Serializers: invalid enum values, field length/format validation, optional update payloads, and schema examples.
- Config/runtime: `SQL_DB_LESS`, `DATABASE_URL`, `POSTGRES_DB_URL`, `REDIS_DB_LESS`, and environment fallback behavior.

Done means:

- New tests are added in the existing numbered integration style.
- Tests pass locally with `bash test.sh -f`.
- Any intentionally skipped tests include clear skip reasons.
- Coverage report is checked for the touched files.

## Active Task: Cygnus Missing Test Scenarios

Status: assigned.

Assigned agent: Helmholtz, Cygnus Testing Gap Agent.

Goal:

- Add missing Cygnus frontend tests while preserving the React app's current Jest and React Testing Library conventions.

Current observed pattern:

- Cygnus uses Create React App tooling through `react-app-rewired`.
- `package.json` test command currently allows no tests with `--passWithNoTests`.
- `src/setupTests.js` enables Testing Library matchers.
- API behavior is centralized through `src/helpers/ApiCaller.js`.
- Auth state is centralized through `src/helpers/AuthContext.js` and localStorage helper functions in `src/helpers/Utils.js`.
- Auth pages compute `x-payload-auth` with `getChecksum` before calling Centaurus.

Required gap analysis areas:

- Auth pages: signin/signup happy path, failed API path, loading spinner state, localStorage writes, navigation after success, checksum header generation, and modal messages.
- Auth context: initial state from localStorage, signin/signout state transitions, localStorage synchronization.
- Protected routing: unauthenticated redirect to `/signin`, signed-in access to investor/partner/provider routes, signin/signup redirect when already signed in, `NotFound` fallback.
- API caller: supported methods, unsupported method error, success response shape, timeout propagation, server error response handling, network error handling, and redaction of sensitive config in logs.
- Utility helpers: checksum deterministic ordering, storage getters/removers, string case helpers.
- URL config: backend fallback, `REACT_APP_BACKEND_DOMAIN` override, route URL builders.
- Join pages: delayed navigation behavior for investor/partner/provider join pages.

Done means:

- Tests are added under existing `src` conventions using Jest and React Testing Library.
- Axios/API calls are mocked rather than hitting real Centaurus.
- localStorage is cleared between tests.
- `npm test -- --watchAll=false` passes without relying on `--passWithNoTests`.
