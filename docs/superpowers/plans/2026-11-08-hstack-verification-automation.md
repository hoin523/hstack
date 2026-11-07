# hstack Verification Automation Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Extend hstack into a reusable web verification system that generates project-specific verification profiles, runs release QA with gstack-browse evidence, and maintains the profile as the app changes.

**Architecture:** Keep hstack as one Codex skill with three routed workflows: profile setup, release QA, and profile maintenance. Setup writes an app-local `verify-<app>` skill and complete feature map; release runs reuse that contract and combine existing deterministic harnesses with gstack-browse; maintenance reconciles source and live behavior without editing product code.

**Tech Stack:** Codex Agent Skill, Markdown references/templates, existing project harnesses, gstack-browse.

**Spec:** `docs/superpowers/specs/2026-11-08-hstack-verification-automation-design.md`

## Global Constraints

- Keep requirement-based and exploratory coverage distinct.
- Use existing app harnesses before generating helpers; use gstack-browse for deployed visual/browser checks.
- A generated profile has exact `Launch`, `Doctor`, `Drive`, `Evidence`, `Cleanup`, and `Helpers` sections.
- A feature map indexes all discoverable user-facing routes and flows; uncovered or unreachable features remain explicit.
- Prove a generated profile on one feature in a safe local/test environment before calling it ready.
- Never edit application behavior during profile setup or maintenance.
- Keep evidence after cleanup; do not stage, commit, upload, or publish QA artifacts unless requested.
- Run sequentially unless ports, browser contexts, and data isolation are proven.
- Do not run payments, external messages, publication, permission changes, or irreversible deletion as ordinary QA.
- Keep overall disposition precedence: `Not ready`, `Inconclusive`, `Ready with issues`, `Ready`.
- Automated tests and skill evals are not in scope for this implementation unless the user asks to run them.

## Review Focus

- A repo with several app surfaces may produce the wrong verification profile; setup must select and name the primary web app and report other surfaces separately.
- Missing local environment configuration may make the app impossible to start; setup must distinguish an app defect from a missing prerequisite and never invent a ready check.
- Existing Playwright/Cypress tests and gstack-browse may exercise different layers; release QA must preserve both forms of evidence and avoid claiming one proves the other.
- A stale feature map may hide newly added routes; maintenance must compare source inventory with the map and report uncertain route ownership.
- Parallel sessions may share accounts, ports, profiles, or server data; default serial execution unless isolation has observable proof.

---

### Task 1: Route hstack through setup, release, and maintenance

**Files:**
- Modify: `.agents/skills/hstack/SKILL.md`

**Interfaces:**
- Consumes: User intent and the current repository/target URL.
- Produces: A selected workflow and required context for the appropriate reference.

- [x] Update the description to trigger on app-specific verification setup, deployed release QA, and verification profile maintenance.
- [x] Add a workflow router that distinguishes setup, run, and maintain requests and loads only the references required for that route.
- [x] Make the run route prefer an existing project profile, then supplied acceptance criteria, then bounded exploratory checks.
- [x] Preserve existing safety, status, evidence, and report rules.
- [x] Review all referenced paths against the package tree after Tasks 2-4.

### Task 2: Generate a project-local verification profile and feature map

**Files:**
- Create: `.agents/skills/hstack/references/profile-setup.md`
- Create: `.agents/skills/hstack/references/feature-map-contract.md`

**Interfaces:**
- Consumes: Application repository, existing docs and harnesses, optional deployment URL.
- Produces: `<app-repo>/.agents/skills/verify-<app>/SKILL.md`, `features/README.md`, and one feature file per discoverable user-facing web flow.

