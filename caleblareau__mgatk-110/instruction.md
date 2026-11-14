# Issue

**Title:** Fix CLI version regression, gzip barcode counting; add CI + tests

## Description

## Summary

Fixes two shipped regressions, then adds CI and tests to lock them in.

### Bug fixes
- **CLI version crash (blocker):** a bad merge left `@click.version_option(version=__version__, …)` at module import time while `__version__` was only bound *inside* `main()`. Every console script (`mgatk`, `mgatk-del`, `mgatk-del-find`) crashed with `NameError` on any invocation, including `--version`, and `pytest` failed at collection. `mgatk/__init__.py` now resolves `__version__` from installed package metadata (was a stale hardcoded `0.7.1`), and the three CLI modules import it at module level.
- **Gzipped barcodes:** `mgatkHelp.file_len` used plain `open()` and raised `UnicodeDecodeError` on a `.gz` barcode list, breaking `tenx`/`check` mode. It now uses the gzip-aware opener.

### Tests
- `tests/test_version.py` — all three CLIs import and report the installed version.
- `tests/test_remove_background_removed.py` — CellBender/mitobender functionality is fully removed; only a `remove-background` deprecation stub remains (no output produced, no implementation modules importable).
- `tests/test_cli.py` — added gzipped-barcode end-to-end tests for `bcall` and `tenx`.

### CI
- `.github/workflows/ci.yml` — `unit` job (Python 3.11/3.12: install, version checks, pytest) + `end-to-end` job (R/Bioconductor + snakemake, full pipeline on bundled data).
- `.travis.yml` — rewritten from the old `echo $PATH` stub to actually install deps and run the suite.

### Docs
- `README.md` — replaced the dead Travis badge with the GitHub Actions badge and switched to the dynamic shields.io PyPI version badge.
- Wiki (pushed separately): dropped Picard/`java` dedup references in favor of `bam-dedup`; refreshed the `--help` block and noted the deprecated `--max-javamem` flag and `remove-background` mode.

Full suite: **50/50 passing** locally (with snakemake on the same PATH as the package).

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `673435161e22`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.