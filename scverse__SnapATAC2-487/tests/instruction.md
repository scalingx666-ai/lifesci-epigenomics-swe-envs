# Issue

**Title:** fix: preserve observation-major Harmony output

## Description

Update `_harmony` in `snapatac2/preprocessing/_harmony.py` to normalize `Z_corr` against the input observation count: keep an observation-major result unchanged and transpose a component-major result for compatibility with older `harmonypy` releases. `snap.pp.harmony` currently transposes `harmonypy`'s `Z_corr` result unconditionally before its public wrapper stores the matrix in `.obsm`.

Verify observation-major `Z_corr` is returned unchanged and can be written by `harmony(..., inplace=True)` to an AnnData `.obsm` entry with shape `(n_obs, n_components)`; Verify legacy component-major `Z_corr` is transposed to the same observation-major shape, including the `inplace=False` return path.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e669f8197585`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.