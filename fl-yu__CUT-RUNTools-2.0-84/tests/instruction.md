# Issue

**Title:** Edits in installation script and bulk-pipeline script

## Description

The correct python module name is "umap" not "umap-learn". This ensures the validation script runs well.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cf72ca5d0801`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.