---
name: performance-testing
description: Use when a requirement depends on response time, throughput, concurrency or capacity — defines the load model, the baseline and the acceptance thresholds before any run. Trigger: "load test", "performance", "how many users does it support", "latency", "stress", "capacity".
---

# Performance testing

## Overview

Measures behaviour under a declared load and compares it against an approved threshold. Without a load
model and without an agreed threshold, a performance test produces numbers, not evidence.

## When to Use

- When a requirement fixes response time, concurrency or capacity.
- Before a release, to establish a baseline.
- When a performance regression is suspected between versions.

Do not use to measure the performance of the test suite itself or to compare tools.

## Before running

| Element | Without it |
|---|---|
| **Load model** | It is unknown what is being measured: concurrent users, arrival rate, duration, mix profile |
| **Declared environment** | Results are not comparable across runs |
| **Threshold** | There is no acceptance criterion; the number means nothing |
| **Baseline** | There is nothing to compare against |
| **Controlled data** | The result depends on what happened to be in the database |

If one is missing, the test does not run: it is recorded as a blocker.

## Conventions

1. **Approved thresholds, never invented.** They are proposed with justification — requirement, service
   agreement, previous baseline — and the human approves them before the run.
2. **One variable per run.** If load and environment change together, the result explains nothing.
3. **Measure the percentile, not the mean.** The mean hides the tail, and the tail is what users feel.
   State which percentile is used and why.
4. **Non-destructive or authorized.** Load, stress and capacity run only in the declared environment and
   with explicit authorization.
5. **Isolated, reproducible data.** Environment state is recorded before and after.
6. **Declared interference.** If other tests share the environment, note it and do not compare.
7. **One run does not declare a regression.** Repeat it and find the cause before opening a finding.
8. **The result is evidence, not a promise.** Attach conditions, configuration and artifacts.

## Output format

- **Load model:** users, rate, duration, mix and data.
- **Agreed threshold:** metric, percentile, value and who approved it.
- **Runs:** `RUN-YYYYMMDD-## | Scenario | Load | Environment | Metric | Value | Result | Evidence`.
- **Comparison** against the baseline, when one exists.
- **Findings:** `BUG-###` with the condition that reproduces them.

## Relation to the cycle

- `test-analyst` defines the load model, the threshold and the cases; without its approval there is no run.
- `test-automation` runs and delivers results with evidence.
- Performance outside the threshold is an open finding: it blocks a favourable verdict.
