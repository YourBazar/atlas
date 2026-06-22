# YourBazar Bash Script Reference

## Logging Convention

All tracked `.sh` scripts now support a common `-f` flag.

When `-f` is provided, the script writes all stdout and stderr to a log file named after the script without the `.sh` suffix:

```bash
bash build.sh -f
# logs to build.log

bash services/centaurus/connectionTest.sh -f
# logs to connectionTest.log
```

The logs are also printed to the terminal through `tee`, so existing interactive behavior is preserved.

## Root Repository Scripts

| Script | Purpose | Primary side effects | Notes |
| --- | --- | --- | --- |
| `build.sh` | Iterates through `services/*` and runs each submodule `build.sh` when present. | Executes submodule build scripts; may create containers or build artifacts depending on each submodule. | Use from repository root. With `-f`, root output goes to `build.log`; submodule scripts only create their own logs when called with `-f` directly. |
| `docker_cleanup.sh` | Stops and removes all Docker containers, removes all Docker images, then prunes volumes and networks. | Destructive Docker cleanup across the whole local Docker daemon. | Use cautiously. This is not scoped to YourBazar containers. |

## Dockyard Scripts

| Script | Purpose | Primary side effects | Notes |
| --- | --- | --- | --- |
| `services/Dockyard/build_setup.sh` | Loads `.env`, normalizes enabled service flags, generates config, pulls images, starts enabled Docker Compose services, and verifies container state. | Creates generated config files, pulls Docker images, starts containers. | Main Dockyard local infrastructure entrypoint. |
| `services/Dockyard/conf_generator.sh` | Generates Prometheus, Grafana, MySQL exporter, and Kafka UI config files based on `.env` feature flags. | Writes files under `prometheus/`, `grafana/`, `mysql_exporter/`, and `kafka_ui/`. | Called by `build_setup.sh`; can be run directly after `.env` is present. |
| `services/Dockyard/start_setup.sh` | Starts existing stopped Docker Compose containers. | Starts containers from the current Compose project. | Does not rebuild images or recreate containers. |
| `services/Dockyard/pause_setup.sh` | Stops running Docker Compose containers without deleting them. | Stops containers while preserving volumes and container definitions. | Use when pausing local infrastructure. |
| `services/Dockyard/delete_setup.sh` | Runs `docker compose down --volumes --remove-orphans`. | Removes containers, networks, anonymous volumes, and orphan containers for the Compose project. | More destructive than `pause_setup.sh`. |
| `services/Dockyard/rebuild_setup.sh` | Runs `delete_setup.sh` and then `build_setup.sh`. | Recreates Dockyard services from scratch. | Useful when config or images change. |
| `services/Dockyard/postgres_db_init.sh` | Creates the configured Postgres test database if it does not already exist. | Connects through `psql` and creates `POSTGRES_TEST_DB`. | Intended for Postgres container initialization. |

## Cygnus Scripts

| Script | Purpose | Primary side effects | Notes |
| --- | --- | --- | --- |
| `services/cygnus/build.sh` | Loads `.env`, then runs `docker-compose down -v` and `docker-compose up --build -d`. | Rebuilds and starts Cygnus Docker services, deleting volumes first. | Requires `.env`. Uses legacy `docker-compose`. |
| `services/cygnus/sync.sh` | Syncs Cygnus `main` into the `deploy` branch of `https://github.com/iamjpsonkar/cygnus.git`. | Fetches/pulls `main`, switches to `deploy`, merges `main`, pushes `deploy`, then switches back to `main`. | Requires clean working tree and must be run from `main`. |

## Centaurus Scripts

