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
