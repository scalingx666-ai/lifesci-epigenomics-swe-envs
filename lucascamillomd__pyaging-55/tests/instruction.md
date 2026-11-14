# Issue

**Title:** Release pyaging 0.6.0 with DNAm median fallback

## Description

Human CpG beta clocks without a model reference can now fill absent columns from a packaged lookup. It covers 984,158 CpGs from Hannum 450K and BoA EPIC v2. Shared probes use the equal-weight mean of the two cohort medians. Uncovered features fall back to zero.

The release includes a reproducible generator, source checksums, and a held-out validation report. Median fill reduced prediction shifts in 10 of 12 comparisons. Two masks worsened, including the 25% glioma mask.

Validation: 678 offline tests passed. Two complete generations produced identical resource bytes. The wheel and source distribution contain the lookup, and the installed 0.6.0 wheel loaded it offline.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c15cf28347f5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.