# CI/CD

## Existing CI

- Root security audit workflow.
- Cygnus build/test/deploy/security workflows.
- Centaurus pytest/pylint/flake8/docker/security workflows.
- Secret scan workflows in non-runtime submodules.

## Recent Fixes

- GitHub Actions were updated away from Node 20 action runtime warnings in Cygnus.
- gitleaks runs through Docker to avoid missing binary failures.
- Cygnus production audit passes locally.

## Recommendation

Use CI failures as task inputs for Atlas `plans/active-plan.md`.
