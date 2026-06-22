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
