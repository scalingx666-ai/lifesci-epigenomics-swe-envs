# Issue

**Title:** Run the test_controls profile with --with_control in CI

## Description

Implements the fix to 

`tests/controls.nf.test` never selects the `test_controls` profile, so it runs the default `test` profile; its snapshot is identical to `default.nf.test.snap` apart from the name and timestamp.
  `conf/test_controls.config` also never sets `with_control = true`, so its samplesheet's control columns are ignored. As a result, no CI job runs the `--with_control` code path.

  - `tests/controls.nf.test`: select `profile "test_controls"`.
  - `conf/test_controls.config`: set `with_control = true`; replace the obsolete `max_*` params with `process.resourceLimits`, as in `test.config`.
  - Snapshot regenerated.


## PR checklist

- [X] This comment contains a description of changes (with reason).
- [ ] If you've fixed a bug or added code that should be tested, add tests!
- [ ] If you've added a new tool - have you followed the pipeline conventions in the [contribution docs](https://github.com/nf-core/atacseq/tree/master/.github/CONTRIBUTING.md)
- [ ] If necessary, also make a PR on the nf-core/atacseq _branch_ on the [nf-core/test-datasets](https://github.com/nf-core/test-datasets) repository.
- [X] Make sure your code lints (`nf-core lint`).
- [X] Ensure the test suite passes (`nextflow run . -profile test,docker --outdir <OUTDIR>`).
- [X] Usage Documentation in `docs/usage.md` is updated.
- [ ] Output Documentation in `docs/output.md` is updated.
- [X] `CHANGELOG.md` is updated.
- [ ] `README.md` is updated (including new tool citations and authors/contributors).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6a9307709662`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.