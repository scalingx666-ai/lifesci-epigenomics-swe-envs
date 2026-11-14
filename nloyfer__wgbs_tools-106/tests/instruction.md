# Issue

**Title:** Add other statistical tests for find_markers

## Description

Added Mann-Whitney U test as well as a Welch's t-test (non-equal variance) after an M-value transformation. This includes now allowing the user to choose which test to use to filter p-values.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `624f0c76f540`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.