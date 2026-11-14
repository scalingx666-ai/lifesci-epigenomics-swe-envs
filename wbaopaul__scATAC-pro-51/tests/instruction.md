# Issue

**Title:** Enable PE/SE choice in call_peak.sh

## Description

Decide either `-f BAM` or `-f BAMPE` based on the `isSingleEnd` parameter in the user's config.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b6380d961bd0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.