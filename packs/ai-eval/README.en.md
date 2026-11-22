# ai-eval

> **Alpha — principles, not implementation.** This directory is not a pack yet: it has no `pack.json`, no
> roles and no skills. It holds the rules any future implementation must respect.

> Versión en español: [README.md](README.md)

## Purpose

Certification of systems built on models: RAG and agentic applications. The question is not "does it
work?" but "can I prove, with evidence and thresholds, that it delivers what it promises?".

## Principles

1. **Non-determinism.** One run proves nothing. You measure a distribution, repeat it and declare the
   variability. A single result is an anecdote in report format.
2. **An evaluation set is not a test case.** It is a dataset with a selection rule, a version and a date.
   It is cited by identifier, like a `DATA`, and the value is never embedded in the case.
3. **Thresholds approved before running.** A threshold chosen after seeing the number is not a criterion,
   it is a rationalization.
4. **Retrieval and generation are measured separately.** Did it bring the right documents? Did it answer
   faithfully to what it brought? Mixing them hides which one fails.
5. **A model as judge requires calibration.** Without comparison against human labels and an
   inter-judge agreement metric, automated judging is an opinion in a uniform.
6. **Agentic systems are evaluated by their trajectory, not only the final answer.** Tools chosen,
   arguments, extra steps, task abandonment.
7. **Reproducibility.** Model version, prompt, temperature and dataset pinned and recorded. Without that
   there is no comparison between runs, only memories.

## What remains open

- The analysis role: likely its own (`eval-analyst`), because acceptance is not pass/fail.
- The artifacts: evaluation set, rubric, score baseline.
- The thresholds per family: RAG versus agentic.

It will be decided with the real criterion of whoever certifies, not by imitation.

## Links

- Quality bundle: <https://github.com/cedaniel200/atlas-qa>
- Engine: <https://github.com/cedaniel200/atlasctl>
