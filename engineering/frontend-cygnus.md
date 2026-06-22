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
