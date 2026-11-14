# Issue

**Title:** Added support for tabix indices to speed up retrieving regions of indexed bedGraph files

## Description

I also added a line to the README about this. This way, users don't have to wait to read through an entire bedGraph file line by line just to retrieve a small region.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1a089d79e4d4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.