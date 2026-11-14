# Issue

**Title:** Add back to line in group

## Description

Hi,
I noticed that there were a back to line missing in `CountFragments`.
For example when running in a for loop for multiple fragment files:
```
Found 2393 cell barcodes
Done Processing 70 million linesFound 5448 cell barcodes
Done Processing 60 million linesFound 38519 cell barcodes
Done Processing 11 million linesFound 4242 cell barcodes
...
```
I just added it.

Also I am not sure this bloc is useful:
https://github.com/stuart-lab/signac/blob/2984cd24f41aaad893903ff0aead5cd6af09e512/src/group.cpp#L157-L159

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c6f316c16854`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.