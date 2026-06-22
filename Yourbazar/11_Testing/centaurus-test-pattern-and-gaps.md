# Centaurus Test Pattern And Gap Analysis

## Current Pattern

Centaurus uses integration-first pytest coverage.

The structure is ordered by numbered folders:

| Folder | Responsibility |
| --- | --- |
| `0000_test_db_connection_and_template` | Database, root route, Redis connector smoke coverage. |
| `0001_test_utils` | Utility, token, encryption, hashing, masking, and helper coverage. |
| `0002_test_decorators` | Auth decorator behavior. |
| `0003_test_auth_handlers` | Signup and signin route behavior. |
| `0004_test_portal_handlers` | Portal CRUD route behavior. |
| `0005_test_business_handlers` | Business CRUD and collection route behavior. |
| `0006_test_business_profile_handlers` | Business profile CRUD route behavior. |

Common conventions:

- Async tests use `pytest.mark.asyncio`.
- Route tests use the shared `test_client` fixture from `tests/conftest.py`.
- Randomized entity data comes from fixture factories, not hard-coded shared records.
- Payload-protected endpoints compute and pass `x-payload-auth` checksums.
- Connector tests instantiate `SQLConnector` or `RedisConnector` directly.
- Tests assert HTTP status codes and key response fields instead of only checking non-empty responses.

## Current Validation State

Latest analyzed `test.log`:

```text
87 passed, 9 skipped, 14 warnings
```

The skipped tests are Redis DB-less tests. They skip because local Windows execution cannot use `redislite`, and `REDIS_DB_LESS=true` is active.

Warnings to clean:

- Pydantic `Field(example=...)` deprecation in `app/serializers/portal_serializers.py`.
- Unknown pytest option `json_report` in `setup.cfg`.
- Starlette deprecation for `HTTP_422_UNPROCESSABLE_ENTITY` in `app/app.py`.
- Local artifact export failure from `ci-test.sh` when `/mnt/artifacts` is unavailable.

## Missing Scenario Backlog

| Area | Missing scenarios | Preferred location |
| --- | --- | --- |
| Auth | Invalid checksum, missing checksum, duplicate mobile, duplicate email, inactive user signin, invalid password boundaries, token response shape. | `tests/integrations/0003_test_auth_handlers/test_ord0000_auth_handler.py` |
| Auth decorators | Missing bearer prefix, malformed token, expired token, wrong user id, inactive user id. | `tests/integrations/0002_test_decorators/test_ord0000_auth_decorators.py` |
| Payload decorators | Body mutation after checksum, empty body with checksum, invalid JSON body, missing header. | New `tests/integrations/0002_test_decorators/test_ord0001_payload_decorators.py` |
| SQL connector | Duplicate insert behavior, not-found update/delete, rollback on failed bulk operation, invalid key, invalid model. | `tests/integrations/0000_test_db_connection_and_template/test_ord0001_sql_db_connection.py` |
| Redis connector | Standalone Redis tests gated by env, clearer skip validation for DB-less mode, unavailable Redis failure behavior. | `tests/integrations/0000_test_db_connection_and_template/test_ord0003_redis_db_connection.py` |
| Portal handlers | Unauthorized requests, invalid enum values, inactive/deleted portal lookup, duplicate names if constrained, update partial payloads. | `tests/integrations/0004_test_portal_handlers/test_ord0000_portal_handler.py` |
| Business handlers | Unauthorized requests, invalid portal relation, deleted business lookup, invalid bulk payloads, optional field behavior. | `tests/integrations/0005_test_business_handlers/test_ord0000_business_handler.py` |
| Business profile handlers | Unauthorized requests, invalid business relation, deleted profile lookup, partial update validation, email/mobile validation. | `tests/integrations/0006_test_business_profile_handlers/test_ord0000_business_profile_handler.py` |
| Serializers | Invalid enum values, required fields, optional update serializers, schema examples. | New `tests/integrations/0007_test_serializers/` |
| Config | `DATABASE_URL` normalization, `POSTGRES_DB_URL` fallback, SQLite path generation, Redis DB-less config. | New `tests/integrations/0008_test_configs/` |

## Agent Assignment

Primary owner: `Testing_QA_Agent`.

Assigned execution agent: Planck, Centaurus Testing Gap Agent.

Supporting agents:

- `Backend_Service_Agent` for route/controller behavior clarification.
- `Infrastructure_DevOps_Agent` for CI and environment-specific skips.
- `Security_Agent` for auth and sensitive-data test cases.

## Planck Findings

Planck confirmed that new tests should be added to existing ordered integration files unless a new concern deserves its own ordered folder.

Additional concrete tasks from the agent review:

- Add auth tests for missing `x-payload-auth`, tampered checksums, invalid email format, short passwords, invalid mobile lengths, unknown valid-format signin email, and `token_type == "Bearer"` response shape.
- Add decorator tests for missing `request` kwarg, malformed authorization header, expired token, payload-validator success, missing checksum, invalid JSON body, and checksum mismatch.
- Add SQL connector tests for `fetch_data(..., fetch_type="all")`, invalid SQL returning safe fallback values, duplicate unique email insert, failed bulk insert, non-existing update/delete behavior, and `object_to_dict` / `dict_to_object` round trip.
- Add Redis tests for invalid mode, sentinel mode without hosts/master, cluster mode without nodes, `_decode_if_bytes`, safe `close()` with no Redis client, and `get_all_data` set/zset branches.
- Add portal tests for `portal_type=ALL`, invalid `portal_type`, invalid `privacy`, empty PUT body, invalid portal id, and missing auth headers.
- Add business tests for invalid user identifier, empty update payload, wrong user id scope, invalid `user_identifier`, and delete side effects on related profiles.
- Add business profile tests for invalid business/profile identifiers, missing name, empty update body behavior, and `social_media_profiles` round trip.
- Add serializer/config folders:
  - `tests/integrations/0007_test_serializers/test_ord0000_serializers.py`
  - `tests/integrations/0008_test_configs/test_ord0000_config.py`

Pattern constraints:

- Reuse `URLS` from `tests/constants.py`.
- Reuse checksum generation with `get_checksum(payload)`.
- Keep route tests against `test_client`.
- Keep external Redis modes skipped unless explicitly configured.
