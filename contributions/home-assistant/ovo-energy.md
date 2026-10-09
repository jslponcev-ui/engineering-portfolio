# Home Assistant — OVO Energy daily usage resilience

**Outcome:** merged into Home Assistant Core on 9 October 2026.
**Contribution:** [PR #185519](https://github.com/home-assistant/core/pull/185519).
**Final implementation:** [60e4b9f](https://github.com/home-assistant/core/commit/60e4b9f88f6d74122e742bf15d4481e49e888433).

## Problem

OVO Energy can temporarily return an empty or absent daily electricity or gas usage list. Sensor lookups assumed the last entry existed, which could raise an error instead of reporting an unknown value.

There was a second recovery case: if fuel data was absent during initial setup, no sensors for that fuel were created. A later successful update therefore had no entities to update.

## Final change

Guarded consumption, cost and timestamp lookups before accessing the last entry. Kept zero consumption and zero cost as valid values.

Added discovery from coordinator updates so sensors appear when their fuel data first becomes available. The discovery listener avoids duplicate entities and is removed on unload. Accounts reporting only one fuel do not receive entities for a fuel they do not report.

## Validation

The final implementation passed all **17 OVO Energy integration tests**. Regression coverage checks:

- Empty lists and None for each fuel, with the other fuel unchanged.
- Existing sensors reporting unknown values while usage is missing.
- Recovery on the next scheduled update.
- A missing fuel during initial setup and discovery when data arrives.
- An initially empty account that later receives usage data.
- Zero values remaining valid and discovery avoiding duplicates.

[GitHub CI run 37962899805](https://github.com/home-assistant/core/actions/runs/37962899805) completed successfully on the final implementation commit. Ruff and single-process Pylint also passed locally.

## Review and acceptance

[An initial review finding](https://github.com/home-assistant/core/pull/185519#discussion_r4232488904) identified the missing-first/available-next sequence. The implementation and tests were expanded to cover that setup path before merge.

[Maintainer approval](https://github.com/home-assistant/core/pull/185519#pullrequestreview-5473411354) was given by abmantis. The PR was then merged into dev as [4a9e5638](https://github.com/home-assistant/core/commit/4a9e5638a2ef299ddb0021f2ccd1a94122f5618b).

## Engineering lesson

An existing entity losing its value and an entity never being created require different recovery mechanisms. Tests that always begin with complete data can miss a setup failure. Exercising scheduled updates verifies polling and discovery through the integration's actual update path.

## Skills demonstrated

Python, asynchronous coordinator updates, defensive API parsing, regression testing, entity lifecycle management and responding to review feedback.

## Defensible CV wording

Contributed a merged Home Assistant fix for missing OVO Energy daily usage, including late sensor discovery and regression tests for scheduled-update recovery.

This is an open-source contribution, not employment at Home Assistant. AI assistance was used in the workflow; the public diff, tests and review provide the evidence for the contribution. The contributor must be able to explain the code and tests when presenting this work.
