# hstack

Evidence-based release QA for deployed web applications. hstack combines pstack's repeatable verification approach with gstack-browse's Chromium control and screenshot capture. It runs both acceptance-criteria checks and a bounded exploratory pass, then writes a Korean-friendly Markdown report with evidence and reproduction steps.

## What it does

- Maps each supplied acceptance criterion to observable scenarios.
- Runs a separate exploratory pass for common navigation, form, state, responsive, and keyboard-visible problems.
- Captures screenshots for important states and every failure.
- Produces a PM report with coverage, `PASS` / `FAIL` / `BLOCKED` / `NOT TESTED` results, defects, evidence, blockers, and limitations.
- Uses gstack-browse as its browser backend. It does not open a dashboard unless requested.

hstack is an instruction-only Codex skill. It does not replace Playwright's test suite, CI, Allure, or a test-management system. pstack is a source of workflow ideas; hstack does not claim turnkey pstack or Cursor integration.

## Requirements

- Codex with Agent Skills available.
- gstack-browse installed and available to the same Codex workspace or user profile.
- A deployed URL and an allowed test environment.
- Acceptance criteria or requirements for requirement-based coverage.
- An approved QA account and seeded test data for flows that require sign-in or state changes.

If gstack-browse is missing, install or enable it using the [gstack instructions](https://github.com/garrytan/gstack/tree/main/browse). hstack stops instead of silently switching browser tools.

## Install hstack

Codex discovers project skills in `.agents/skills/` and user skills in `~/.agents/skills/`. Copy the `hstack` folder into one of those locations.

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

Then ask Codex to use hstack. If it does not appear, start a new Codex session and check that the skill folder contains `SKILL.md`.

## Run a QA pass

Provide the URL, release identifier, acceptance criteria, approved test account/data, and report destination when known.

```text
Use hstack on https://staging.example.com for release 2026.11.08-rc1.
Check the attached acceptance criteria and run a bounded exploratory pass too.
Use the approved QA account and seeded test records only.
Save the Korean result report and screenshots in qa-reports/.
```

The skill records assumptions separately from supplied criteria. If access, test data, or a safe way to perform an action is missing, the affected scenario is marked `BLOCKED` or `NOT TESTED` rather than being treated as a pass. If no explicit requirement source is available, hstack can still run safe exploratory checks, but the overall disposition remains `Inconclusive`.

## Results

Each run is stored under `qa-reports/<run-id>/` in the active project by default:

```text
qa-reports/<run-id>/
├── report.md
└── screenshots/
```

The report links each screenshot to a scenario, URL, and viewport. `PASS` requires an observed expected result with evidence. `FAIL` records contradictory behavior or a supported defect. `BLOCKED` records a missing prerequisite, unsafe action, or environment problem. `NOT TESTED` records excluded or unreached work. An exploratory pass is bounded and never represents exhaustive coverage. hstack keeps reports and screenshots local and does not stage, commit, upload, or publish them automatically.

## Local source checkouts

These commands fetch upstream sources separately. The pstack directory is a sparse checkout of the `pstack/` plugin inside `cursor/plugins`.

```bash
git clone --depth 1 https://github.com/garrytan/gstack.git gstack
git clone --filter=blob:none --no-checkout https://github.com/cursor/plugins.git pstack
git clone https://github.com/hoin523/hstack.git hstack
cd pstack
git sparse-checkout init --cone
git sparse-checkout set pstack
```

Upstream references: [gstack browse](https://github.com/garrytan/gstack/tree/main/browse) and [pstack](https://github.com/cursor/plugins/tree/main/pstack).
