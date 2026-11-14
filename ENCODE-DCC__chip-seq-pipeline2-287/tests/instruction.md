# Issue

**Title:** v2.2.1

## Description

- Fixed the issue that test run takes too much resources. Now users can run a test pipeline on a laptop.
- Fixed conflicts in Conda installer.
- Updated documentation:
  - Pipeline still supports Conda as an optional method, DACC will no longer help fixing Conda conflicts on user's computer. If any error occurs, then switch to Singularity method.
  - Made README simpler.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a7b7d6fb5a93`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.