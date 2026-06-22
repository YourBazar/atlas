# Deployment Context

## Known Deployments

Observed repository context indicates:

- Cygnus has deployment GitHub Actions.
- Centaurus and Cygnus include `sync.sh` scripts.
- The user uses personal repositories under `iamjpsonkar` with `deploy` branches for automatic deployment.

## Sync Flow

The service sync scripts are intended to:

1. Start from service repo `main`.
2. Fetch latest `origin/main`.
3. Fetch personal remote `sync/deploy`.
4. Merge `main` into local `deploy`.
5. Push to the personal `deploy` branch.
6. Return to `main`.

## Deployment Caution

Atlas should record deployment context but should not run deployment commands automatically. Any deploy-triggering branch push should be intentional.
