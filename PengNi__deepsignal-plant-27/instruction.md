# Issue

**Title:** Update environment.yml

## Description

Currently the conda environment .yaml installs scipy v1.9.1 which experiences a floating point error when running tombo resquiggle, downgrading to 1.7.3 fixes this. See: 

Experienced this while running the example dataset!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a056d23dc43c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.