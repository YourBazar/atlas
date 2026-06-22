# Cygnus Logging Gap Analysis

## Current Pattern

Cygnus has a small frontend logger in `src/utils/logger.js`.

Observed behavior:

- Logs are disabled when `REACT_APP_NODE_ENV === "production"`.
- The logger delegates to `console[level]`.
- `src/utils/logger.js` logs `"Logger imported"` at import time.
- `src/configs/Urls.js` logs frontend and backend domains at import time.
- `src/helpers/ApiCaller.js` logs successful API responses and detailed API errors.

## Main Risks

| Risk | Evidence | Required task |
| --- | --- | --- |
| Sensitive API error logging | `ApiCaller.js` logs full `error.config`, which can include request body, password, `x-payload-auth`, authorization headers, and backend URL. | Add frontend redaction before logging Axios config/headers/body. |
| Sensitive response logging | `ApiCaller.js` logs full response data, which can include `access_token` and user identifiers. | Log response summaries instead of raw payloads. |
| Import-time console noise | `logger.js` and `Urls.js` log during module import. | Remove import-time `console.log` calls or route through debug logger with redaction. |
| User-safe vs developer logs are mixed | Auth pages show generic modal errors but log raw error objects. | Log sanitized error metadata only. |
| No logging tests | No committed frontend tests cover logger or API redaction behavior. | Add Jest tests for redaction and logger production gating. |

## Prioritized Tasks

1. Add a frontend redaction helper for keys such as `password`, `accessToken`, `access_token`, `Authorization`, `x-authorization`, `x-payload-auth`, `token`, and `secret`.
2. Update `ApiCaller.js` to log method, URL path, status, and safe error message, not raw config/data.
3. Remove `console.log("Logger imported")` from `logger.js`.
4. Remove or debug-gate frontend/backend domain logs in `Urls.js`.
5. Update signin/signup error logging to avoid logging raw Error objects if they contain Axios config.
6. Add Jest tests for API logging redaction and production logger suppression.

## Kant Findings

Kant confirmed the following concrete risks:

- `src/helpers/ApiCaller.js` logs full `response.data`, `error.response.data`, `error.config`, and stack traces. For auth calls this can expose passwords, access tokens, user IDs, headers, checksums, and backend payloads.
- `src/utils/logger.js` suppresses logs based on `REACT_APP_NODE_ENV`, but Create React App normally provides `process.env.NODE_ENV`; production suppression may be unreliable unless the custom env var is always set.
- `src/configs/Urls.js` logs frontend/backend domains unconditionally during import.
- `apiCaller` rethrows a plain `Error`, losing structured fields like status, timeout/network classification, backend error code, retryability, and request/correlation IDs.
- `Signin.js` and `Signup.js` show overly generic modal messages. They should distinguish safe user-facing cases such as invalid credentials, duplicate account, validation failure, offline/timeout, and server error.
- Auth submit handlers should use `finally { setIsLoading(false); }` so loading state always clears.
- `investor/Join.js`, `partner/Join.js`, and `provider/Join.js` log navigation errors but leave users on `Loading...` with no fallback.
- `App.js` has no error boundary, and `index.js` calls `reportWebVitals()` without a sink.
- Access tokens are stored in `localStorage`, increasing the severity of console/config logging and XSS exposure.

Recommended implementation sequence:

1. Sanitize API diagnostics and remove raw Axios config logging.
2. Remove import-time console calls.
3. Switch logger production gating to `process.env.NODE_ENV`.
4. Throw structured app errors from `apiCaller`.
5. Improve auth user-visible errors and loading cleanup.
6. Add join-page fallback states.
7. Add an app-level error boundary.
8. Add redaction and logger tests before changing token-storage strategy.

## Agent Assignment

Primary owner: `Infrastructure_DevOps_Agent`.

Assigned execution agent: Kant, Cygnus Logging Agent.

Supporting agents:

- `Frontend_UX_Agent` for user-visible error states.
- `Security_Agent` for redaction rules.
- `Testing_QA_Agent` for regression tests.
