# atlas-qa

> **Alpha.** Bundle of the ATLAS suite for the quality cycle. The `atlasctl` engine lands in 0.6.0 and
> bundle wiring in 0.6.1. Nothing here is installable yet.

This repository is **content**, not a product: roles, skills and a contract fragment that the engine
applies over a project. It is not cloned in order to be used.

## Structure

```text
bundle.json          bundle metadata: id, dependencies, roles and skills
contract/qa.md       fragment of the base contract (not an AGENTS.md of its own)
roles/               procedure for each role exclusive to this bundle
skills/              techniques, loaded by whichever role needs them
packs/               optional specializations (ai-eval, pending)
```

`bundle.json` declares `requires: ["core"]`. Shared roles (`analyst`, `researcher`, `reviewer`,
`doc-writer`) come from the `core` bundle and this bundle **specializes them with skills** instead of
redefining them. One copy of each role, no drift between products.

## What is a role, a skill, a pack or a profile

The classification is not cosmetic: each one has a different failure mode.

| Kind | Test | Failure mode if misused |
|---|---|---|
| **Role** | Own mandate, own stopping points, own write limits | Two roles with the same mandate drift apart |
| **Skill** | Same mandate, different checklist | Explodes into roles that contradict each other |
| **Pack** | Different artifacts or acceptance model, same gates | A bundle that is really a technique |
| **Profile** | Stack, language, commands, paths | The content stops being agnostic |

Applied to this bundle:

| Piece | Kind | Why |
|---|---|---|
| `test-analyst` | role | Designs, stops for approval, never automates |
| `test-automation` | role | Writes code, runs, never designs |
| design review | skill of `reviewer` | Same mandate as code review, different checklist |
| automation review | skill of `reviewer` | Idem |
| `researcher` | role from `core` + `quality-research` | The role already exists; the technique is local |
| `doc-writer` | role from `core` + `docs-quality` | Idem |
| `performance-testing` | skill | A technique: no own flow, no own gates |
| `ai-eval` (RAG, agents) | pack | Different artifacts and acceptance model, same gates |

## Roles

| Role | Prompt | Responsibility | Writes to |
|---|---|---|---|
| `test-analyst` | `roles/test-analyst.md` | Strategy, plan, cases, data and traceability | `artifacts/analysis/` |
| `test-automation` | `roles/test-automation.md` | Automatability, architecture and test code | the project's test area |

## Skill Registry

> Triggers can be expressed in any language; each skill declares examples in its `description`.

| Skill | Path | Trigger |
|---|---|---|
| `docs-quality` | `skills/docs-quality/` | Consolidate inputs into official documentation |
| `review-test-design` | `skills/review-test-design/` | Review a test design before approval |
| `review-test-automation` | `skills/review-test-automation/` | Review automation before validation |
| `quality-research` | `skills/quality-research/` | Verify an external fact that changes the design |
| `performance-testing` | `skills/performance-testing/` | Latency, concurrency or capacity requirement |

## Cycle

```text
Human → test-analyst → [approval] → test-automation → [validation] → doc-writer
```

Two human checkpoints. Details in `contract/qa.md`.

## Pending for 0.6.1

- **`ai-eval` pack**: certification of RAG and agentic systems. Same flow, different techniques:
  evaluation set, rubrics, score baselines and statistical thresholds. It needs its own analyst role
  because the acceptance model is not pass/fail.
- **Templates**: plan, cases, matrix and report, linked from the skills.
- **`core` bundle**: this bundle depends on it and it does not exist yet.

## Links

- Engine: <https://github.com/cedaniel200/atlasctl>
- Development bundle: <https://github.com/cedaniel200/atlas-dev>
