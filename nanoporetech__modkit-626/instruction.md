# Issue

**Title:** Add `--min-samples` and `--min-sample-coverage` to `bedmethyl merge`

## Description

## Summary

Adds two optional, backward-compatible flags to `modkit bedmethyl merge` so it can keep only positions that are reproducible across inputs (an inner join), instead of always performing an outer join.

Documented in Feature Request #625 

- `--min-samples <N>` (default `1`): output a position only if it is present in at least `N` input files. `1` reproduces the current outer join; setting `N` = number of inputs performs an inner join.
- `--min-sample-coverage <C>` (default `0`): an input only contributes to a position (both for the `--min-samples` tally and for the summed counts) when that input's record has at least `C` valid coverage.

Defaults (`--min-samples 1`, `--min-sample-coverage 0`) reproduce the existing behaviour exactly.

## Motivation

When merging replicates, the outer join keeps single-replicate (non-reproducible) positions, and the merged bedMethyl retains no record of how many inputs contributed, so this cannot be filtered downstream. Doing it inside `merge` is natural and efficient because the merge already reads each input per region via the tabix index. See the linked issue for details.

## Implementation

In `merge_data` (`modkit-core/src/bedmethyl_util/subcommands.rs`) the per-region accumulator changes from `FxHashMap<Key, BedMethylLine>` to `FxHashMap<Key, (BedMethylLine, usize)>`, where the `usize` tallies how many inputs contributed to that `Key` (each input has at most one record per key). A record below `--min-sample-coverage` is skipped (so it counts toward neither the tally nor the sums). After accumulation, `into_values()` is filtered to entries whose tally `>= --min-samples`. The CLI args are validated in `run()` (`--min-samples >= 1`; a warning if it exceeds the number of inputs). No change to the region-wise, tabix-indexed, rayon-parallel structure.

## Docs

- `book/src/intro_bedmethyl_merge.md`: new "Requiring positions in multiple samples (inner join)" section with examples.
- `book/src/advanced_usage.md`: regenerated (only the `bedmethyl merge` section changed).
- `CHANGELOG.md`: `[Unreleased] -> Adds` entry.

## Testing

**Unit/integration tests** (`modkit/tests/test_bedmethyl_util.rs`) — `cargo test -p modkit --test test_bedmethyl_util`, 4/4 pass:
- existing `test_bedmethyl_merge` (backward-compat, unchanged) — still passes;
- `--min-samples 2` over the same input twice keeps all shared positions;
- `--min-samples 3` over two inputs drops everything (insufficient samples);
- a high `--min-sample-coverage` excludes all records.

**Build** — both profiles pass on Ubuntu with the default (pure-Rust `burn`/`ndarray`) backend, no libtorch required:
- `cargo build` (debug) — OK
- `cargo build --release` — OK
- `cargo fmt` applied to the changed files.

**Real data** — merge of three ONT DRS replicate bedMethyl files (~120M rows each), transcriptome-aligned:
Below timings are from a debug binary.
```
=== backward-compat: --min-samples 1 (outer) ===
  elapsed: 1128s
=== inner join: --min-samples 3 --min-sample-coverage 5 ===
  elapsed: 1041s
=== record counts ===
  --min-samples 1 (outer)            : 143592975
  --min-samples 3 --min-sample-cov 5 : 81647746
  inner / outer retained             : 56.86%
=== cross-check: existing pipeline outer merge record count (modkit 0.6.3 release) ===
  existing d0_merged.bed.gz          : 143592975
  (should equal --min-samples 1 count: 143592975)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a8c0846f0e7c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.