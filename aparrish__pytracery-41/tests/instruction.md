# Issue

**Title:** Add support for Python 3.8-3.10

## Description

I wondered whether it worked on recent versions of Python, and the tests pass, so it looks like it does!

I'm not sure how much backward compatibility you want to maintain (and the maintenance burden is probably slight anyway), so I didn't drop any outdated versions.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5800d1bbc911`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.