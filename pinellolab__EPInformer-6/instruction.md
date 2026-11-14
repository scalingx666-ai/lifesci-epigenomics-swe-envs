# Issue

**Title:** Add K562 end-to-end reproduction walkthrough notebook

## Description

## Summary
- Adds `K562_walkthrough.ipynb`, a guided end-to-end runbook that reproduces EPInformer on K562: raw ENCODE data to ABC links to a 256 bp enhancer-activity encoder to the `EPInformer_v2` gene-expression model.
- Complements the existing `predict_enhancer_and_expression.ipynb` on the `pipeline` branch by walking through both training stages in order (encoder first, then the frozen-encoder expression model) with per-stage targets.

## Notebook contents
- Part 0: kernel/reference preflight (sets `HDF5_USE_FILE_LOCKING=FALSE`, checks required inputs).
- Part 1: enhancer-activity encoder with the `log2(0.1 + Activity)` target, 5-bin / L1KL recipe, one in-cell fold plus a 12-fold SLURM array; single-reverse ~0.734, fwd+RC ~0.740.
- Part 2: `EPInformer_v2` (f3 = distance + activity + Hi-C + promoter signal) on the frozen encoder; RNA ~0.86 / CAGE ~0.87.
- Part 3: evaluation and result-inspection cells (pooled metrics gated on all 12 fold files being present).

## Notes
- Preprocessing and training cells are opt-in / preview-by-default so Run All will not start large downloads or queue heavy jobs.
- Designed to run on an HPC GPU under the EPInformer conda env.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fca6a3436385`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.