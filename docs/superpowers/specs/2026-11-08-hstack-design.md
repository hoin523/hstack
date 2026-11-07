# hstack Design

## Intent

Build a reusable Codex skill for PMs who need evidence-based QA of a deployed web application. It combines pstack's repeatable verification approach with gstack-browse's Chromium interaction and screenshot capture.

## Goals

- Run both requirements-based checks and exploratory testing in one invocation.
- Turn requirements into observable scenarios with expected results.
- Capture screenshots for key states and failures, with viewport and URL context.
- Produce a Korean-friendly Markdown report with a clear coverage summary and reproducible defect records.
- Mark results as `PASS`, `FAIL`, `BLOCKED`, or `NOT TESTED`; never report overall success when coverage is incomplete or evidence is missing.
- Prefer gstack-browse for browser operations and use other browser tooling only when the user approves or the environment requires a documented fallback.
- Keep test runs sequential by default. Use parallel browser sessions only after confirming that the app and browser contexts can be isolated safely.

## Non-goals

- Replacing Playwright's automated test suite, CI, Allure, or a test-case management product.
- Automatically filing issues or sending reports to third parties.
- Executing payments, sending messages, or performing irreversible data changes.
- Claiming comprehensive coverage from an exploratory pass.

## Workflow

1. Gather the deployed URL, environment, release identifier when available, requirements, and approved test account/data.
2. Build a coverage map that separates explicit requirements from inferred exploratory checks. Surface assumptions and blockers.
3. Run requirement scenarios with expected and actual results. Then run exploratory checks across primary navigation, forms, validation, loading/empty/error states, responsive layouts, and keyboard-visible behavior where applicable.
4. Capture evidence for important states and every failure. Record viewport, URL, and scenario ID; retain artifacts in the project workspace.
5. Write a Markdown result report with an executive summary, tested scope, coverage table, defects, evidence links, blockers, and limitations.
6. Stop before external or irreversible actions, and report them as blocked unless the user explicitly authorizes a safe test workflow.

## Report contract

Each scenario records an ID, source (`requirement` or `exploratory`), priority, steps, expected result when specified, observed result, status, and evidence. Every `PASS` or `FAIL` needs a screenshot path, exact URL, and viewport unless sensitive content makes safe retention impossible; in that case record `none`, reason, URL, and viewport, and use `Inconclusive` if required evidence is insufficient. For `BLOCKED` or `NOT TESTED`, record a screenshot when safe and available; otherwise record `none` with the reason and the relevant URL or blocker. For an exploratory `PASS`, the report must say that no issue was observed only within the recorded probe and timebox; it must not imply all states passed. Avoid capturing sensitive production data. If the screen contains sensitive data, use test data or approved redaction; otherwise do not retain that screenshot and report the evidence limitation. Defects additionally record severity, reproducibility, and concise reproduction steps. Inferred expectations must be labeled as inferred. Do not stage, commit, upload, or publish reports or screenshots unless the user requests it.

Apply overall-disposition precedence in this order. `Not ready` wins if any in-scope requirement fails or a confirmed P0/P1 defect remains. Otherwise `Inconclusive` applies when explicit requirements are absent, any in-scope scenario is blocked or not tested, or required evidence is insufficient. Otherwise use `Ready with issues` when P2/P3 defects or usability findings remain. Use `Ready` only when all in-scope requirements pass with evidence, the exploratory charter is complete, and no unresolved defect remains.

Report requirement coverage as the number of in-scope source requirements whose mapped scenarios are all executed (`PASS` or `FAIL`) divided by the total in-scope source requirements. Report scenario execution coverage as (`PASS` + `FAIL`) divided by the total in-scope scenarios. `BLOCKED` and `NOT TESTED` count as uncovered. When no requirement source exists, requirement coverage is `N/A` and disposition is `Inconclusive`.

## Repository shape

- `.agents/skills/hstack/SKILL.md` provides the entry point and orchestration rules.
- `.agents/skills/hstack/references/requirements-testing.md` describes requirements-based test design and execution.
- `.agents/skills/hstack/references/exploratory-testing.md` describes bounded exploratory passes.
- `.agents/skills/hstack/references/report-template.md` defines the result report.
- `README.md` documents purpose, prerequisites, installation, and invocation.

## Approaches considered

1. A thin browser runner would be fast to build but would leave test scope and report quality to each prompt.
2. A project verification-skill generator would improve repeatability but would not be a ready-to-run release QA flow for a PM.
3. A single release-QA skill with separate requirements and exploratory passes, shared evidence handling, and a fixed report format meets both requested modes. This is the selected approach.

## Calls left to the implementer

- Use gstack-browse when its installed skill and CLI are available; otherwise stop with setup guidance rather than silently switching tools.
- Write artifacts to a timestamped `qa-reports/` directory in the active project unless the user specifies another destination.
- Do not create test accounts, change production data, or run irreversible workflows without explicit authorization.
