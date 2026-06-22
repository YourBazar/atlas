# Tasks

- Implement backend routes and controllers.
- Maintain serializers and response shapes.
- Coordinate migrations and model changes.
- Add or update backend tests with Testing QA.

## Active Task: Centaurus Role Onboarding API Gap

**Owner:** Backend Service Agent  
**Repository:** `services/centaurus`  
**Source signal:** Cygnus role onboarding screens at `services/cygnus/src/pages/investor/Join.js`, `services/cygnus/src/pages/partner/Join.js`, and `services/cygnus/src/pages/provider/Join.js` currently simulate onboarding and redirect after a one-second delay.

### Objective

Verify whether Centaurus has existing APIs for assigning a signed-in user to a role path (`investor`, `partner`, `provider`) and retrieving that user's role onboarding status. If the APIs are missing, design and implement the smallest safe backend surface needed by Cygnus.

### Expected API Capabilities

- Assign or confirm a role for the authenticated user.
- Return the current user's role enrollment status.
- Preserve existing auth, checksum, response wrapper, logging, tracing, and controller patterns.
- Add tests following the current Centaurus test style.

### Coordination

- Coordinate request/response shapes with the Frontend UX Agent before Cygnus replaces the simulated join flow with a real API call.
- Coordinate schema/model changes with the Data Model Agent if role membership is not already represented.
- Record any new route, model, migration, or test details in Atlas.

## Handoff

**From:** Cygnus Principal Product Engineer Agent  
**To:** Centaurus Backend Service Agent  
**Topic:** Role-aware marketplace APIs for Cygnus  
**Priority:** high

### What I found

Cygnus now has shared role onboarding and dashboard components for investor, partner, and provider paths. Because Centaurus does not yet expose confirmed user-role and marketplace opportunity APIs, Cygnus stores the selected role locally and labels opportunity data as preview content.

### Evidence

- Frontend role onboarding: `services/cygnus/src/component/RoleOnboarding.js`
- Frontend role dashboard: `services/cygnus/src/component/RoleDashboard.js`
- Frontend sample marketplace model: `services/cygnus/src/data/marketplace.js`
- Detailed API gap: `services/atlas/Yourbazar/04_Features/cygnus-marketplace-frontend-api-gaps.md`

### Frontend need

Cygnus needs durable role persistence, current-user retrieval, and live opportunity data to replace local preview behavior.

### Proposed backend change

Implement the smallest safe Centaurus API set:

- `GET /users/me`
- `PATCH /users/me/role`
- `GET /opportunities`
- `GET /opportunities/{opportunity_id}`

### Request shape

```json
{
  "role": "investor"
}
```

Allowed roles:

- `investor`
- `partner`
- `provider`

### Response shape

```json
{
  "id": "user-id",
  "email": "user@example.com",
  "first_name": "Jay",
  "role": "investor",
  "onboarding_status": "complete"
}
```

### Validation and errors

- Require auth.
- Reject unsupported roles.
- Return consistent JSON error payloads.
- Avoid leaking sensitive auth payloads in logs.

### Tests needed

- Role assignment success for all allowed roles.
- Unsupported role rejection.
- Unauthorized access rejection.
- Opportunity list and detail smoke tests.

### Suggested next step

Inspect Centaurus user model/controller patterns, then add role persistence with the least disruptive schema change.

## Handoff

**From:** Cygnus Principal Frontend Product Agent  
**To:** Centaurus Backend Service Agent  
**Topic:** Public website API gaps for contact and marketplace filtering  
**Priority:** medium

### What I found

Cygnus public website Phase 1 needs production APIs for contact submission and richer marketplace filtering. Current pages can render with honest preview states, but contact submission is not production-backed.

### Evidence

- `services/cygnus/src/pages/app/Contact.js`
- `services/cygnus/src/pages/app/Opportunities.js`
- `services/cygnus/src/pages/app/BusinessDetail.js`
- `services/atlas/Yourbazar/04_Features/cygnus-public-website-api-gaps.md`

### What is uncertain

The exact moderation, notification, and persistence model for contact requests is not defined yet.

### What I need from you

Design and implement:

- `POST /contact-requests`
- expanded `GET /opportunities` filtering
- richer public business/opportunity detail fields

### Suggested next step

Start with `POST /contact-requests` because Cygnus already has a visible contact form and currently labels submission as pending.

## Handoff

**From:** Cygnus Principal Frontend Product Agent  
**To:** Centaurus Backend Service Agent  
**Topic:** Protected workspace and admin API gaps for complete Cygnus frontend  
**Priority:** high

### What I found

Cygnus now includes protected Investor, Partner, Provider, and Admin shells. These pages are complete enough for frontend navigation and testing, but several pages are explicitly preview-backed because Centaurus does not yet expose the role-specific workflow APIs.

### Evidence

- `services/cygnus/src/component/RoleWorkspacePage.js`
- `services/cygnus/src/data/roleWorkspace.js`
- `services/cygnus/src/pages/admin/AdminPage.js`
- `services/cygnus/src/data/adminWorkspace.js`
- `services/cygnus/src/pages/auth/AuthSupportPage.js`
- `services/atlas/Yourbazar/04_Features/cygnus-public-website-api-gaps.md`

### What is uncertain

The backend authorization model for admin users is not confirmed. Cygnus currently protects `/admin/*` behind signed-in state only because role/permission claims are not yet defined.

### What I need from you

Design and implement the smallest safe API set for:

- investor saved opportunities, portfolio, activity, and profile,
- partner projects, applications, milestones, and profile,
- provider requests, services, leads, quotes, and profile,
- admin summary, users, opportunity moderation, provider moderation, and review queues,
- auth password recovery and email verification.

### Suggested next step

Start with role-specific read APIs and admin authorization claims. Cygnus can then replace preview data route by route without changing the route structure.
