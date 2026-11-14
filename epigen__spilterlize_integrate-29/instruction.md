# Issue

**Title:** Handle errors that arise when splitting leaves annotation variables with no levels

## Description

As in epigen/spilterlize_integrate#27. In config.yaml desired can now be over-specified and variables will be dropped gracefully if not applicable to a given split. If no batch, or covariates are possible to be removed then the integrated data is the input data as in removeBatchEffect (https://rdrr.io/bioc/limma/src/R/removeBatchEffect.R).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `39a3a68504d7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.