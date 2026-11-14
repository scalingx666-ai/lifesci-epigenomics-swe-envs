# Issue

**Title:** added a seed to grammar.flatten

## Description

Allows passing in a seed value to get repeatable results. If one is not given, passing in None uses the default behavior of system time as the seed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `101915dc41e1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.