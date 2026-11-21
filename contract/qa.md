# Quality cycle contract

> Fragment of the base contract. It is not a second `AGENTS.md`: it is merged into the project contract
> when the `qa` bundle is active. Anything not decided here is asked, never invented.

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
Human (inputs) → test-analyst → [APPROVAL] → test-automation → [VALIDATION] → doc-writer
```

1. **Human input:** requirements, stories, acceptance criteria, contracts, diagrams, access and
   constraints. No agent fills those gaps on its own.
2. The `test-analyst` reviews the inputs, asks questions and designs. It does not automate and does not
   document.
3. **Mandatory checkpoint:** the human approves the analysis package. Without that approval the flow
   stops; `test-automation` starts nothing.
4. `test-automation` consumes only `Approved` cases and returns results with evidence.
5. **Mandatory checkpoint:** the human validates results and evidence before consolidation.
6. The `doc-writer` consolidates approved material into the official documentation.
7. If information is missing, the agent records the blocker and asks concrete questions.
8. A requirement change forces a review of affected cases, automation and documents; the flow resumes at
   step 3.

## 4 · Hand-off to doc-writer

- **Approval triggers consolidation.** It never depends on someone remembering to invoke it.
- Every input awaiting consolidation declares version, sources and open questions. Without that there is
  no hand-off.
- An official document marked `Consolidated` states which input version it came from. If the input
  changes, the mark disappears.
- The input describes its content, not its destination. State and version are enough.

## 5 · Artifacts and identifiers

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
- Every case links to at least one identifiable requirement. If no identifier exists, a provisional one
  is proposed and marked as such.
- The matrix walks requirement → data → case → run/evidence → finding whenever that data exists.

## 6 · Reference data

- The case describes behaviour and cites the data by identifier. **It never cites the value.**
- `DATA-###` declares a **selection rule** and, separately, the live values with their date.
- A concrete value is written only when the value **is the condition under test**.
- Test data is controlled and isolated. Never personal or production data without authorization.

## 7 · Evidence and verdict

- Expected result and actual result are kept apart. Without evidence there is no `Pass`.
- No report is closed with open blocking findings.
- A verdict states approval, approval with suggestions or non-approval, and it is supported by the
  findings table: with an open blocker, the verdict cannot be favourable.
- Certification or compliance is never declared just because no defects were found.

## 8 · Autonomy

| Role | May do without asking | Requires human approval |
|---|---|---|
| `test-analyst` | Read requirements, identify gaps, propose strategy, plan, cases and matrix as drafts | Approve scope, resolve business ambiguity, change requirements |
| `test-automation` | Inspect the repository, propose architecture, write tests in the authorized area and run non-destructive tests in an authorized environment | Change production code, modify shared infrastructure, use real data, run destructive tests or run against production |
| `doc-writer` (skill `docs-quality`) | Read inputs and update drafts without changing meaning | Approve certifications, declare compliance, discard findings |

- If the environment permission is unclear, the agent stops and asks.
- No agent approves its own deliverables.

## 9 · Operating guards

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

## 10 · Precedence

1. The approved product specification and its acceptance criteria are the functional source of truth.
2. This contract defines the quality cycle.
3. Skills define technique; roles define procedure.
4. On contradiction, nothing is chosen silently: the conflict is recorded and a decision is requested.
