# Issue

**Title:** Fix the sign of getLinkingNumber

## Description

## Description

.

`_getLinkingNumberCpp` sums signed projected crossings into `L` and returns `-L/2`. `intersectValue` already uses the standard crossing sign, so the negation makes `getLinkingNumber` return the opposite of the Gauss linking number, and of its own docstring, on every linked pair.

This change returns `L/2`. It also releases the `new[]` buffers in `intersectValue` and `_getLinkingNumberCpp` with `delete[]` instead of `free()`, and releases `data`, which leaked. `test_hopf_link_sign` pins the sign: -1 for the test's Hopf link, +1 with one ring reversed. The existing `test_hopf_link` only checked `abs(L) == 1`. `mutualSimplify`, `simplifyPolymer` and the Python signatures are unchanged.

## Validation

From this branch on `master` at `1e9ac0f`, in-place build on Ubuntu (WSL2), g++ 15.2.0, Python 3.14.4, openmm 8.6.1:

- Compiled `getLinkingNumber` vs a Gauss–Legendre double integral, 11 pairs (Hopf link in four orientations, T(2,2), T(2,4), T(2,6) in both chiralities, unlinked rings): 1/11 agree on `master`, 11/11 with this change.
- `pytest tests`: 64/64 passed on `master`, 65/65 with this change.
- `test_hopf_link_sign` with `master`'s `__polymer_math.cpp`: fails, 1 failed and 1 passed of the two Hopf tests.
- `valgrind` on a 30-point Hopf link: 16 `Mismatched free() / delete / delete []` contexts on `master`, 0 with this change. Definitely lost: 3,192 bytes on `master`, 2,712 with this change.
- `black -l 120 --check .`: 40 files unchanged. `isort --check-only .`: clean. `flake8 tests/test_polymer_math.py`: clean.

## Limits

Every non-zero result changes sign, so analyses run against `master` since #75 that kept the sign will invert. Results from v0.1.0 keep their sign and halve.

## PR Checklist

- [x] apply "black" to the whole codebase (`black .`)
- [x] apply isort to the codebase (`isort .`)
- [x] fun `flake8` and try to resolve all the issues (work in progress!)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1e9ac0f7955c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.