# Issue

**Title:** Stream tagged BAM to stdout and protect source files

## Description

## Summary

. Support `tag --output -` as compressed BAM on stdout, without progress text contaminating the stream.

```text
output = "-" -> BAM stdout + progress stderr
file path   -> existing .bam/SAM format selection
            -> reject source-file aliases before writing
```

Bare relative output filenames now work. Prevent accidental overwrites of the alignment input or reference genome, including Unix symlinks/hard links.

## Evidence

- **Before:** `--output -` and bare `tagged.bam` failed directory validation; using the input as output succeeded and overwrote it.
  **After:** stdout decodes as compressed BAM with the exact header, record and XM string; relative .bam/.sam outputs have the correct formats; source reuse fails without changing source bytes.
- Five CLI tests cover stream decoding, relative filenames, direct input reuse, source aliases and reference preservation.
- All 175 test executions, strict Clippy, formatting and whitespace checks passed.
- File-output progress behavior and methylation calculations remain unchanged. No new dependencies.
- Shared CI repairs are separate commits.

- Adversarial review found a closed downstream could fail during buffered BGZF finalization while the pinned Rust writer returned exit 0. The follow-up owns the HTSlib handle with checked `hts_close` and RAII error cleanup; a deterministic closed-socket CLI regression failed before the fix and now exits nonzero. Independent re-review at `2d768f1`: **Ship**. The original closed-pipe case now exits nonzero, all 14 tagging tests pass, HD/SQ/RG/PG/CO headers round-trip exactly, and handle ownership/single-close cleanup were checked. The exact head also passed all eight applicable remote checks.

- Integration against actual master through PR #34 is at `10876d6`: conflict-free, exact tree match with reviewed preparation `27294ad`, all 224 tests and strict checks pass. Combined probes confirm identical BAM stdout for workers 1/3 and nonzero exit on a closed downstream with three workers. Checked finalization occurs exactly once after either tagging path. Independent integration re-review: **Ship**. Actual-head probes verified 4,103 ordered records, workers 1/3 decoded BAM equality with EOF and stderr-only progress, downstream failure, and hardlink source/reference protection. All eight hosted checks passed for updated head `10876d6` before merging.

## Merge Danger

**Door:** two-way

**Blast Radius:** output

`-` always streams BAM. File aliases are checked by file identity on Unix; non-Unix fallback uses canonical paths and has not been validated for hard links. Existing inputs and reference genomes cannot be deliberately overwritten in place. Production pipe throughput was not benchmarked.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `346da8d6ddb1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.