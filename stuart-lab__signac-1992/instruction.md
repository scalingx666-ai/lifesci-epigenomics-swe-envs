# Issue

**Title:** Update LinkPeaks

## Description

add FindCandidateLinks; mod LinkPeaks; mod CollapseToLongestTranscript

- changed default peak-TSS matching strategy

- added link attributes

- LinkPeaks no longer runs zscore calcs by default

- removed expectation for gene_biotype when using gencode annotations

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8a730c54acfe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.