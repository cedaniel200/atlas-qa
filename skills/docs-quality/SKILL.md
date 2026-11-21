---
name: docs-quality
description: Use when consolidating QA artifacts into official documents — keeps test strategy, plan, cases, traceability matrix, certification report and findings log consistent, versioned and traceable to approved inputs. Trigger: "document the tests", "update the matrix", "close the report".
---

# Quality documentation

## Overview

Turns approved inputs into official, brief, consistent and traceable documentation. It does not design
tests: it consolidates what was already decided.

## When to Use

- When the design phase closes and the analysis package is `Approved`.
- When `test-automation` delivers results and evidence.
- When a requirement changes and the documentation impact must be reflected.

Do not use to design cases, set thresholds or write code.

## Documents

| Document | Content |
|---|---|
| `docs/test-strategy.md` | Objective, scope, levels, types, approach, risks, metrics, entry and exit criteria |
| `docs/test-plan.md` | Activities, roles, dependencies, resources, available schedule, risks, deliverables |
| `docs/test-cases.md` | Catalogue with state, identifier, requirement, priority, preconditions, data, steps and expected result |
| `docs/traceability-matrix.md` | Requirement → criterion → data → case → run/evidence → finding |
| `docs/certification-report.md` | Scope, version, environment, exit criteria, summary, open defects, exceptions, evidence and decision |
| `docs/findings-log.md` | Chronological log of defects, blockers, discrepancies and decisions |

Adapt names to the repository's existing structure; do not duplicate documents.

## Conventions

1. Consolidate only approved inputs. Do not fill gaps by inference.
2. If two inputs contradict each other, keep the discrepancy as pending and ask for a decision.
3. Separate artifact state (`Draft`, `In review`, `Approved`, `Not applicable`, `Obsolete`) from run result
   (`Pass`, `Fail`, `Blocked`, `Skipped`, `Not run`).
4. Keep stable identifiers and never reassign them.
5. Record provenance and version of every input used.
6. Update only affected documents; do not rewrite the whole set.
7. Mark every consolidated document with the version of the input it came from. If the input changes, the
   mark disappears.
8. Never declare certification without exit criteria and sufficient evidence.
9. Do not delete open findings or change states without evidence or explicit decision.
10. Keep facts and decision apart: the report presents results and evidence before the conclusion.

## Output format

- **Update summary:** affected documents and reason.
- **Documents:** Markdown content or changes applied to the repository.
- **Consistency control:** duplicated identifiers, broken links, requirements without cases, orphan
  cases, missing references.
- **Pending items:** conflict, affected document, impact and suggested owner.
- **Provenance:** versions and dates of the inputs.

## Brevity

- Strategy and plan: executive summary and tables; expand only relevant risks or decisions.
- Cases: steps sufficient to reproduce; do not copy story context.
- Matrix: one row per relation; no paragraphs.
- Certification: results and evidence first.
- Findings log: one entry per event; do not delete closed entries.

## Success criteria

- Every statement traces to an identifiable input or piece of evidence.
- Identifiers, states, versions and cross-references are consistent.
- Discrepancies and gaps stay visible until resolved.
- No tests were designed and no code was modified.
