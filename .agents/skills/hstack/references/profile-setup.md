# Project verification profile setup

Create a reusable verification profile when a user asks hstack to learn an application's features or prepare repeatable QA for future releases. The generated profile belongs to the application repository; hstack's own references describe how to create and use it.

## Inspect before drafting

1. Confirm the application repository root and identify the primary web application if the repository is a monorepo. Use the app's own package/workspace scripts and docs rather than assuming a directory or framework.
2. Inspect source routes, navigation, role guards, feature entry points, existing unit/integration/e2e tests, browser harnesses, seed fixtures, environment setup, and teardown. Reuse stable existing commands and selectors where practical; do not duplicate a deterministic suite in prose.
3. Build a route and feature inventory from both user-visible navigation and source. Label the evidence source. Include reachable routes, role-specific branches, important states, and routes not discoverable through navigation. Do not infer that an unlinked source route is a supported user flow without checking it.
4. Determine whether a local or staging environment and safe test identity/data are available. Ask only for missing information that is needed for a safe proof; continue drafting independent sections while noting gaps.

## Create the profile

Create `.agents/skills/verify-<app>/SKILL.md` in the application repository, plus a `features/README.md` index and one Markdown file per discovered feature. Use a concise kebab-case app name. Keep hstack's skill files separate from the generated project profile.

The profile `SKILL.md` must give exact, repository-specific instructions under these headings:

- `Launch`: command, working directory, required environment, readiness signal, and stop/recovery steps.
- `Doctor`: checks that prove the target app, account, seeded data, and browser are ready; include safe diagnostic commands.
- `Drive`: the repository's existing deterministic command(s), and how to select focused tests or a whole release suite. Describe the gstack-browse workflow for the deployed or locally visible UI.
- `Evidence`: artifact locations, naming convention, viewport/URL capture expectations, and how to avoid retaining secrets or personal data.
- `Cleanup`: reversible cleanup steps, what evidence must be copied/confirmed first, and what must never be removed automatically.
- `Helpers`: local scripts or selectors worth reusing, with their source paths and limitations.

Also record app identity, supported environments, roles, test-data policy, known access constraints, and links to feature map and deterministic test docs. Every command and path must be verified against repository content. If something is unavailable, say `not configured` and explain how it blocks a proof; never invent a command, credential, selector, route, or successful outcome.

`Doctor` is read-only and idempotent, and runs before browser interaction. It must verify the intended app instance and target URL, available build/release identity, required account role and seed state without exposing secrets, and browser/harness readiness. It must not log in, create or alter records, seed data, or run cleanup. A prerequisite that requires mutation belongs in a separate setup step and needs explicit user authorization. A check that cannot be made safely must return `unknown` with a reason; it must not change data or turn missing configuration into a passing check.

## Prove one vertical slice

Before calling setup verified, choose one representative high-value feature that can be tested safely. Follow its mapped user path end to end in an approved local or staging environment, using the described harness and browser as appropriate. Observe the result, capture evidence, run the specified cleanup, then confirm the evidence remains readable and useful. Record the date, environment, feature, and proof outcome in the profile.

If the environment, account, or safe data is unavailable, create the inspectable draft profile and feature map, mark it `DRAFT — LIVE PROOF BLOCKED`, and list the exact prerequisite. Do not claim setup is complete or that the workflow works end to end.

## Report setup

Summarize the app and sources inspected, features/routes mapped, existing test systems reused, generated files, vertical-slice proof, uncovered/uncertain areas, and next action. Do not modify product code, CI, deployment configuration, or test data to make the profile work. Ask before installing dependencies or changing shared developer environment settings.
