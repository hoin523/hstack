# Verification profile maintenance

Use this workflow when the application or its verification profile may have changed. The purpose is to keep routes, features, commands, and user paths accurate for the next release.

## Reconcile sources and index

1. Read the profile, feature index, all mapped feature files, app routes/navigation/role guards, current requirements/docs, existing test files, and relevant package scripts.
2. Compare every mapped feature with its current source path, user-visible entry point, role/guard, harness command, selectors/readiness, expected observable result, evidence location, and cleanup guidance.
3. Check for new routes or feature branches, removed/renamed paths, stale links/commands, duplicate feature IDs, missing files, orphaned files, and unclear/uncovered surfaces. Update the index and route inventory.
4. Do a live browser pass through every mapped feature in the approved environment. Confirm the entry point and a representative end-to-end result; exercise required role/state only when authorized. If a safe live pass is unavailable, mark that feature `blocked` and do not call the profile clean.

## Separate profile drift from product regression

Classify each discrepancy as one of:

- `profile drift`: source or UI changed and the documented procedure/route is stale.
- `harness gap`: a deterministic command, selector, fixture, or helper is missing or no longer drives the mapped path.
- `coverage gap`: a new or changed surface has no mapped verification.
- `app regression`: the mapped user flow still represents current intended behavior, but observed app behavior contradicts it.
- `environment blocker`: access, deployment, data, or browser setup prevented a conclusion.
- `requirement ambiguity`: current expected behavior has no authoritative source.

Do not rewrite an acceptance expectation just to make a failed live result pass. Link discrepancies to source evidence and observed URL/evidence.

## Update and report

Edit only `.agents/skills/verify-<app>/` profile files and its local verification helpers when they are explicitly part of that profile. Re-drive any changed helper and confirm evidence remains usable after its cleanup. Do not modify application code, CI, deployment settings, or production data. For each change, record why it changed, source path, and live confirmation status. Keep stable IDs when the user-visible feature remains the same.

Return exactly one maintenance outcome:

- `clean`: all mapped features match current source and every mapped feature passed the required live pass; no uncovered changes were found.
- `changed`: the profile/map was updated or a confirmed app regression/coverage gap was found. Summarize edits and unresolved issues.
- `blocked`: one or more required comparisons or live passes could not be completed. Name the blocked feature and required prerequisite. Profile edits made before the blocker may still be summarized.

Never claim `clean` when any mapped feature was not checked live or a source comparison remains unresolved.
