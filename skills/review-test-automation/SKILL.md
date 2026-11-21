---
name: review-test-automation
description: Use when an automation deliverable is about to be validated — checks that every test maps to an approved case, that oracles were preserved, and that execution is deterministic, isolated and evidenced. Trigger: "revisa la automatización", "revisa los tests", "¿esto está listo para validar?".
---

# Review of a test automation deliverable

## Overview

Reviews the automation the way an independent peer would before the human validates it. It does not
rewrite tests: it says what is wrong, how severe it is and why.

## When to Use

- When `test-automation` delivers code, results and evidence.
- When a test starts failing intermittently and the cause is not clear yet.

Do not use to review the test design (that is `review-test-design`) or the official documentation.

## What is reviewed

| Dimension | Question |
|---|---|
| **Traceability** | Does every test map to an approved `TC-###` and keep its identifier? |
| **Oracle** | Was the expected result preserved, or was it relaxed to make the test pass? |
| **Scope** | Did the change stay inside the authorized test area and configuration? |
| **Determinism** | Can it run twice with the same result? Does it depend on execution order? |
| **Synchronization** | Are there fixed waits used as synchronization instead of observable conditions? |
| **Locators** | Are selectors stable and accessible, or tied to layout structure? |
| **Isolation** | Does each test own its data and clean up after itself? |
| **Secrets** | Any credential, token or real data in code, fixtures, logs or reports? |
| **Evidence** | Does every result carry a run identifier, environment, command and artifact? |
| **Honesty** | Is a `Fail` reported as such, or turned into a `Pass` because the script exited cleanly? |
| **Commands** | Do the documented commands and variables actually exist? |

## Severity

- **Blocking:** cannot be validated. Missing case link, changed oracle, production code touched, secret
  committed, result reported without evidence.
- **Important:** affects reliability or maintenance. Fixed waits, fragile locators, order dependency,
  shared test data.
- **Suggestion:** improves clarity without changing the outcome.

## Output format

Summary table, mandatory and first:

| # | Finding | Severity | State | Proposed action |
|---|---|---|---|---|

- `State`: `open`, `closed` or `accepted` (with reason).
- Close with a verdict derived from the table: with an open blocker there is no validation.
- For each failure, separate product failure, test failure, environment problem and inconclusive result.

## Rules

1. One full pass, not one per file.
2. Do not propose a rewrite unless the defect requires it; then, one alternative only.
3. Do not close your own finding without evidence that it was fixed.
4. If there are no blockers, say so explicitly and hand it to the human.
