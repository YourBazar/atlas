# Architecture Map

## Observed Architecture

YourBazar is currently split by repository ownership rather than by a single monorepo workspace package manager.

```text
YourBazar
  services/cygnus      React frontend
  services/centaurus   FastAPI backend
  services/ApiSpecs    API contract documents
  services/MarkDowns   product and requirement documents
  services/Dockyard    local infrastructure
  services/atlas       context and planning memory
```

## Runtime Flow

1. User opens Cygnus.
2. Cygnus routes public pages, auth pages, and role areas with React Router.
3. Signup/signin calls go to Centaurus.
4. Cygnus sends `x-payload-auth`, computed as a checksum over the request payload.
5. Centaurus validates request payload checksum and creates or authenticates the user.
6. Centaurus returns JWT-like access token data.
7. Cygnus stores auth state and identifiers in local storage.
8. Protected frontend routes are gated by local auth state.
9. Centaurus protected resource APIs validate `x-user-id` and `x-authorization`.

## Backend Layers

Centaurus follows this pattern:

```text
urls -> views/handlers -> decorators -> controllers -> connectors -> models
                    \-> serializers
```

## Frontend Layers

Cygnus follows this pattern:

```text
App -> AppRoute -> role route modules -> page components
                 -> auth pages -> ApiCaller -> BACKEND_URLS
```

## Integration Gaps

- ApiSpecs are not synchronized with current Centaurus routes.
- Cygnus auth response parsing expects `user_data`, while Centaurus serializers use `data`; this needs verification.
- Future workflow entities exist in backend models but do not yet have route/controller coverage.
