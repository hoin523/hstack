# Feature map contract

The feature map is the durable index between product requirements, user navigation, verification steps, and release evidence. Keep it small enough for the agent to find the right procedure and detailed enough to replay it.

## Index: `features/README.md`

Include:

- App name and last source review date.
- Link to the generated verification profile and the source-of-truth docs/routes.
- A table with stable feature ID, feature name, user-visible entry point, roles, criticality, mapped procedure file, and verification status (`verified`, `draft`, `blocked`, or `not covered`).
- A route inventory with route, entry point, role/guard, and state or uncertainty. Mark routes not linked in the UI and explain their status.
- A list of known uncovered surfaces and the reason each remains uncovered.

Use stable IDs such as `FEAT-AUTH-001`; do not recycle an ID for a different behavior. Link each feature row to `features/<feature-id>-<short-name>.md`.

## One file per feature

Every feature file must include these exact headings:

### Sub-features

List meaningful user-visible branches or states and their stable subfeature IDs, such as `FEAT-BILLING-001.a`. Keep implementation-only details out unless they explain an observable constraint.

### How to get to it (user POV)

Give the start URL or app entry point, required role/state, and numbered user actions. Include expected URL/title or visible landmarks where useful. Include normal exits and prerequisite setup. Never include credentials or private user data.

### Driving it with <harness>

Replace `<harness>` with the actual available harness name (for example, Playwright or gstack-browse). Give an exact existing command or browser procedure, working directory, selectors/landmarks, waits/readiness condition, and how to select the feature. Describe both deterministic automation and a real browser proof when both apply. Link to code instead of copying a large test. If no harness is configured, state this and give the verified manual browser path; don't fabricate automation.

### Gotchas

Record role/feature flags, test-data requirements, nondeterministic states, known environmental constraints, side-effect boundaries, cleanup, and what the procedure cannot prove. Mark source-derived inference explicitly.

Each feature should specify at least one observable success result and relevant negative/edge states, even if a state is explicitly deferred. Map requirements and evidence IDs during each release run; keep run-specific results out of the durable feature description.

## Accuracy and maintenance

Source paths and commands are evidence for how to drive a feature, not proof that the user flow works. Confirm the user POV in the browser when an approved environment exists. Label draft or unverified procedures. Keep source quotes brief; link to their location. Do not store secrets, cookies, real customer data, or production screenshots in the profile.
