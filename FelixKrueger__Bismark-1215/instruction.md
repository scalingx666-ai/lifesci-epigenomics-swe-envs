# Issue

**Title:** aligner: clear stale stats files before --nucleotide_coverage runs

## Description

`--nucleotide_coverage` now removes every input's `nucleotide_stats.txt` before bam2nuc starts, so a run that fails (a genome load error, or a hard error on an earlier input) can't leave a stats file from a previous run next to a BAM it just rewrote, where `bismark report` would read it. The `--output_dir` docs stop promising that a failed run keeps an earlier run's outputs, and the `--nucleotide_coverage` docs mention the up-front clearing. Successful runs are unchanged.

<details><summary>Details (AI-assisted)</summary>

- Found by the post-merge review of #1213; the per-input removal after a failure stays, for a file this run half-wrote.
- New test `nucleotide_coverage_leaves_no_stale_stats_file`: a planted stale stats file is replaced on success, and removed when the loop stops at the first input. It fails without the fix.
</details>

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b06389178561`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.