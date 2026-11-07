# hstack Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create a reusable Codex skill that performs requirements-based and exploratory QA on deployed web applications and writes an evidence-backed PM report.

**Architecture:** A small Agent Skills package keeps the top-level workflow in `SKILL.md`, with focused references for requirements tests, exploratory checks, and report structure. It invokes the installed gstack-browse skill for browser control and screenshot capture, without adding a second browser runtime.

**Tech Stack:** Markdown Agent Skill, gstack-browse CLI/skill, Git.

**Spec:** `docs/superpowers/specs/2026-11-08-hstack-design.md`

## Global Constraints

- Run both explicit-requirement and exploratory passes.
- Use only `PASS`, `FAIL`, `BLOCKED`, and `NOT TESTED` scenario statuses.
- Label inferred expectations as inferred.
- Save reports and evidence under timestamped `qa-reports/` in the active project unless the user specifies another path.
- Use gstack-browse as the browser backend and do not silently switch tools.
- Keep browser work sequential unless isolation is proven for safe parallel execution.
- Do not perform payment, external messaging, or irreversible data changes.
- Never claim full success when evidence or coverage is incomplete.
- Do not stage, commit, upload, or publish QA artifacts unless the user requests it.
- Report requirement and scenario execution coverage percentages; blocked and untested scenarios count as uncovered.
- For completed scenarios, attach screenshot evidence; for blocked or untested scenarios without safe evidence, record why no screenshot exists.
- Avoid retaining screenshots with sensitive production data; use approved redaction or report the evidence limitation.

## Review Focus

- Missing or ambiguous requirements can create false pass results; the requirements reference must require assumptions to be labeled and unresolved acceptance criteria to be blocked.
- Production URLs may expose real user data or trigger real effects; the entry skill must stop before irreversible actions and use only approved test identities/data.
- Screenshots can be detached from the scenario that produced them; the report schema must tie each evidence path to an ID, URL, and viewport.
- Exploratory testing can imply broad coverage from a short session; its reference must bound the pass and report its limitations.
- A missing gstack-browse installation can tempt silent fallback; the skill must stop and give setup guidance.

---

### Task 1: Create the installable hstack skill entry point

**Files:**
- Create: `.agents/skills/hstack/SKILL.md`

**Interfaces:**
- Consumes: User-provided deployment URL, requirements/spec, approved account and test data, optional release/build identifier, and optional report output path.
- Produces: A test plan, browser execution steps, evidence inventory, and report assembled with the reference files.

- [ ] Write the skill metadata and invocation description. Include purpose, prerequisites, inputs, and the rule to load all three reference files.
- [ ] Write an intake step that separates supplied facts from inferred assumptions and identifies blockers before browser actions.
- [ ] Write the orchestration sequence for requirement-based checks, exploratory checks, evidence review, and report generation.
- [ ] Add the safety rules and result-status semantics from the spec.
- [ ] Review every referenced path against the planned repository tree.

### Task 2: Define requirements-based and exploratory procedures

**Files:**
- Create: `.agents/skills/hstack/references/requirements-testing.md`
- Create: `.agents/skills/hstack/references/exploratory-testing.md`

**Interfaces:**
- Consumes: Intake output from `SKILL.md`.
- Produces: Scenario records with stable IDs, bounded steps, expected/observed results, statuses, and evidence paths.

- [ ] Define the requirement-to-scenario mapping, with traceability to the original requirement and explicit expected outcomes.
- [ ] Define handling for ambiguous, untestable, and blocked requirements.
- [ ] Define a bounded exploratory charter for navigation, forms, validation, loading/empty/error states, responsive behavior, and keyboard-visible behavior where applicable.
- [ ] Require the tester to distinguish observed defects from usability suggestions and environmental failures.
- [ ] Check both procedures against the status and safety rules in the design spec.

### Task 3: Define evidence capture and the PM report

**Files:**
- Create: `.agents/skills/hstack/references/report-template.md`

**Interfaces:**
- Consumes: Scenario records and screenshots produced by the two test procedures.
- Produces: A Markdown report with summary, scope, coverage, scenario outcomes, defect records, evidence links, blockers, and limitations.

- [ ] Define a report template with test run metadata, executive summary, and explicit requirement and scenario coverage formulas.
- [ ] Add per-scenario fields for ID, source, priority, steps, expected/observed result, status, and evidence.
- [ ] Add defect fields for severity, reproducibility, and concise reproduction steps.
- [ ] Add precedence rules for overall disposition so findings and incomplete coverage cannot yield a false-ready report.
- [ ] Add a rule that missing evidence or unfinished coverage prevents a full-pass summary.
- [ ] Check that every screenshot can be traced to a scenario and viewport.

### Task 4: Document installation and usage

**Files:**
- Create: `README.md`

**Interfaces:**
- Consumes: The completed skill and report contract.
- Produces: Instructions for installing and invoking hstack in a Codex workspace.

- [ ] Explain prerequisites, including gstack-browse availability and its setup guidance.
- [ ] Provide a sample request containing URL, requirements, test account/data, and desired report output.
- [ ] Explain where reports and screenshots are saved and how to read statuses.
- [ ] State platform limits: this repository targets Codex Agent Skills and does not claim turnkey pstack/Cursor integration.
- [ ] Review README instructions against the actual repository tree and skill paths.

### Task 5: Review the package and publish the initial repository content

**Files:**
- Review: all files created by Tasks 1-4.

**Interfaces:**
- Consumes: Complete hstack package.
- Produces: A coherent initial commit on the private `hoin523/hstack` repository.

- [ ] Read the design spec and every package file end to end. Confirm all design goals and constraints have a corresponding instruction or report field.
- [ ] Check Markdown structure, local links, command names, and that no unsupported test coverage or reporting integrations are promised.
- [ ] Commit the reviewed files with a descriptive conventional commit message.
- [ ] Push the initial commit to the newly created private repository and report its URL and file layout.