- [x] Define repository discovery for app surface, exact launch command, ready signal, seed data, auth, existing harness, screenshot capability, and safe cleanup.
- [x] Define a read-only `Doctor` contract that checks the instance, build/release identity, target URL, and required test state before browser actions.
- [x] Define exact generated-skill sections: `Launch`, `Doctor`, `Drive`, `Evidence`, `Cleanup`, and `Helpers`; leave no placeholders.
- [x] Define the feature-map index and the four required feature headings: `Sub-features`, `How to get to it (user POV)`, `Driving it with <harness>`, and `Gotchas`.
- [x] Require the full route/flow inventory from source; use explicit uncovered entries when behavior cannot yet be driven.
- [x] Define one safe end-to-end proof of the generated profile and evidence-survives-cleanup check before declaring it ready.

### Task 3: Run release QA using the profile and gstack-browse

**Files:**
- Create: `.agents/skills/hstack/references/release-execution.md`
- Modify: `.agents/skills/hstack/references/requirements-testing.md`
- Modify: `.agents/skills/hstack/references/exploratory-testing.md`
- Modify: `.agents/skills/hstack/references/report-template.md`

**Interfaces:**
- Consumes: Verified project profile, supplied requirements, deployment identity, approved test identity/data.
- Produces: Requirement scenarios, feature-map coverage, bounded exploratory scenarios, durable evidence, and a PM report.

- [x] Define the order: doctor, deterministic harness checks, browser user-path checks, exploratory charter, evidence review, cleanup, report.
- [x] Use existing project tests for deterministic logic and gstack-browse for deployed UI behavior, responsive screenshots, console/network evidence, and exploratory checks.
- [x] Map every result to both a requirement ID and feature ID where applicable; record uncovered entry points as uncovered.
- [x] Require action/result evidence for each completed scenario and verify side effects through a second read-only observation when safe.
- [x] Ensure cleanup removes only run-created resources and leaves evidence intact.
- [x] Extend the PM report with profile identity/version, feature coverage, harness-vs-browser evidence, environment/build identity, and cleanup result.

### Task 4: Maintain profile drift without hiding product regressions

**Files:**
- Create: `.agents/skills/hstack/references/profile-maintenance.md`

**Interfaces:**
- Consumes: Existing app-local verification profile, current source/routes, and current runnable app.
- Produces: One outcome: `clean`, `changed`, or `blocked`; only verification profile/helper changes are eligible for edits.

- [x] Define feature-map index hygiene and source comparison for every feature.
- [x] Define a live serial pass through every mapped feature using doctor and the profile's own launch/drive/cleanup procedures.
- [x] Triage mismatch as profile drift, harness gap, product regression, or external prerequisite.
- [x] Allow edits only inside the app-local verification profile and its owned helpers; report product regressions without changing product code.
- [x] Require re-driving any changed helper and evidence to survive teardown before reporting `changed`.

### Task 5: Document commands and platform boundaries

**Files:**
- Modify: `README.md`

**Interfaces:**
- Consumes: Completed skill contract and generated-profile format.
- Produces: Setup, run, and maintain usage instructions for Codex users.

- [x] Document setup, release QA, and maintenance prompts with one realistic example for each.
- [x] Explain where the app-local profile and QA report/evidence are written.
- [x] State that hstack is a Codex Agent Skill using the installed gstack-browse skill and existing app harnesses, not a new browser runtime or test-management product.
- [x] Clarify that reports are not committed or uploaded automatically.

### Task 6: Review, publish, and set repository visibility

**Files:**
- Review: all files changed by Tasks 1-5.

**Interfaces:**
- Consumes: Updated hstack skill package.
- Produces: A reviewed update to `hoin523/hstack` and the user-authorized public repository visibility.

- [x] Review the package against this spec, status/evidence contracts, and gstack-browse/pstack source behavior.
- [x] Check local links, file references, markdown structure, and git whitespace without running automated tests.
- [ ] Commit and push the updated skill to `main`.
- [ ] Change repository visibility to public after the tracked files and diff are confirmed not to contain secrets or private user data.
- [ ] Verify the pushed skill path and public repository visibility.
