# Cygnus Marketplace Frontend API Gaps

## Summary

Cygnus now presents a role-aware marketplace frontend for investor, partner, and provider paths. Role persistence is backed by Centaurus when an authenticated session has token details. Public opportunity pages and role dashboards now prefer live Centaurus opportunity data and fall back to preview records when the API is empty or unavailable. Opportunity detail pages can submit role-specific marketplace actions to Centaurus. Dedicated investor saved, partner application, and provider lead pages now read live marketplace actions. Dashboard counts, action-rich opportunity summaries, status transitions, and activity feeds are still preview-oriented.

## Needed By

- `services/cygnus/src/component/RoleOnboarding.js`
- `services/cygnus/src/component/RoleDashboard.js`
- `services/cygnus/src/data/opportunities.js`
- `services/cygnus/src/data/marketplace.js`
- Routes:
  - `/marketplace`
  - `/opportunities`
  - `/opportunities/:opportunityId`
  - `/opportunities/:opportunityId` action panel
  - `/investor`
  - `/investor/join`
  - `/investor/saved`
  - `/partner`
  - `/partner/join`
  - `/partner/applications`
  - `/provider`
  - `/provider/join`
  - `/provider/leads`

## Current Frontend Behavior

Cygnus stores selected role and onboarding status in browser local storage for session hydration:

- `marketplaceRole`
- `onboardingStatus`

When `accessToken` and `userId` are available, `services/cygnus/src/component/RoleOnboarding.js` calls `PATCH /users/me/role` and stores the returned `role` and `onboarding_status`.

Dashboard opportunity cards call `GET /opportunities?role=<role>` and use sample marketplace previews only as fallback.

Opportunity detail pages submit role-specific actions:

- investor: `save`
- partner: `application`
- provider: `provider_response`

Dedicated action-backed workspace pages call `GET /users/me/marketplace-actions` with role and action filters:

- `/investor/saved`: `role=investor&action_type=save`
- `/partner/applications`: `role=partner&action_type=application`
- `/provider/leads`: `role=provider&action_type=provider_response`

These pages render live action status, note, role, opportunity identifier, and a link back to the opportunity detail. They show workflow guidance when the API is empty or unavailable.

## Missing Centaurus Capability

Centaurus now supports:

1. Current signed-in user/session profile through `GET /users/me`.
2. Marketplace role assignment and retrieval through `PATCH /users/me/role`.
3. Role onboarding status via user `meta`.
4. Basic business-backed opportunity listing and detail through `GET /opportunities` and `GET /opportunities/{opportunity_identifier}`.
5. Role-specific marketplace actions through `POST /opportunities/{opportunity_identifier}/actions`.
6. Current user's marketplace action list through `GET /users/me/marketplace-actions`.

Remaining Centaurus capability gaps:

1. Richer action responses that include opportunity title/category/location without extra frontend lookups.
2. Live dashboard counters and activity data.
3. Opportunity moderation/review workflows for admin.
4. Domain-specific transitions beyond initial `submitted` status.
5. Provider quote/request workflow beyond initial `provider_response`.

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
POST /opportunities/{opportunity_id}/actions
GET /users/me/marketplace-actions
```

Current state:

- basic list/detail exists and is backed by active `Businesses`,
- list filters exist for role, category, location/city, status, and investment/capital range,
- Cygnus consumes live list/detail responses and normalizes them into the frontend card shape.
- action persistence exists for `save`, `interest`, `application`, and `provider_response`.

Implemented filters:

- `role`
- `category`
- `city`
- `location`
- `status`
- `investment_range`
- `capital_range`
- `investment_min`
- `investment_max`

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
- Opportunity list supports role, category, location, status, and capital range filtering. Implemented in Centaurus tests.
- Cygnus public opportunity list renders live Centaurus opportunities and calls selected filters. Implemented in Cygnus tests.
- Cygnus opportunity detail renders live Centaurus detail with preview fallback. Implemented in Cygnus tests.
- Cygnus role dashboards call role-scoped opportunity filters. Implemented in Cygnus tests.
- Opportunity detail returns 404 for unknown ID. Implemented in Centaurus tests.
- Centaurus creates and lists marketplace actions, returns duplicate actions idempotently, and rejects unknown action types/opportunities. Implemented in Centaurus tests.
- Cygnus opportunity detail submits signed-in investor/provider actions and renders success/error states. Implemented in Cygnus tests.
- Cygnus investor saved page renders live saved actions from `GET /users/me/marketplace-actions?role=investor&action_type=save`. Implemented in Cygnus tests.
- Cygnus provider leads page falls back to workflow guidance when marketplace actions are unavailable. Implemented in Cygnus tests.

## Priority

High. Durable role persistence, live opportunity filtering, role-specific action submission, and dedicated action list pages are now in place. Dashboard counters, richer action payloads, activity feeds, moderation, and provider service request workflow states remain the next blockers before the product can move beyond preview state.
