# Issue

**Title:** build: explicit interpreter, drop --find-interpreter

## Description

build: supported python versions are now encoded in 1 place, toml classifiers
build: check_version_sync also on python versions + CI
ci: pypi upload to trusted publisher
ci: pytests don't fail fast

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b4fc6808aaf5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.