# Issue

**Title:** use scverse-misc for dataset registry and downloads

## Description

Implements #472 by moving dataset metadata into `snapatac2/datasets.yaml` and routing downloads through `scverse-misc[datasets]`.

- Add YAML registry for all 22 dataset files.
- Use `scverse-misc` for registry parsing and fetching.
- Preserve existing `snap.datasets.*` and `snap.genome.*` APIs.
- Add runtime fallback to original URLs if the new URL fails.
- Bump Python requirement to `>=3.12`.
- Drop Python 3.11 wheel builds.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7be574427086`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.