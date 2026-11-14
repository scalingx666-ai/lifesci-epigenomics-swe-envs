# Issue

**Title:** fix(xena-tcga-gene-query): write the reproducibility bundle with the shared helpers

## Description

## Summary

xena-tcga-gene-query wrote `commands.sh` as curl lines against the API and no `environment.yml` or checksums, so it was listed in `skills/contract_known_gaps.json` for `repro_files`. It now writes the bundle through the shared helpers and leaves the known-gaps list.

```diff
 write_outputs(results, base_url, output_dir, mode, *, replay)
-  commands.sh       curl "${BASE_URL}/api/v1/diff-expr?gene=TP53&cancer=LUAD" ...
+  write_portable_commands_sh   re-runs this CLI call: --base-url, task, --gene/--gene2/--cancer, --output "$OUTPUT_DIR"
+  write_environment_yml        stdlib only, no pip deps
   run.json
+  write_checksums              report.md, result.json, reproducibility/*, written last
```

The API key is never written to the bundle; a replay reads it from `UCSCXENA_API_KEY` as the original run can.

## Evidence

- **Before:** `reproducibility/` holds `commands.sh` (three curl calls, demo hard-coded to TP53/EGFR/LUAD whatever was asked) and `run.json`; no `environment.yml`, no checksums.
  **After:**
  ```text
  $ python skills/xena-tcga-gene-query/scripts/query_tcga_api.py --demo --output <tmp>/out   # exit 0
  $ cd / && PYTHON=<venv python> bash <tmp>/out/reproducibility/commands.sh      # exit 0
  $ cd <tmp>/out && shasum -a 256 -c reproducibility/checksums.sha256
  5 files: OK
  ```
- New `tests/test_repro_bundle.py` (4 tests): the bundle verifies, `commands.sh` replays the task and flags given, and the API key never appears in the bundle.
- `skills/xena-tcga-gene-query/tests` + `tests/`: 511 passed, 2 skipped, 2 xfailed. `generate_catalog.py`: no diff.

Made with `scripts/factory/run.sh repro-bundle xena-tcga-gene-query` (a separate review agent found nothing it could reproduce), then I reviewed it and rebased it onto main.

## Merge Danger

**Door:** two-way

Revert restores the old bundle code and the known-gaps entry.

**Blast Radius:** skill

This and the other repro-bundle PRs (#778, #779, #780 and the five opened alongside this one) each delete one entry from `skills/contract_known_gaps.json`. Whichever merges later may need a one-line rebase there.

---

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a4a8c7281c8d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.