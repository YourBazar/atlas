# Sync and Branching Context

## Branch Preference

User preference: push changes to `main`, not feature branches, unless explicitly requested otherwise.

## Commit Identity

Commits should use:

```text
Jay Prakash Sonkar <iamjpsonkar@gmail.com>
```

## Deploy Branches

Cygnus and Centaurus can be synced to personal deploy branches through their `sync.sh` scripts. These scripts are deployment-affecting and should be run only when deploy is intended.

## Submodule Update Pattern

When a submodule changes:

1. Commit and push inside the submodule.
2. Return to the root repo.
3. Commit and push the updated submodule pointer.
