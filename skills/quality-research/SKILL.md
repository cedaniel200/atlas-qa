---
name: quality-research
description: Use before designing or approving tests when a claim depends on an external fact — tool behaviour, API limits, standard versions, framework features. Trigger: "does it support this?", "how does X work?", "look up how this is done", "check the version".
---

# Quality research

## Overview

Answers with sources what the design takes for granted. A claim that changes the design without a source
is an assumption, and an assumption is marked as such.

## When to Use

- Before fixing a strategy or a technique that depends on how a tool behaves.
- Before promising coverage, limits or version compatibility.
- When two project documents contradict each other.

Do not use for facts about the repository itself: those are read from the code and the contract.

## Evidence bar

| Level | What counts | What does not |
|---|---|---|
| **Verified** | Official documentation of the version in use, or a reproducible experiment in the project | A blog post, a forum answer, model memory |
| **Assumption** | Reasonable but unproven | Presenting it as fact |
| **Blocker** | It cannot be verified | Filling the gap with something plausible |

Every claim that changes the design cites a source or an experiment. If it cannot, it is marked as an
assumption and what would confirm it is stated.

## Procedure

1. **State the question** in one sentence, limited to a single fact.
2. **Search the project first:** contract, context, code, configuration, history.
3. **Verify outside** when the fact is external: documentation for the exact version in use.
4. **Record:** what was asked, what was found, what changed in the design and what remains open.
5. **Limit the output:** at most ten findings, each with its source and its practical consequence.

## Rules

1. Do not research what the project contract already decides; that is noise.
2. Separate the installed version from the latest published one. Behaviour of the latest does not explain
   the current.
3. If documentation contradicts observation, the observation wins and the discrepancy is recorded.
4. If there are no open questions, say so in one line. Do not produce an empty investigation.
5. There is no cost target per finding: the limit is usefulness, not exhaustiveness.

## Output format

- **Questions:** one line each, with state (`resolved`, `assumption`, `blocked`).
- **Findings:** fact, source, effect on the design.
- **Discards:** what was checked and changed nothing, one line per discard.
