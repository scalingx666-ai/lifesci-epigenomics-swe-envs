# Issue

**Title:** aligner: staging and --nucleotide_coverage follow-ups

## Description

Follow-ups from the reviews of #1212 and #1200. Staging now only works inside a driver scope and publishes each input all or nothing, with the BAM renamed last; the gzipped aux FASTQs check their final flush; `--nucleotide_coverage` loads the genome once and never leaves a half-written stats file. Successful runs produce the same files with the same bytes. Also adds the tests the coverage audit found missing.

<details><summary>Details (AI-assisted)</summary>

- `staging::scope` replaces `finish_staged`: `stage`/`create`/`scratch` error outside a scope (or on another thread). `commit` renames in reverse registration order, so the BAM goes last; a failed rename removes what that commit already published and names both paths.
- `AuxWriter::finish` and `merge_aux_gz` flush the `BufWriter` after `GzEncoder::finish`; that only drains the buffer, so the single-core/multicore byte identity holds.
- bam2nuc: `run` = `load_genome` + `run_with_genome`; standalone output is unchanged (goldens + composition oracle green). The aligner loads once and removes the stats file on any bam2nuc error.
- `merge_coverage_files_ARGV.py` validates before writing; the wrapper resolves its path with `realpath`.
- New tests: staging scope/thread/rename units; `--nucleotide_coverage` with `--multicore 2`, `--basename`, two inputs; Linux-only `/dev/full` failures for the bam2nuc write and the gz aux finish; 5-Base duplex/consensus/simplex kept for the finished input; combined-index failure; standalone consensus.
</details>

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `090054e6d309`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.