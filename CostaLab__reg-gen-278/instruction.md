# Issue

**Title:** Update signalProcessing.py

## Description

mat has been removed from numpy v2.0.0, replaced with asmatrix (https://numpy.org/doc/stable/release/2.0.0-notes.html#numpy-2-0-python-api-removals)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `86dee1afbf05`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.