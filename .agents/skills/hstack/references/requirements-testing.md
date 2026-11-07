# Requirements-based testing

Use this reference for scenarios derived from a PRD, acceptance criteria, user story, design handoff, or other explicit product requirement.

## Build the coverage map

1. Preserve the source requirement ID and wording. If the source has no IDs, assign stable IDs such as `REQ-001` and quote a short identifying phrase.
2. Split a requirement into multiple scenarios only when the user-visible outcome or precondition differs. Keep a trace from every scenario back to its source requirement.
3. Write one primary observable outcome per scenario. Include preconditions, action steps, expected result, priority, and relevant viewport or account role.
4. Keep supplied expectations separate from assumptions. Prefix an inferred expectation with `Inferred:` and include its basis.
5. Before execution, check that every in-scope requirement maps to at least one scenario. Mark an uncovered requirement `NOT TESTED` with the reason.
6. When a project feature map exists, attach its stable feature/subfeature ID to each scenario. Also record requirements with no matching feature ID as a coverage gap; the feature map does not replace the requirement source.

## Scenario record

Use the shared record shape from `report-template.md`:

| Field | Requirement-based value |
| --- | --- |
| ID | Stable `R-001` style ID |
| Source | `requirement` |
| Requirement | Original ID and short source wording |
| Priority | `P0`, `P1`, `P2`, or `P3`, based on user impact |
| Scenario and preconditions | User goal, account role, seeded data, and starting state |
| Steps | Short, reproducible UI actions |
| Expected | Supplied acceptance criterion, or explicitly labeled inference |
| Observed | What the rendered application actually did |
| Status | `PASS`, `FAIL`, `BLOCKED`, or `NOT TESTED` |
| Evidence | Screenshot path(s), URL, and viewport |
| Feature link | Project feature/subfeature ID(s), or `unmapped` |

Priorities are triage aids, not proof of severity. `P0` blocks a core release path or causes severe user/data harm. `P1` breaks an important user journey with no reasonable workaround. `P2` impairs a secondary flow or has a workaround. `P3` is a minor defect or polish issue.

## Execute and decide

- Start from the supplied URL and approved test state. Use the browser UI to perform the steps a user would take.
- Compare the visible result to the requirement's expected result. A successful click, navigation, HTTP response, or console-clean page is not enough by itself.
- Capture a screenshot for every `PASS` or `FAIL` at the point that proves the result. For a failure, include the resulting screen and enough preceding context to reproduce it. For `BLOCKED` or `NOT TESTED`, capture evidence when safe and available; otherwise record `none`, the reason, and the relevant URL or blocker.
- Record the exact URL and viewport for each capture. Do not include tokens, passwords, or private user data in screenshot names or report text.
- Avoid saving screenshots that expose sensitive production data. Use approved test data or an approved redaction method; if neither is possible, omit the image and report the evidence limitation.
- Use `PASS` only when the expected user-visible result is present and evidence supports it. Use `FAIL` when observed behavior contradicts the expectation. Use `BLOCKED` when a missing account, environment, authorization, or unsafe effect prevents the check. Use `NOT TESTED` when the check was excluded or not reached.
- When acceptance criteria conflict or leave a material outcome undefined, do not invent a pass condition. Record the ambiguity and mark the affected scenario `BLOCKED`; proceed with unrelated safe scenarios.
- Separate a reproducible product defect from an environment problem. If a failure may be caused by the deployment, network, or test account, record the evidence and label the cause unconfirmed until the same behavior is reproduced or independently isolated.
