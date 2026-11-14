# Issue

**Title:** Add keep_temp_files argument to call-methylation-state

## Description

Running methylpy call-methylation-state and wanted to set --keep-temp-files True, and realized that the argument was not passed on to the function.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d354ef50e8e6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.