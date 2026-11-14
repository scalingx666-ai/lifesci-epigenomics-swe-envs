# Issue

**Title:** Removed multiprocessing during training

## Description

_Same pull request as (), because I had deleted my original fork:_

According to the comment, the intention is to stop multiprocessing after loading the data into memory. However, setting num_workers = 1 results in a creation of an extra process later on when using the custom data loader.

Having the extra worker resulted in deadlocks when I worked with larger data.

Corrected by setting num_workers = 0.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6b846b65e9c0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.