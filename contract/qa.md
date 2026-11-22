# Quality cycle contract

> Fragment of the base contract. It is not a second `AGENTS.md`: it is merged into the project contract
> when the `qa` bundle is active. Anything not decided here is asked, never invented.
>
> Loop limits and parallelism rules are cross-cutting: they are declared here so the bundle works on its
> own today, and they move to the `core` contract once it exists.

## 1 · Purpose

This bundle covers planning, designing, automating and documenting software tests, and closing the cycle
with a verdict supported by evidence. It does not cover AI system evaluation: that is the `ai-eval` pack.

## 2 · Project context

- The stack, the system surfaces and the environments come from the project context and the active
  profile. **They are never inferred.**
- If the context does not declare a surface, an environment or a command, it is recorded as a blocker and
  asked. It is not filled with a plausible guess.
- Build, test and lint commands are the ones declared by the profile. They are not invented.

## 3 · Flow

```text
Human (inputs)
   │
   ▼
test-analyst ──► researcher (on demand: questions, facts, versions)
   │
   │ analysis package (Draft)
   ▼
reviewer (review-test-design) ──► findings
   ▲                                  │
   └────── test-analyst revises ◄─────┘   max 2 rounds
   │
   ▼
[CHECKPOINT 1] Human approves the package
   │
   ▼
test-automation ──► skills (performance, interaction model, tooling)
   │
   │ tests + runs + evidence
   ▼
reviewer (review-test-automation) ──► findings
   ▲                                     │
   └──── test-automation fixes ◄─────────┘   max 2 rounds
   │
   ▼
[CHECKPOINT 2] Human validates results and evidence
   │
   ▼
doc-writer (docs-quality)
```

1. **Human input:** requirements, stories, acceptance criteria, contracts, diagrams, access and
   constraints. No agent fills those gaps on its own.
2. The `test-analyst` reviews inputs, asks questions and designs. It does not automate and does not
   document.
3. The `researcher` is the only agent allowed to answer external facts (§4).
4. The `reviewer` verifies the package before it reaches the human (§5). The human sees a package that
   already survived a peer review.
5. **Checkpoint 1:** the human approves the package. Until then the state is `Draft` and `test-automation`
   consumes nothing.
6. `test-automation` consumes only `Approved` cases and returns tests, runs and evidence.
7. The `reviewer` verifies the deliverable before the human. **Checkpoint 2:** the human validates it.
8. The `doc-writer` consolidates approved material into the official documentation.
9. A requirement change forces a review of affected cases, automation and documents; the flow resumes at
   step 4.

**Roles that never write in this flow:** `researcher` and `reviewer`. They produce reports, not artifacts
of the product.

## 4 · Research: when to use it and when not

The `test-analyst` invokes the `researcher` at its own discretion, with these rules.

### The ownership test

Two questions, in this order:

1. **Is the answer outside the project and citable?** A tool behaviour, a version, a limit, a standard, an
   API contract. That is research.
2. **Is the answer owned by the business?** A rule, a priority, a policy, what the product is supposed to
   do for its users. That is **never** research: it is a question to the human. The researcher does not
   infer it, does not search for what the client probably wants and does not dress a guess as a finding.

If neither applies, the question does not need an agent: it is read from the repository.

### It is required

| Trigger | Example |
|---|---|
| The design depends on external behaviour the project does not document | Does the tool retry on timeout? What does this version support? |
| Two project sources contradict each other | The requirement says one limit, the contract another |
| A requirement cites a limit, quota, version or compatibility that nobody verified | "50 requests per minute" |
| The analyst is about to hedge | "usually", "should work", "by default" |
| Applicability of a technique is assumed but not established | Whether a pattern fits this system |

### It is not justified

- Facts already in the project contract, context, code or configuration.
- Decisions already taken: research is not a way to reopen them.
- **Business knowledge:** rules, priorities, policies and expected product behaviour. The human provides
  it; the researcher never replaces them.
- Anything that would not change a case, a threshold, a technique or a verdict.
- Curiosity about the product that does not affect a design decision.

### Enforcement, not discipline

- **Every design statement about external behaviour cites a source or an experiment, or is marked as an
  assumption.** The `reviewer` blocks it otherwise. That is what prevents "never used".
- **The output is bounded:** one pass per question set, at most ten findings, one line if there are no open
  questions. That is what prevents "used for everything".
- The human may invoke `researcher` at any moment. The analyst cannot refuse, but the evidence bar still
  applies.

## 5 · Review: two points, one role

The `reviewer` is a shared role; this bundle specializes it with two skills. It is read-only: it writes a
report, never an artifact of the product.

| Point | Skill | What it checks | Blocks |
|---|---|---|---|
| Before checkpoint 1 | `review-test-design` | Traceability, oracle quality, data coupling, technique fit, feasibility, thresholds, unsourced claims | Human approval |
| Before checkpoint 2 | `review-test-automation` | Case mapping, preserved oracle, determinism, isolation, secrets, evidence, real commands | Human validation |

