# Issue

**Title:** Report invalid BEDPE/FRAG coordinates accurately

## Description

Non-numeric BEDPE/FRAG coordinates currently produce misleading interval-order errors or are silently converted by `atoi`; `python -O` also disables interval assertions. Validate complete coordinate fields within the signed 32-bit range and raise informative `ValueError` messages identifying the field and row. Enforce interval ordering explicitly, including under optimized Python, and route unusual FRAG coordinates through checked parsing while retaining its usual C fast paths.

Add 40 bounded public-API subprocess regressions covering build/append, normal/optimized Python, EOF variants, plain/gzip/BGZF input, threaded FRAG parsing, malformed and out-of-range coordinates, and valid-input controls. Update existing parser and callpeak expectations for the corrected errors.

This is item **18/18**, the final item in the #757 verification series. Merging this PR completes both the individual ticket and the parent series.

.
.

Validation:

- Reproduced the original issue with 192 bounded public-API probes on a freshly compiled baseline at `b7929196a98032219afbef43b00ce3e0996aacb7`.
- In-place build and wheel build passed.
- Initial in-place parser/EOF tests: **314 passed, 99 skipped**; final coordinate/singleton tests: **46 passed**.
- Every parser module plus singleton FRAG tests against an installed wheel from a clean source checkout: **444 passed, 99 skipped**. The runner verifies the installed compiled parser and absence of in-place source extensions.
- Both affected callpeak modules: **75 passed** in-place and **75 passed** against the installed wheel.
- Full suite: **5,476 passed, 491 skipped, 4 failed**. All four failures were old diagnostic/exception expectations collected before their updates; their complete updated modules passed in both builds, and `pytest test --lf` passed all **4** cases. The entire suite was not rerun a second time.
- `git diff --check` passed. No new skips were added.

[Detailed reproduction and validation evidence](https://github.com/macs3-project/MACS/blob/408d6e03e9928aa718796c7972369594546e6292/test/issue_815_verification.md).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b7929196a980`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.