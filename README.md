# hstack

PM-oriented web release QA that builds reusable verification procedures, checks deployed user flows with browser evidence, and keeps the procedures current as the application changes.

hstack combines pstack's repeatable verification-profile and feature-map approach with gstack-browse's Chromium control and screenshot capture. It keeps deterministic application tests in their existing harness, adds a real browser pass for visible behavior, and produces a stakeholder-ready Markdown report.

## Three workflows

1. **Set up a project profile:** inspect the application routes, roles, existing tests and safe test data; create `.agents/skills/verify-<app>/` with launch/doctor/drive/evidence/cleanup instructions and an indexed feature map. The profile is marked verified only after a safe end-to-end feature pass and evidence-retention check. If prerequisites are missing, hstack leaves a clear draft and blocker.
2. **Run release QA:** connect requirements to stable feature IDs, run the relevant existing deterministic checks, then verify the deployed user experience with gstack-browse. Capture screenshots for scenario outcomes, do a bounded exploratory pass, clean up approved disposable data, and write an evidence-linked report.
3. **Maintain the profile:** compare every mapped feature against current routes, source, and tests; live-check every mapped feature; update only verification-profile files; report `clean`, `changed`, or `blocked`.

## Install hstack

hstack itself is an instruction-only Codex skill. Install the `.agents/skills/hstack/` folder in a project or user skills directory. Generated application profiles are separate and live inside the application repository.

For one project:

```bash
mkdir -p /path/to/your-project/.agents/skills
cp -R .agents/skills/hstack /path/to/your-project/.agents/skills/hstack
```

For all your projects:

```bash
mkdir -p ~/.agents/skills
cp -R .agents/skills/hstack ~/.agents/skills/hstack
```

Codex discovers project skills in `.agents/skills/` and user skills in `~/.agents/skills/`. If hstack does not appear, start a new Codex session and confirm `SKILL.md` exists in the copied folder.

## Example requests

### Create a reusable verification profile

```text
Use hstack to prepare a reusable verification profile for this app.
Map the user-visible features and roles, reuse the existing test harness,
and prove one safe end-to-end feature in staging. Do not change product code.
```

### Check a release

```text
Use hstack on https://staging.example.com for release 2026.11.08-rc1.
Check the attached acceptance criteria, run the relevant existing tests,
and do a bounded exploratory pass with screenshots. Use only the approved
QA account and seeded records. Save a Korean report in qa-reports/.
```

### Maintain a profile after changes

```text
Use hstack to maintain this app's verification profile after the latest changes.
Compare every mapped feature with current routes and tests, live-check each
feature in staging, and update only the verification profile. Report clean,
changed, or blocked.
```

## Browser and evidence

Live web QA uses gstack-browse and its `browse` CLI. hstack reads the installed browse instructions and setup before opening the target; it does not silently switch browser tools. A deployed browser pass is separate evidence from a passing Playwright or other deterministic test. Screenshots, URL, viewport, action, and scenario ID are linked in each report result. Sensitive production content is not retained.

Each release report is stored under `qa-reports/<run-id>/` by default:

```text
qa-reports/<run-id>/
├── report.md
└── screenshots/
```

Reports and screenshots stay local and are not staged, committed, uploaded, or published automatically. The release disposition is `Not ready`, `Inconclusive`, `Ready with issues`, or `Ready`, based on in-scope requirements, blockers, retained evidence, and unresolved defects. Exploratory QA is bounded; it does not claim exhaustive coverage.

## Requirements and limits

- Codex with Agent Skills available.
- gstack-browse installed and available to the same workspace or user profile for live browser QA.
- For release QA: a deployed URL and allowed environment. Sign-in or state-changing flows also need an approved test identity and disposable data.
- For requirement coverage: acceptance criteria or another explicit source of truth. Without it, safe exploratory checks can run, but the overall disposition is `Inconclusive`.
- hstack does not replace the application's test suite, CI, Allure, or a test-management system. It orchestrates and connects their evidence with user-visible browser checks.

If gstack-browse is missing, install or enable it using the [gstack instructions](https://github.com/garrytan/gstack/tree/main/browse). hstack stops rather than silently changing browser tools.

## Source repositories

Upstream workflow references: [gstack browse](https://github.com/garrytan/gstack/tree/main/browse) and [pstack](https://github.com/cursor/plugins/tree/main/pstack).
