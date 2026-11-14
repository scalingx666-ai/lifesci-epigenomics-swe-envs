# Issue

**Title:** Limit GREAT region annotations

## Description

## Control retrieval of regions with a new config parameter

Introduce the `map_associated_regions` parameter to decide the number of hits for which we retrieve regions and associated genes.
* If set to 0, no retrieval
* If set to n >=1, associated genes and regions for the top n hits accorded to adjusted p value are retrieved
* if set to -1, associated genes and regions for all significant terms according to adjusted p value threshold are retrieved

* Added great parameters as params in the enrichment rule

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `921efa4dd571`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.