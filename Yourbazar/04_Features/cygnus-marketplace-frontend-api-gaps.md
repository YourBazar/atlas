# Cygnus Marketplace Frontend API Gaps

## Summary

Cygnus now presents a role-aware marketplace frontend for investor, partner, and provider paths. The UI is intentionally honest about unavailable backend capabilities: marketplace cards and dashboard counts are marked as previews until Centaurus exposes real role, opportunity, and service APIs.

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

Cygnus stores selected role and onboarding status in browser local storage:

- `marketplaceRole`
- `onboardingStatus=local-preview`

Dashboard opportunity cards are sample marketplace previews, not live production data.

## Missing Centaurus Capability

Centaurus needs durable APIs for:

1. Current signed-in user/session profile.
2. Marketplace role assignment and retrieval.
3. Role onboarding status.
4. Business/opportunity listing and detail.
5. Investor save/interest action.
6. Partner project/application action.
7. Provider profile/service request action.

## Proposed Endpoints

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

- Signed-in user can assign each allowed role.
- Signed-in user can retrieve role after assignment.
- Unknown role is rejected.
- Unauthenticated user cannot assign role.
- Opportunity list supports role filtering.
- Opportunity detail returns 404 for unknown ID.

## Priority

High. Cygnus can now provide a usable frontend shell, but durable role persistence and live opportunity data require Centaurus support before the product can move beyond preview state.
