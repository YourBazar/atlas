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
