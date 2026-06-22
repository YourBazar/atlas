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
