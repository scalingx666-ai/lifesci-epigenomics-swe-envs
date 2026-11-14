# Issue

**Title:** Save one BigWig file per track

## Description

Originally, full chromosome prediction script was writing a bigwig file per chromosome for each track. Now that this functionality is wired through `agt predict` and can be used to generate predictions for across, or all, chromosomes, it makes sense to generate a single file per track, with `--split-by-chrom` available if that behavior is desired.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dffeca12f324`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.