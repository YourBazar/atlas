# Cygnus Public Website API Gaps

## Summary

Cygnus Phase 1 public website pages are being added for the full YourBazar website. Several pages can render with static product content, but production behavior requires backend/service work.

## Needed By

Public routes:

- `/contact`
- `/opportunities`
- `/opportunities/:opportunityId`
- `/businesses`
- `/businesses/:businessId`
- `/status`

Protected app routes:

- `/investor/opportunities`
- `/investor/opportunities/:opportunityId`
- `/investor/saved`
- `/investor/portfolio`
- `/investor/activity`
- `/investor/profile`
- `/partner/projects`
- `/partner/projects/:projectId`
- `/partner/applications`
- `/partner/milestones`
- `/partner/profile`
- `/provider/requests`
- `/provider/requests/:requestId`
- `/provider/services`
- `/provider/leads`
- `/provider/profile`
- `/admin`
- `/admin/users`
- `/admin/opportunities`
- `/admin/providers`
- `/admin/reviews`

Auth support routes:

- `/forgot-password`
- `/reset-password`
- `/verify-email`

## Backend Service Agent Tasks

### Contact Submission API

Cygnus route:

- `/contact`

Needed endpoint:

```http
POST /contact-requests
```

Suggested request:

```json
{
  "name": "Jay",
  "email": "jay@example.com",
  "topic": "Investor interest",
  "message": "I want to discuss a business opportunity."
}
```

Suggested behavior:

- validate email,
- validate message length,
- store request,
- return request identifier,
- avoid logging full message bodies in production logs.

### Marketplace Filters

Cygnus route:

- `/opportunities`

Needed endpoint expansion:

```http
GET /opportunities?category=&role=&location=&stage=&capital_range=
```

Current state:

- Centaurus exposes opportunity list/detail backed by `Businesses`.
- Cygnus Phase 1 uses curated preview filters until backend filtering is expanded.

### Business Public Detail

Cygnus route:

- `/businesses/:businessId`

Needed endpoint expansion:

```http
GET /businesses/{business_identifier}
```

Current state:

- Centaurus has business detail.
- Public business detail needs richer fields for readiness, provider needs, partner expectations, and risk checks.

### Investor Workspace APIs

Cygnus routes:

- `/investor/opportunities`
- `/investor/opportunities/:opportunityId`
- `/investor/saved`
- `/investor/portfolio`
- `/investor/activity`
- `/investor/profile`

Needed endpoints:

```http
GET /investor/opportunities
GET /investor/opportunities/{opportunity_identifier}
GET /investor/saved-opportunities
POST /investor/saved-opportunities/{opportunity_identifier}
DELETE /investor/saved-opportunities/{opportunity_identifier}
GET /investor/portfolio
GET /investor/activity
GET /investor/profile
PATCH /investor/profile
```

Suggested behavior:

- require authenticated investor context,
- support saved opportunity notes and status,
- expose portfolio ventures with milestone, capital, and risk state,
- expose audit-safe activity events,
- avoid returning sensitive financial data to the wrong role.

### Partner Workspace APIs

Cygnus routes:

- `/partner/projects`
- `/partner/projects/:projectId`
- `/partner/applications`
- `/partner/milestones`
- `/partner/profile`

Needed endpoints:

```http
GET /partner/projects
GET /partner/projects/{project_identifier}
GET /partner/applications
POST /partner/applications
GET /partner/milestones
PATCH /partner/milestones/{milestone_identifier}
GET /partner/profile
PATCH /partner/profile
```

Suggested behavior:

- require authenticated partner context,
- represent application states: draft, submitted, shortlisted, rejected, accepted,
- expose launch milestones with owner, due date, evidence, and blocker fields,
- support partner profile categories, geography, commitment level, and experience.

### Provider Workspace APIs

Cygnus routes:

- `/provider/requests`
- `/provider/requests/:requestId`
- `/provider/services`
- `/provider/leads`
- `/provider/profile`

Needed endpoints:

```http
GET /provider/requests
GET /provider/requests/{request_identifier}
POST /provider/requests/{request_identifier}/quotes
GET /provider/services
POST /provider/services
PATCH /provider/services/{service_identifier}
GET /provider/leads
PATCH /provider/leads/{lead_identifier}
GET /provider/profile
PATCH /provider/profile
```

Suggested behavior:

- require authenticated provider context,
- expose request scope, owner, location, delivery window, and quote status,
- represent service catalog, coverage, capacity, and response target,
- support lead states: new, opened, quoted, accepted, declined.

### Admin Moderation APIs

Cygnus routes:

- `/admin`
- `/admin/users`
- `/admin/opportunities`
- `/admin/providers`
- `/admin/reviews`

Needed endpoints:

```http
GET /admin/summary
GET /admin/users
GET /admin/opportunities
PATCH /admin/opportunities/{opportunity_identifier}/status
GET /admin/providers
PATCH /admin/providers/{provider_identifier}/status
GET /admin/reviews
PATCH /admin/reviews/{review_identifier}
```

Suggested behavior:

- require authenticated admin authorization, not only signed-in state,
- expose moderation state without leaking credentials or private auth payloads,
- record review decisions with actor, timestamp, reason, and previous state,
- support public listing states: draft, review, public, paused, archived.

### Auth Recovery And Verification APIs

Cygnus routes:

- `/forgot-password`
- `/reset-password`
- `/verify-email`

Needed endpoints:

```http
POST /auth/password-reset-requests
POST /auth/password-resets
POST /auth/email-verifications
```

Suggested behavior:

- generate expiring signed tokens,
- throttle recovery attempts,
- never reveal whether an email exists,
- invalidate used tokens,
- expose user verification state through `GET /users/me`.

## Orion Agent Task

Cygnus route:

- `/status`

Need:

- public-safe service health summary from Orion or Centaurus-mediated status endpoint.

Suggested endpoint:

```http
GET /platform/status
```

Response should avoid exposing internal secrets, hosts, or credentials.

## Nebula Agent Task

Cygnus routes:

- `/opportunities`
- `/businesses`
- `/providers`

Need:

- generated business/opportunity/provider imagery through a secure backend-mediated path.

Suggested flow:

```text
Cygnus -> Centaurus -> Nebula -> storage provider
```

## Priority

Medium-high. Public pages can launch with honest preview states, but production contact, marketplace filtering, public status, and generated media need service support.
