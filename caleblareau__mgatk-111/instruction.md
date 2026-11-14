# Issue

**Title:** Bump python_requires to >=3.11; drop universal wheel flag

## Description

Aligns packaging metadata with the actual dependency floor.

- `python_requires='>=3.8'` → `'>=3.11'`. The current stack (snakemake 9, numpy 2.5, pandas 3.0) requires ≥3.11, so the old declaration advertised installs pip cannot resolve. CI already runs on 3.11/3.12.
- Add `Programming Language :: Python :: 3.11` / `3.12` classifiers.
- Remove `[bdist_wheel] universal=1` from setup.cfg — that flags a py2/py3 universal wheel, but mgatk is py3-only.

No code or dependency-version pins changed; nothing else needs a floor bump (the package's own imports are 3.11-compatible and rely on pip to resolve compatible transitive versions under the new `python_requires`).

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b29930633995`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.