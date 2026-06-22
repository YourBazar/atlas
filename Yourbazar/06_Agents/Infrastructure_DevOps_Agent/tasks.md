# Tasks

- Maintain CI/CD context.
- Validate Docker and environment scripts.
- Document deploy and sync behavior.
- Improve reliability of Actions.

## Active Task: Dockyard Infrastructure Hardening

Status: in progress.

Scope:

- Improve `services/Dockyard` local infrastructure safety, Compose correctness, env handling, script portability, and Centaurus development alignment.

Completed:

- Assigned audit to Herschel, Dockyard Infrastructure Improvement Agent.
- Identified Postgres init, exporter credential, Kafka listener, local secret, port binding, script portability, and documentation gaps.
- Applied first hardening pass in Dockyard.

Next:

- Validate `docker compose config` and service startup on a host where Docker Compose is available.
- Add additional health checks and consider Compose profiles.

## Active Task: Centaurus Logging Improvements

Status: assigned.

Assigned agent: Ohm, Centaurus Logging Agent.

Goal:

- Improve Centaurus logging reliability, signal quality, and secret safety without changing business behavior.

Required analysis areas:

- Request/response logging in `app/views/base_handler.py` and middleware logging in `app/app.py`.
- Sensitive fields currently visible in logs, especially passwords, auth headers, payload auth hashes, tokens, database URLs, and environment dumps.
- OpenTelemetry trace/log correlation in `app/otel/*` and controller tracing decorators.
- Error logging consistency across `auth_handler.py`, `portal_handler.py`, `business_handler.py`, and `business_profile_handler.py`.
- Script log behavior for `test.sh`, `run.sh`, `connectionTest.sh`, `refreshinstall.sh`, `rinstall.sh`, and `runinstall.sh`.
- Local artifact behavior from `ci-test.sh`, especially `/mnt/artifacts` permission failures during local Git Bash runs.

Done means:

- Logging task list identifies exact files and functions to change.
- Secret redaction rules are explicit.
- Local and CI logging behavior is separated cleanly.
- Existing tests still pass with `bash test.sh -f`.

## Active Task: Cygnus Logging Improvements

Status: assigned.

Assigned agent: Kant, Cygnus Logging Agent.

Goal:

- Improve Cygnus frontend logging, error visibility, and sensitive-data safety without changing user-facing behavior.

Required analysis areas:

- `src/utils/logger.js`: production gating, import-time console log, log level behavior, and testability.
- `src/helpers/ApiCaller.js`: full response/config logging, Axios error logging, header/body redaction, and useful error messages.
- `src/configs/Urls.js`: import-time console logs for frontend/backend domains.
- Auth pages: logging of signin/signup failures without passwords, tokens, checksums, or full Axios config.
- User-visible error states: modal messages should remain user-safe while logs retain actionable non-secret diagnostics.
- CI behavior: test workflow should capture logs/artifacts without committing generated logs.

Done means:

- A redaction strategy exists for frontend logs.
- `console.log` side effects at import time are removed or routed through logger.
- API errors do not expose passwords, access tokens, auth headers, checksum headers, or full request bodies.
- Tests cover sensitive-data redaction for API errors and auth failures.
