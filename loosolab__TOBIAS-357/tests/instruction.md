# Issue

**Title:** Revise Python publish workflow for building wheels

## Description

Updated Python setup and build steps in CI workflow. Manylinux should be used as build enviroment.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c1e0671e1a5a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.