# Issue

**Title:** Preserve metadata columns during EPIC v2 probe aggregation

## Description

## Summary

Fix `epicv2_probe_aggregation` renaming metadata such as `tissue_type`, which caused the scheduled DNAm tutorial to fail with a `KeyError`. Strip only EPIC v2 replicate suffixes and average matching probes, including unsuffixed copies, while preserving other columns and their order.

Add a regression test for metadata preservation with and without duplicate probes, and include the original CI failure log in `log.txt`.

Reported local validation: preprocessing tests passed with 97 passed and 1 skipped; the DNAm tutorial passed; Ruff lint and format checks passed. The full pytest suite and pre-commit checks were not reported as run.

## Checklist

- [ ] `uv run pytest` passes locally
- [ ] `uv run pre-commit run --all-files` passes
- [ ] New clocks include a notebook in `clocks/notebooks/` and a validated entry in `clocks/metadata/clock_metadata.json` — not applicable; no new clocks
- [x] Scientific behavior of existing clocks is unchanged

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `412360cb97eb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.