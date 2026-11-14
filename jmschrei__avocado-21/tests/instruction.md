# Issue

**Title:** RELEASE 0.4.1: document current behavior, pin dependencies, deprecate

## Description

This pull request was prepared with Claude Code.

Version 0.4.1 is the final release of this code base before Avocado is re-implemented as a simpler package. It documents the code as it stands, pins the dependencies to versions that still work together, and marks the package as deprecated. It changes no code.

## Why 0.4.1

PyPI's 0.4.0 (2020-09-04) was built from 20bba0a with the version changed only in the uploaded package. That code still passes `pickle_safe=True` to `fit_generator` (#13). 0.3.6 (2021-04-13) removed it (#14), but its version number is lower than 0.4.0's, so `pip install avocado-epigenome` kept installing 0.4.0. 0.4.1 is the 0.3.6 code plus this PR.

## Changes

**Packaging** (`setup.py`). Upper bounds on the dependencies, each one a failure observed when it was lifted:

| Bound | Failure without it |
|---|---|
| `keras < 2.4` | Keras 2.4.3: `ImportError: Keras requires TensorFlow 2.2 or higher` |
| `numpy < 1.22` | numpy 1.22.4: Theano 1.0.5 raises `module 'numpy.distutils.__config__' has no attribute 'blas_opt_info'` on import |
| `h5py < 3` (new) | h5py 3.11.0: Keras 2.3.1 raises `'str' object has no attribute 'decode'` in `load_model` |
| `python_requires='<3.9'` | h5py 2.10.0 has no Python 3.9 wheel and its source build fails |

`long_description` is the README, so the PyPI page shows the deprecation notice.

**Documentation.** README: deprecation notice, tested environment, `KERAS_BACKEND=theano`, where each pre-trained model can still be downloaded, Zenodo download steps for the default CLI example, and a known-issues section. `cli/README.md` matches `--help` again. Docstrings for `build_model`, the `avocado.io` generators, `download_bigWig` and `get_params`; corrected docstrings for `fit`, `fit_celltypes`, `fit_assays` and `load`. `CHANGELOG.md` reconstructs every PyPI release from the sdists, matched to the commit each was built from.

## Testing

Run on Linux, on the CPU, with Python 3.8.20, Keras 2.3.1, Theano 1.0.5, h5py 2.10.0, numpy 1.21.6, pandas 2.0.3 and pyBigWig 0.3.22. A fresh `pip install` of the 0.4.1 wheel resolves exactly these versions.

- Every public method of `Avocado` on the pilot data in `data/`: `fit` with each `sampling` value, `predict`, the three embeddings, `save`/`load`, `load_weights`, `fit_celltypes`, `fit_assays`, `get_params`. Each Python API bug listed under Known issues was reproduced, except the `NameError`s in `download_bigWig`, which cannot be reached on Python 3 because `avocado.utils` does not import there. Nothing else failed.
- `Avocado.load` and `predict` on the chr21 Roadmap and ENCODE Core models from Zenodo.
- `avocado-impute` end to end on a small custom model, with the bigWig values checked against `sinh(predict)`, and on the chr21 ENCODE Core model from Zenodo with `-k` and the other chromosomes missing. That run took 32.7 minutes on one CPU core, peaked at 1.7 GB of memory, and wrote chr21 up to (n − 1) × 25 bp, as the README describes. The README's original CLI example fails as described: every download returns 403, then the tool raises `Model file for chromosome chr1 not found`.
- `twine check` passes on the sdist and the wheel.

## Surfaces

- Tests: the repository has no test suite. The checks above were run by hand and are not added, since the code is being replaced.
- Docs: no docs site; README, `cli/README.md` and docstrings updated.
- Changelog: `CHANGELOG.md` added.
- Bundled skill: none.
- Compatibility: no code changes; the pre-trained models load and predict under the pinned versions.

Back-compatible: no public API changes.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `35961b6b0374`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.