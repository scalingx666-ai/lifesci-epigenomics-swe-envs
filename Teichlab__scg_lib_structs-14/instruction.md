# Issue

**Title:** Fix pMENTS strand in sci-ATAC-seq

## Description

19 bp ME sequence was showing the wrong strand, should be `5Phos/CTGTCTCTTATACACATCT`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6c05b8fe0878`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.