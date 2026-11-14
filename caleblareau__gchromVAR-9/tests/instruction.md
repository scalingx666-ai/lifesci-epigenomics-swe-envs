# Issue

**Title:** Remove unneeded packages from DESCRIPTION

## Description

Just removing BiocGenerics, chromVARmotifs, motifmatchr and S4Vectors from the description as they're not actually used and installation will throw an error otherwise if they're not installed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e202461d0f4b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.