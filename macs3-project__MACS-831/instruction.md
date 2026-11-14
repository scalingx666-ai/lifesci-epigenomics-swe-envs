# Issue

**Title:** Fix callvar assembly and spliced-read edge cases

## Description

## Summary

`callvar` could abort on reads shorter than the assembly overlap, crash when spliced reads shortened its reconstructed reference, or pass an empty-assembly sentinel into variant calling.

- Safely return no unitigs when fermi-lite filtering removes every read, and finish the peak while retaining any first-pass calls.
- Skip CIGAR N alignments with treatment/control diagnostics and guard reference reconstruction lengths.
- Validate assembly overlap as 1–63, handle small-overlap hashing safely, and document fallback behavior.
- Add bounded API/CLI regressions covering mixed inputs, empty results, and continuation to later peaks.

. Related to #757.

## Validation

- Full in-place suite: **5,730 passed, 101 skipped**.
- Focused in-place suite: **280 passed, 1 skipped**.
- Clean-source installed-wheel focused suite: **280 passed, 1 skipped**; imported extension paths verified.
- Assertion-enabled native API probes: **14 passed**; original short-read CLI probes now exit successfully.
- `git diff --check` passed.

Detailed baseline reproductions and results: `test/issue_813_verification.md`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b8dc7d694ad2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.