# Issue

**Title:** Reject degenerate HMM models before decoding

## Description

hmmratac can save a fitted Gaussian model with an indefinite covariance and fail only when decoding starts. Validate fitted and loaded models before returning, saving, or decoding them, and report actionable training-region/cutoff guidance through the existing CLI error handler. Also handle covariance failures raised inside Gaussian fitting, before post-fit validation can run.

Check covariance shape, finiteness, symmetry, and positive definiteness, plus emission parameters and state probabilities. Preserve valid single-region training, Gaussian/Poisson predictions, and saved-model JSON round trips. Add public-API and bounded CLI regressions, including failure before output with `--modelonly`.

. Part of #757.

### Verification

The compiled baseline at `ebdc4417ab304b2cd9e00486800e6f349d06ef9c` accepts and saves an intentionally indefinite model, then raises the reported covariance error during prediction. The original BAM-driven training failure remains unreproduced because that input is unavailable. Constant-data training independently produces a degenerate model; depending on the numerical backend, fitting either returns an invalid model or raises a covariance error inside a subsequent E-step. Both paths now provide the same actionable guidance. Unrelated input and Poisson training errors retain their original exceptions. Full evidence is in `test/issue_814_verification.md`.

### Validation

- Latest affected API modules, including all new subprocess regressions: **151 passed, 1 existing skip** in-place and **151 passed, 1 existing skip** against a rebuilt installed wheel from clean source.
- Deterministic regression reproduces the Linux CI failure inside a real E-step; CLI probes cover both early fitting failure and invalid returned models, with and without `--modelonly`.
- Prior broader installed-wheel HMM suite including slow real-data controls: **428 passed, 2 existing skips**. In-place CLI rejection and all five real-data controls: **11 passed**.
- Subprocesses use the parent's installation, training probes verify the exact compiled extension path, and probes have 30-second timeouts.
- `git diff --check` passes.

### CI confirmation for `3146541`

The [previously failing Linux x64 Python 3.14 job](https://github.com/macs3-project/MACS/actions/runs/41974653971/job/127070592738) now passes: **5,460 tests passed, 471 skipped**, followed by successful CLI integration tests and source-distribution creation. The full x64 Python 3.13 and ARM64 Python 3.12 jobs also pass. Additional Linux native-compatibility jobs pass across NumPy 1.26.4, 2.0.2, 2.1.3, 2.3.5, and current NumPy 2, including installed-wheel checks. Remaining matrix jobs are still running or queued; no failures have been reported on the updated commit.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ebdc4417ab30`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.