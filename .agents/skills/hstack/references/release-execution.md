# Release execution

Use this workflow when checking a release with a generated project verification profile. The profile supplies app-specific launch/doctor/drive/evidence/cleanup guidance and feature routes. hstack supplies release scope, safety, browser evidence, and the PM report.

## Prepare and diagnose

1. Read the project profile and feature map. Record their verification state and last source review. If missing, use the standard hstack workflow and mark profile-guided coverage unavailable.
2. Read the installed gstack-browse instructions and complete its setup check. Run the profile's read-only, idempotent `Doctor` checks before browser interaction, login, or data changes: confirm target URL/app instance, build identity when available, required role/seed state, and browser/harness readiness. The Doctor must not log in, create/alter records, seed data, or clean up. Record failed or unknown checks as environment blockers; do not make a green doctor result stand in for product verification.
3. Identify release/build, target environment and URL, requirements, approved account/role/data, timebox, allowed side effects, and report destination. Use the project profile only for known safe setup and teardown.
4. Map each in-scope requirement to one or more feature/subfeature IDs and run scenario IDs. Link each scenario to its source requirement and feature ID where applicable. Mark requirements with no feature-map link as unmapped; do not drop them.
5. Define the release's in-scope features from the user's requirements, release scope, and requested flows. A profile feature outside this release scope is listed as out of scope and does not enter the release coverage denominator.

## Execute with two kinds of evidence

1. Run the smallest relevant existing deterministic checks, then broader release checks when in scope. Record exact commands, result, and artifacts. A passing code-level test is not evidence that the deployed UI is correct.
2. Use gstack-browse against the requested deployed URL to drive the user-facing scenarios. Confirm rendered outcomes, capture evidence for each PASS/FAIL, and check responsive layout, console/network signals where useful. Keep browser and harness evidence clearly separated.
3. Execute requirement scenarios first, followed by the bounded exploratory charter. Use one browser session at a time unless data and contexts are demonstrably isolated. Avoid duplicating a check unless comparing deployed UI against deterministic behavior answers a distinct question.
4. Use only approved accounts and disposable test data. Stop before external communication, publishing, purchases, permission changes, or irreversible deletion. Mark blocked work and its prerequisite. When a safe state-changing action is in scope, verify its result through a second read-only observation where the product allows it.
5. Before cleanup, ensure required evidence is saved. Remove only resources created by this run, using the profile's documented reversible cleanup, and verify cleanup completed. If cleanup can invalidate evidence, preserve evidence first and report cleanup status separately.

## Assess and report

Evaluate every result against the stated expected behavior and retained evidence. A screenshot proves only what is visible in that image; it does not establish hidden state or backend persistence. Treat console/network output as supporting diagnostics, not as a substitute for user-visible behavior.

Use `references/report-template.md`. Include the app profile version/source-review date, build identity, mapped and unmapped feature IDs, deterministic command results, browser run results, evidence paths, cleanup outcome, uncovered areas, and disposition. Keep reports/screenshots in `qa-reports/<run-id>/` by default and do not upload, commit, or publish them unless requested.

For profile-guided QA, every in-scope mapped feature must receive its planned live check for profile coverage to be complete. A blocked or unrun in-scope feature, or an in-scope requirement with no feature-map link, makes the disposition `Inconclusive` unless an in-scope failure or confirmed P0/P1 defect already makes it `Not ready`. This profile-coverage rule does not apply when no project profile was requested or available; ordinary requirement and exploratory coverage rules still apply.