| Script | Purpose | Primary side effects | Notes |
| --- | --- | --- | --- |
| `services/centaurus/build_setup.sh` | Loads `.env`, forces container Postgres host/port, then rebuilds Docker Compose services. | Runs `docker-compose down -v` and `docker-compose up --build -d`. | Requires `.env`; deletes Compose volumes. |
| `services/centaurus/start_setup.sh` | Starts existing stopped Docker Compose containers. | Starts containers. | Uses `docker-compose start`. |
| `services/centaurus/pause_setup.sh` | Stops running Docker Compose containers without deleting them. | Stops containers. | Use for non-destructive pause. |
| `services/centaurus/delete_setup.sh` | Runs `docker-compose down --volumes --remove-orphans`. | Removes containers, networks, volumes, and orphans for Centaurus Compose project. | Destructive for local data volumes. |
| `services/centaurus/rebuild_setup.sh` | Runs delete then build setup. | Recreates local Centaurus Compose services. | Calls `delete_setup.sh` and `build_setup.sh`. |
| `services/centaurus/run.sh` | Creates/activates `.venv`, installs dependencies when needed, loads `.env` or defaults, configures Python env, then starts Uvicorn with reload. | Starts the local FastAPI server and may create `.venv`. | Accepts `KEY=VALUE` overrides; `-f` is stripped before override parsing. |
| `services/centaurus/test.sh` | Creates/activates `.venv`, configures test env, then runs `ci-test.sh`. | Runs test suite and writes coverage artifacts. | Accepts `KEY=VALUE` overrides; `-f` is stripped before override parsing. |
| `services/centaurus/ci-test.sh` | Runs pytest with coverage and moves reports to `/mnt/artifacts`. | Deletes `coverage/`, writes coverage reports, attempts to write `/mnt/artifacts`. | Used by CI and `test.sh`. |
| `services/centaurus/connectionTest.sh` | Creates/activates `.venv`, installs requirements, loads `local.env`/`.env`/explicit env file, then tests database connectivity. | Connects to configured Postgres or SQLite through Centaurus SQL connector. | Supports Koyeb `DATABASE_URL`; use `local.env` for ignored local secrets. |
| `services/centaurus/fix_checks.sh` | Installs formatting/linting tools, runs isort, black, autopep8, pylint, and flake8. | Modifies Python source formatting. | Pylint/flake8 failures are allowed to continue. |
| `services/centaurus/generate_and_execute_streamlit_ui.sh` | Waits for local OpenAPI YAML, generates an API client, writes a Streamlit app from the OpenAPI spec, then runs it. | Creates/overwrites `client/`, `streamlit_app.py`, and `streamlit_data.txt`; starts Streamlit. | Intended for exploratory API UI generation. |
| `services/centaurus/sync.sh` | Syncs Centaurus `main` into the `deploy` branch of `https://github.com/iamjpsonkar/centaurus.git`. | Fetches/pulls `main`, switches to `deploy`, merges `main`, pushes `deploy`, then switches back to `main`. | Requires clean working tree and must be run from `main`. |

## Operational Risks

| Risk | Scripts | Mitigation |
| --- | --- | --- |
| Destructive Docker cleanup | `docker_cleanup.sh`, `delete_setup.sh`, `rebuild_setup.sh`, `build_setup.sh` variants that call `down -v` | Confirm the target Docker context and repository before running. |
| Secret exposure in logs | Scripts that load `.env` or `local.env` | Avoid echoing secrets. Keep generated `*.log` files ignored or delete them before sharing. |
| Deploy branch mutation | `services/cygnus/sync.sh`, `services/centaurus/sync.sh` | Run only from clean `main` after commits are pushed. |
| Generated file churn | `conf_generator.sh`, `generate_and_execute_streamlit_ui.sh`, `ci-test.sh`, `fix_checks.sh` | Review generated diffs before committing. |

## Maintenance Notes

- The `-f` flag is reserved for file logging across all shell scripts.
- Scripts that accept positional arguments should continue to receive their original arguments because the logging bootstrap removes `-f` before the script-specific logic runs.
- The log filename is based on the script basename, so duplicate script names in different directories write to separate files when run from their own directories.
- New scripts should copy the same `YOURBAZAR_LOG_BOOTSTRAP` block near the top of the file.
