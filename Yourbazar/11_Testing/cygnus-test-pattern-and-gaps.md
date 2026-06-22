# Cygnus Test Pattern And Gap Analysis

## Current Pattern

Cygnus is a React app built with Create React App conventions through `react-app-rewired`.

Observed test infrastructure:

- `src/setupTests.js` is present for Testing Library matchers.
- `package.json` uses `react-app-rewired test --passWithNoTests --verbose`.
- There are currently no committed test files found under `src`.
- `build-and-test.yml` runs `npm ci`, `npm run build`, and `npm test`.
- `test-cases.yml` has been added to run dependency refresh and `npm test -- --watchAll=false`.
- Tests should be colocated beside source files as `*.test.js`, import through `@/...`, use React Testing Library, and mock modules with `jest.mock(...)`.

## Source Areas To Cover

| Area | Files |
| --- | --- |
| App routing | `src/pages/app/Route.js`, `src/pages/app/AuthenticatedRoute.js` |
| Auth context | `src/helpers/AuthContext.js` |
| API caller | `src/helpers/ApiCaller.js` |
| Auth pages | `src/pages/auth/Signin.js`, `src/pages/auth/Signup.js`, `src/pages/auth/Signout.js` |
| Utility helpers | `src/helpers/Utils.js`, `src/helpers/StringHelpers.js` |
| URL builders | `src/configs/Urls.js` |
| Domain pages | `src/pages/investor/*`, `src/pages/partner/*`, `src/pages/provider/*` |

## Missing Scenario Backlog

| Area | Missing scenarios | Preferred location |
| --- | --- | --- |
| API caller | Success response shape, unsupported method, Axios server error, network error, timeout option, merged headers, sensitive log redaction. | `src/helpers/ApiCaller.test.js` |
| Auth context | Initial state from `localStorage`, signin state transition, signout state transition, persisted `isSignedIn`. | `src/helpers/AuthContext.test.js` |
| Utils | `getChecksum` deterministic ordering, storage getter/remover behavior, empty localStorage fallback. | `src/helpers/Utils.test.js` |
| String helpers | title/snake/camel/kebab/lower/upper conversions and edge inputs. | `src/helpers/StringHelpers.test.js` |
| Routes | Public route rendering, protected route redirect, signed-in protected route access, signin/signup redirect when already signed in, wildcard NotFound. | `src/pages/app/Route.test.js` |
| Signin | Form input, checksum header, success localStorage writes, `signin()` call, navigation, loading spinner, failed API modal. | `src/pages/auth/Signin.test.js` |
| Signup | Required fields, checksum header, success localStorage writes, `signin()` call, navigation, loading spinner, failed API modal. | `src/pages/auth/Signup.test.js` |
| Signout | Removes auth storage, calls `signout()`, navigates home. | `src/pages/auth/Signout.test.js` |
| URL config | Backend fallback and `REACT_APP_BACKEND_DOMAIN` override. | `src/configs/Urls.test.js` |
| Join pages | Investor/partner/provider join navigation after delay. | `src/pages/*/Join.test.js` |

Additional Helmholtz findings:

- Add a regression test for corrupt `localStorage.isSignedIn` JSON because current `JSON.parse(...)` can throw during auth context initialization.
- Signin/signup tests should use fake timers for the delayed `/home` navigation.
- API tests should mock `axios` directly and mock `@/utils/logger` with no-op `info`/`error`.
- Header tests should verify signed-out vs signed-in nav state and links to `/`, `/home`, `/investor`, `/partner`, `/provider`, and `/about`.
- Role route tests should cover `/`, `/join`, and unknown nested role routes for investor, partner, and provider.
- App home tests should cover join-card links and the currently placeholder links `/community`, `/events`, and `/resources`.
- Once the first tests land, remove `--passWithNoTests` or add a CI guard that fails when no test files exist.

## Testing Rules

- Use Jest and React Testing Library.
- Mock `apiCaller` or Axios; do not call live Centaurus.
- Mock `getChecksum` where page tests only need to assert the header is sent.
- Clear `localStorage` and mocks in `beforeEach`.
- Prefer user-facing queries such as labels, button text, modal text, and navigation output.
- Use fake timers for delayed navigation flows.

## Agent Assignment

Primary owner: `Testing_QA_Agent`.

Assigned execution agent: Helmholtz, Cygnus Testing Gap Agent.

Supporting agents:

- `Frontend_UX_Agent` for user-flow intent.
- `Backend_Service_Agent` for Centaurus response contracts.
- `Infrastructure_DevOps_Agent` for workflow execution and artifacts.
