# Issue

**Title:** Update motif positions with genome seqinfo

## Description

Add seqinfo assignment to motif positions so Extend() can properly guard against extending beyond chromosome ends.

This is a possible fix for #1982. Please let me know if you have any questions -- Thanks a lot.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bb231ab70178`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.