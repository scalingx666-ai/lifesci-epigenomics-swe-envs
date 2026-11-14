# Issue

**Title:** Made dump_per_read_statistics an executable script

## Description

Turns `dump_per_read_statistics.py` into an executable script (assuming permissions are set accordingly). Runs by `dump_per_read_statistics.py [-h] [-o OUTPUT_FILE] input_file`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `25c72796effa`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.