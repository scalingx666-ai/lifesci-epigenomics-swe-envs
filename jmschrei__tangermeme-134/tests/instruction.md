# Issue

**Title:** RELEASE bump version to 1.6.0 with changelog entries

## Description

Release 1.6.0: bumps `__version__` and closes the `Unreleased` changelog section as `Version 1.6.0`.

This release adds `PackedSequence`, dependency maps, fastISM in `saturation_mutagenesis`, FDR threshold calibration in `recursive_seqlets`, and faster `one_hot_encode`, `extract_loci` and `dinucleotide_shuffle`, plus the fixes listed in `docs/whats_new.rst`. It also rewrites the bundled Claude Code skill and fixes `randomize` probs, batched `reverse_complement`, the `plot_logo` ylim, the empty seqlet frame and silent convergence failures.

Full test suite: 3465 passed, 3 skipped, 2 xfailed. `sphinx -W` passes.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4c9e294687dc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.