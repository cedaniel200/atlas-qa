---
name: review-test-design
description: Use when reviewing a test design before it is approved — checks traceability, oracle quality, data coupling, technique fit, feasibility and evidence. Trigger: "review the test design", "review the cases", "is this ready to approve?".
---

# Review of a test design

## Overview

Reviews the analysis package the way a peer would before the human approves it. It does not redesign: it
says what is broken, how severe it is and why.

## When to Use

- When `test-analyst` declares the package ready and before human approval.
- When a requirement changes and the impact on cases and matrix must be reviewed.

Do not use to review automation code or consolidated documentation.

## What is reviewed

| Dimension | Question |
|---|---|
| **Traceability** | Does every case link to a requirement, and does every requirement have at least one case or a declared reason? |
| **Oracle** | Is the expected result observable and verifiable, or does it say "works correctly"? |
| **Data** | Does the case cite the data value instead of its identifier? Is the selection rule declared? |
| **Technique** | Does the chosen technique match the type of risk, or is every technique applied by inertia? |
| **Coverage** | Are there uncovered requirements or orphan cases? Is coverage claimed without evidence? |
| **Feasibility** | Can the case be executed with the environments and data that exist? |
| **Thresholds** | Are exit criteria justified or invented numbers? |
| **Evidence** | Are facts and assumptions kept apart? Are there claims without a source? |
| **Duplication** | Is the same thing validated across layers without justification? |

## Severity

- **Blocking:** prevents approval. Missing requirement, unverifiable oracle, embedded data value,
  assumption turned into a criterion.
- **Important:** affects coverage or maintainability. Wrong technique, duplication, unjustified threshold.
- **Suggestion:** improves clarity without changing the outcome.

## Output format

Summary table, mandatory and first:

| # | Finding | Severity | State | Proposed action |
|---|---|---|---|---|

- `State`: `open`, `closed` or `accepted` (with reason).
- Close with a verdict derived from the table: with an open blocker there is no approval.
- Do not rewrite the cases: describe the defect and where it is.

## Rules

1. One full pass, not one per section.
2. Do not propose an alternative design unless the defect requires it; then, one alternative only.
3. Do not close your own finding without evidence that it was fixed.
4. If the package has no blockers, say so explicitly and hand it to the human.
