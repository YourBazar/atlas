# Infrastructure: Dockyard

Dockyard provides Docker Compose infrastructure for local development.

## Supported Services

- PostgreSQL
- MySQL
- Redis
- Kafka
- Zookeeper
- Kafka UI
- Prometheus
- Grafana
- Exporters
- Monitor container

## Configuration Pattern

Services are enabled or disabled through `.env` flags such as `BUILD_POSTGRES`, `BUILD_REDIS`, and similar names.

## Risks

- Development credentials must not be reused in production.
- Docker cleanup scripts can be broad and destructive.
- Atlas should preserve operational cautions around infrastructure scripts.

## Current Improvement Assignment

Primary owner: Infrastructure DevOps Agent.

Audit agent: Herschel, Dockyard Infrastructure Improvement Agent.

Supporting agents:

- Security Agent for local secret hygiene and localhost binding.
- Backend Service Agent for Centaurus database compatibility.
- Documentation Agent for README and Atlas updates.
- Testing QA Agent for smoke validation when Docker is available.

## Current Updates

- Dockyard now uses `.env.example` as the committed local-development template and ignores `.env`.
- Published service ports bind to `127.0.0.1` by default through `BIND_HOST`.
- PostgreSQL test database creation uses `POSTGRES_TEST_DB` and is idempotent.
- PostgreSQL exporter uses the same `POSTGRES_DB_USERNAME` and `POSTGRES_DB_PASSWORD` values as the Postgres service.
- Kafka host port mapping now targets the external listener port.
- Redis has a basic health check.
- Control scripts use Bash and support both `docker compose` and legacy `docker-compose`.
- README documents Centaurus local Postgres settings and clarifies that Dockyard is local infrastructure only, not a production deploy owner.

## Remaining Work

- Add fuller health checks for MySQL, Kafka, MinIO, Prometheus, Grafana, and exporters.
- Consider Compose profiles if direct `docker compose up` should respect service toggles without using `build_setup.sh`.
- Validate the full enabled service matrix on a host with Docker Compose available.
