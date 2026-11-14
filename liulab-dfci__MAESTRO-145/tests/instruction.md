# Issue

**Title:** Release of v1.5.1

## Description

1. Support snRNA-seq pipeline.
2. Fix bug in MAESTRO sample-init script to read samples with an id greater than 9.
3. Add STARsolo parameters --soloFeatures to support quantification of different transcriptomic features.
4. Update MAESTRO documentation to v1.5.1.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7dd51fa11012`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.