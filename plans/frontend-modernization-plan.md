# Frontend Modernization Plan

1. Keep current Cygnus behavior stable.
2. Fix auth response parsing if mismatch is confirmed.
3. Add tests around route guards and auth flows.
4. Move feature API calls into domain-specific helpers.
5. Plan CRA to Vite migration.
6. Add real dashboards for Investor, Partner, and Provider.

## Cygnus UI Improvement Sequence

Primary owner: Frontend UX Agent.

Supporting agents:

- Product Intelligence Agent for role-specific user intent.
- Backend Service Agent for real workflow/API availability.
- Testing QA Agent for route, form, accessibility, and responsive validation.
- Documentation Agent for Atlas updates and handoff records.

### Phase 1: Navigation and CTAs

Fix high-friction dead ends before visual redesign.

- Wire the landing page `Join Us` CTA in `services/cygnus/src/pages/app/Index.js`.
- Resolve `/community`, `/events`, and `/resources` cards in `services/cygnus/src/pages/app/Home.js`; either implement routes or remove/defer the cards.
- Wire role page `Get Started` buttons in investor, partner, and provider home pages to explicit next steps.
- Restore a useful recovery action on the 404 page.

### Phase 2: Shared Layout and Accessibility

Stabilize the app shell so future UI work does not fight global layout behavior.

- Fix the header collapse `aria-controls`/target id mismatch.
- Replace the fixed footer or reserve bottom spacing consistently across all pages.
- Add active navigation state and keyboard-visible focus behavior.
- Add accessible loading/status regions for auth and join flows.

### Phase 3: Auth and Onboarding UX

Make signup/signin and role joining feel like product workflows, not generic form posts.

- Add field-level validation states for signup and signin.
- Replace generic modal-only failure messages with inline, actionable errors.
- Add loading, success, failure, retry, and empty states.
- Replace simulated role join redirects with backend-backed onboarding once APIs are confirmed.

### Phase 4: Role Dashboard Redesign

Convert investor, partner, and provider pages from marketing copy into dashboard entry screens.

- Put role-specific next actions above the fold.
- Add role dashboard cards for portfolio/projects/services, alerts, support, and community.
- Add empty states for missing portfolio/project/service data.
- Keep marketing copy secondary to tasks and status.

### Phase 5: Visual System Cleanup

Improve UI consistency after the core workflows are safe.

- Scope broad CSS selectors that currently affect all `p`, `.btn`, and `.card` elements.
- Add mobile breakpoints for dense cards, long headings, and auth forms.
- Replace Font Awesome CDN dependency with a project-owned icon approach.
- Consolidate repeated card/section patterns into reusable components.
