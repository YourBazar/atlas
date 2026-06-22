# Centaurus Logging Gap Analysis

## Current Pattern

Centaurus logs at several layers:

- App/middleware lifecycle and request completion in `app/app.py`.
- Request and response payload summaries in `app/views/base_handler.py`.
- Controller-level request payloads in `app/controllers/base_controller.py`.
- Domain-specific warnings/errors in auth, portal, business, and profile controllers.
- Trace spans through `app/otel/tracing_decorator.py`.
- Script execution logs through the `-f` Bash logging bootstrap.

## Immediate Risks

| Risk | Evidence | Required task |
| --- | --- | --- |
| Sensitive request fields in logs | `base_handler.py` and controller logs can include password-bearing request payloads. | Add centralized redaction for `password`, tokens, auth headers, checksums, database URLs, and secrets. |
| Noisy expected errors | Expected 401/422 paths are often logged as errors with tracebacks. | Downgrade expected client failures to warning/info and reserve error logs for server faults. |
| Local artifact noise | `ci-test.sh` tries `/mnt/artifacts` on local Windows Git Bash and logs permission errors after successful tests. | Make artifact export conditional on CI or configurable artifact path. |
| Deprecated status constant warning | `app/app.py` uses `HTTP_422_UNPROCESSABLE_ENTITY`. | Replace with `HTTP_422_UNPROCESSABLE_CONTENT`. |
| Pydantic schema warning | `portal_serializers.py` uses `Field(example=...)`. | Move example into `json_schema_extra`. |

## Agent Assignment

Primary owner: `Infrastructure_DevOps_Agent`.

Assigned execution agent: Ohm, Centaurus Logging Agent.

Supporting agents:

- `Backend_Service_Agent` for expected exception classifications.
- `Security_Agent` for redaction policy.
- `Testing_QA_Agent` for regression tests covering redaction and warning cleanup.

## Done Criteria

- Logs do not print passwords, tokens, raw auth headers, raw database URLs, or full environment dumps.
- Expected 4xx responses do not emit server-error tracebacks.
- `bash test.sh -f` remains useful locally and does not emit `/mnt/artifacts` errors outside CI.
- Warning count is reduced by addressing local code/config warnings.

## Ohm Findings

Ohm confirmed the following exact hotspots:

- `app/views/base_handler.py`
  - `BaseHandler.log_request` logs full headers and JSON payloads.
  - `BaseHandler.log_response` can log auth response data containing `access_token`.
- `app/controllers/base_controller.py`
  - `set_controller` logs full `request_payload`, headers, path/query params, and kwargs.
- `app/views/auth_handler.py`
  - Signup/signin paths debug-log auth payloads that can include passwords and checksum headers.
- `app/app.py`
  - Middleware logs incoming/completed requests while Uvicorn access logs are also enabled.
  - `logger.debug("Environment: %s", dict(os.environ))` can expose secrets.
  - CORS exposes `X-Request-ID`, but middleware does not currently create/read/return one.
- `app/configs/redis_configs.py`
  - Redis connection string logging can expose Redis passwords.
- `app/otel/tracing_decorator.py`
  - Custom errors are attached, but standard `record_exception` and span status are not consistently used.
- `app/decorators/auth_decorators.py` and `app/decorators/payload_decorators.py`
  - Validation work is not traced as child spans.
- `app/connectors/sql_connector.py` and `app/connectors/redis_connector.py`
  - Connector operations do not have child spans with safe operation metadata.
- `app/otel/custom_span.py` and `app/helpers/auth_helper.py`
  - Runtime `print(...)` calls bypass structured logging.

Prioritized implementation order:

1. Add centralized redaction for headers, payloads, responses, and selected environment values.
2. Apply redaction in base handler, base controller, and auth handler logs.
3. Remove full environment logging or replace it with an allowlisted summary.
4. Mask Redis URLs.
5. Add request ID middleware that accepts or creates `X-Request-ID`, returns it, logs it, and sets it on the active span.
6. Decide whether middleware or Uvicorn owns request access logs at `INFO`.
7. Add safe tracing around auth/payload decorators and DB operations.
8. Replace prints with module loggers.
9. Add tests proving raw passwords, JWTs, checksums, and authorization headers do not appear in logs.
