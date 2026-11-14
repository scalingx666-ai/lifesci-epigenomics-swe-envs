# Issue

**Title:** update for deepTools >= 3.0

## Description

The `--normalizeUsingRPKM` option in deepTools bamCoverage / bamCompare were replaced in v3.0.
The equivalent call is now `--normalizeUsing RPKM`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `445a3f65d466`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.