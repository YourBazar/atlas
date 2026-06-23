# Cygnus Marketplace Frontend API Gaps

## Summary

Cygnus now presents a role-aware marketplace frontend for investor, partner, and provider paths. Role persistence is backed by Centaurus when an authenticated session has token details. Marketplace cards and dashboard counts are still marked as previews until Centaurus exposes richer opportunity filtering and role-specific action APIs.

## Needed By

- `services/cygnus/src/component/RoleOnboarding.js`
- `services/cygnus/src/component/RoleDashboard.js`
- `services/cygnus/src/data/marketplace.js`
- Routes:
  - `/investor`
  - `/investor/join`
  - `/partner`
  - `/partner/join`
  - `/provider`
  - `/provider/join`

## Current Frontend Behavior

Cygnus stores selected role and onboarding status in browser local storage for session hydration:

- `marketplaceRole`
- `onboardingStatus`

When `accessToken` and `userId` are available, `services/cygnus/src/component/RoleOnboarding.js` calls `PATCH /users/me/role` and stores the returned `role` and `onboarding_status`.

Dashboard opportunity cards are still sample marketplace previews, not live production data.

## Missing Centaurus Capability

Centaurus now supports:

1. Current signed-in user/session profile through `GET /users/me`.
2. Marketplace role assignment and retrieval through `PATCH /users/me/role`.
3. Role onboarding status via user `meta`.
4. Basic business-backed opportunity listing and detail through `GET /opportunities` and `GET /opportunities/{opportunity_identifier}`.

Remaining Centaurus capability gaps:

1. Opportunity filtering by role, category, city/location, status, and investment range.
2. Investor save/interest action.
3. Partner project/application action.
4. Provider profile/service request action.
5. Richer dashboard counters and activity data.

## Implemented Endpoints

### Current User

```http
GET /users/me
```

Expected response:

```json
{
  "id": "user-id",
  "email": "user@example.com",
  "first_name": "Jay",
  "role": "investor",
  "onboarding_status": "complete"
}
```

### Role Assignment

```http
PATCH /users/me/role
```

Request:

```json
{
  "role": "investor"
}
```

Allowed role values:

- `investor`
- `partner`
- `provider`

Expected response:

```json
{
  "id": "user-id",
  "role": "investor",
  "onboarding_status": "complete"
}
```

### Marketplace Opportunities

```http
GET /opportunities
GET /opportunities/{opportunity_id}
```

Current state:

- basic list/detail exists and is backed by active `Businesses`,
- richer filters are still pending.

Suggested filters:

- `role`
- `category`
- `city`
- `status`
- `investment_range`

Suggested response fields:

```json
{
  "id": "opportunity-id",
  "title": "Neighborhood grocery micro-market",
  "category": "Retail essentials",
  "location": "Tier-2 urban cluster",
  "summary": "Compact local retail format...",
  "investment_needed": 500000,
  "expected_partner_role": "operator",
  "provider_needs": ["equipment", "staffing"],
  "status": "preview",
  "created_on": "2026-06-22T00:00:00Z"
}
```

## Validation Rules

- Reject unknown roles with `400`.
- Require auth for current user, role, and action endpoints.
- Do not allow role assignment for another user through `/users/me/role`.
- Return consistent error payloads usable by Cygnus API error rendering.

## Error Cases

- Missing/invalid token.
- Unsupported role value.
- User not found.
- Opportunity not found.
- Attempted action on closed or unavailable opportunity.

## Test Scenarios

- Signed-in user can assign each allowed role. Implemented in Centaurus tests.
- Signed-in user can retrieve role after assignment. Implemented in Centaurus tests.
- Signin returns persisted role and onboarding status. Implemented in Centaurus tests.
- Unknown role is rejected. Implemented in Centaurus tests.
- Invalid role payload checksum is rejected. Implemented in Centaurus tests.
- Cygnus authenticated onboarding calls Centaurus and stores returned role status. Implemented in Cygnus tests.
- Opportunity list supports role filtering. Pending.
- Opportunity detail returns 404 for unknown ID. Pending.

## Priority

High. Durable role persistence is now in place. Live opportunity filtering, role-specific actions, and dashboard data remain the next blockers before the product can move beyond preview state.