- Each report opens with a findings table: identifier, severity, state (`open`, `closed`, `accepted`).
- **A verdict is derived from the table.** With an open blocker there is no approval and no validation.
- The reviewer does not rewrite; it describes the defect and where it is.
- One pass per deliverable, not one pass per section.

## 6 · Loop limits and parallelism

### Rounds

A round is: deliver → review → revise.

| Loop | Limit | On exhaustion |
|---|---|---|
| `test-analyst` ↔ `reviewer` | 2 rounds | The human decides: adjust scope, accept with recorded open findings, or stop |
| `test-automation` ↔ `reviewer` | 2 rounds | Idem |

- **A finding that reopens is escalated, not reviewed again.** Repeating a round to see if it goes away is
  the most expensive way to not fix something.
- The human decides on exhaustion, always with the findings table in front of them.

### Parallelism

- The contract fixes two things: **readers may run in parallel, writers may not share a turn**, and the
  fan-out never exceeds the declared limit, even when the tasks look independent.
- The value of `max_parallel`, the detection of machine resources and the serialization of writes belong to
  the **engine**. The bundle declares the expectation; it does not implement it.
- Memory-heavy work — browser automation, load generation — expects a limit of one.

## 7 · Hand-off to doc-writer

- **Approval triggers consolidation.** It never depends on someone remembering to invoke it.
- Every input awaiting consolidation declares version, sources and open questions. Without that there is
  no hand-off.
- An official document marked `Consolidated` states which input version it came from. If the input
  changes, the mark disappears.
- The input describes its content, not its destination. State and version are enough.

## 8 · Artifacts and identifiers

| Type | Pattern | Assigned by |
|---|---|---|
| Requirement | `REQ-###` | `test-analyst` |
| Test case | `TC-###` | `test-analyst` |
| Reference data | `DATA-###` | `test-analyst` |
| Run | `RUN-YYYYMMDD-##` | `test-automation` |
| Finding | `BUG-###` | `test-automation` |

- Artifact state: `Draft`, `In review`, `Approved`, `Not applicable`, `Obsolete`.
  **Only the human marks `Approved`.**
- Run result: `Pass`, `Fail`, `Blocked`, `Skipped`, `Not run`.
- Every case links to at least one identifiable requirement. If no identifier exists, a provisional one is
  proposed and marked as such.
- The matrix walks requirement → data → case → run/evidence → finding whenever that data exists.

## 9 · Reference data

- The case describes behaviour and cites the data by identifier. **It never cites the value.**
- `DATA-###` declares a **selection rule** and, separately, the live values with their date.
- A concrete value is written only when the value **is the condition under test**.
- Test data is controlled and isolated. Never personal or production data without authorization.

## 10 · Evidence and verdict

- Expected result and actual result are kept apart. Without evidence there is no `Pass`.
- No report is closed with open blocking findings.
- A verdict states approval, approval with suggestions or non-approval, and it is supported by the findings
  table: with an open blocker, the verdict cannot be favourable.
- Certification or compliance is never declared just because no defects were found.

## 11 · Autonomy

| Role | May do without asking | Requires human approval |
|---|---|---|
| `test-analyst` | Read requirements, identify gaps, invoke `researcher`, propose strategy, plan, cases and matrix as drafts | Approve scope, resolve business ambiguity, change requirements |
| `researcher` | Search the project, verify external facts, write its report | Anything that changes scope or requirements |
| `reviewer` | Read deliverables, write its report, block on found blockers | Anything else: it never edits the deliverable |
| `test-automation` | Inspect the repository, propose architecture, write tests in the authorized area and run non-destructive tests in an authorized environment | Change production code, modify shared infrastructure, use real data, run destructive tests or run against production |
| `doc-writer` (skill `docs-quality`) | Read inputs and update drafts without changing meaning | Approve certifications, declare compliance, discard findings |

- If the environment permission is unclear, the agent stops and asks.
- No agent approves its own deliverables. The `reviewer` never reviews its own report.

## 12 · Operating guards

These guards are not duplicated here: they are inherited from the project contract and the engine.

- **Branch and commits:** work happens on a branch, never directly on the base; one logical change is one
  commit with a conventional prefix.
- **Secrets:** no keys, tokens, credentials or real data are versioned.
- **Verification:** no library, API, command or version is claimed from memory; it is verified in the
  current documentation or declared unverified.
- **Persistent memory:** if the project has it configured, it is read at the start and written when a
  decision is made. It complements the documents; it does not replace them.
- **Nothing is invented:** without evidence in requirements, contracts, code or documentation, no
  endpoints, selectors, commands, metrics or results are created.

## 13 · Precedence

1. The approved product specification and its acceptance criteria are the functional source of truth.
2. This contract defines the quality cycle.
3. Skills define technique; roles define procedure; this contract defines where they meet.
4. On contradiction, nothing is chosen silently: the conflict is recorded and a decision is requested.
