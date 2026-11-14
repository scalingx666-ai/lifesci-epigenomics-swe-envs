# Issue

**Title:** Remove interaction OvA. Update documentation.

## Description

- Tried extensively to get OvA to work for interaction terms
- Most recent version, that still is not perfectly stable to changing ref levels, but feels close to being so, is here: https://github.com/bednarsky/dea_limma/tree/fix-OvA-by-calculating-matched-level-interaction-term-difference
- Have to move on and drop this for now, thus, here a PR to **not support OvA for interaction terms**
- Tested on multiple kinds of formulas, stable as described in the docs 

    > Fall in love with the problem, not the solution
    > Liz Reid, Google Head of Search

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ab1c9ce313d5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.