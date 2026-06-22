# Frontend: Cygnus

## Stack

- React
- Create React App through `react-app-rewired`
- React Router
- Axios
- Bootstrap and React Bootstrap

## Route Structure

- Public pages: `/`, `/home`, `/about`
- Auth pages: `/signup`, `/signin`, `/signout`
- Protected role routes: `/investor/*`, `/partner/*`, `/provider/*`

## Integration Points

- `src/configs/Urls.js` centralizes backend endpoints.
- `src/helpers/ApiCaller.js` wraps Axios.
- `src/helpers/AuthContext.js` manages signed-in state.

## Risks

- Tokens are stored in local storage.
- Auth response parsing should be validated against backend response shape.
- Role pages are currently content-heavy and not workflow dashboards.
- CRA ecosystem is a long-term modernization risk.
- Public home cards currently advertise `/community`, `/events`, and `/resources`, but those routes are not registered.
- Primary CTAs are present without actions on the landing page and role pages.
- The shared footer is fixed-position and can overlap pages that do not reserve bottom spacing.
- Header mobile collapse accessibility needs id alignment.

## Current UI Improvement Plan

The current UI gap audit is recorded in `reports/CYGNUS_UI_GAP_AUDIT.md`.

Implementation should start with navigation integrity and CTA behavior before broad visual redesign. Once the clickable paths are safe, the Frontend UX Agent should improve the app shell, auth states, onboarding states, and role dashboard entry screens.
