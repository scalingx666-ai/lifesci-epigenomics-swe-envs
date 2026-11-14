# Issue

**Title:** Fix cropped-window coordinates in motif-merging example

## Description

## Summary
Fix crop-relative seqlet reconstruction in `examples/MergeMotifsAcrossRuns.ipynb`.
The notebook retains full-length arrays, so stored HDF5 starts and ends need the
centered input-window origin added before reconstructing sequences and scores.

- Read each run's HDF5 `window_size`; use explicit per-file configuration for legacy files.
- Validate matching input shapes and positive integer windows; normalize NumPy integer
  metadata before arithmetic to avoid unsigned promotion and narrow-integer overflow.
- Preserve full arrays, example-index remapping, strand handling, background calculation
  and the merge algorithm. Keep the input crop window separate from merge trimming.
- Explain the coordinate frames and clear the affected cell's stale outputs.

Related legacy report: jmschrei/tfmodisco-lite#48. This addresses coordinate
reconstruction, not the separate negative-dimension merge error in #128.

## Verification
WSL Python 3.12.3 / NumPy 2.4.6 / h5py 3.16.0 / pytest 9.2.0:

| Command | Result |
|---|---|
| `python -m pytest tests/test_merge_motifs_example.py -q` | 40 passed |
| `python -m pytest -ra` | 1423 passed, 33 skipped, 69 deselected, 16 warnings |
| `python -m pytest -m 'cmd or not cmd' -ra` | 1490 passed, 35 skipped, 18 warnings |

The tests execute the notebook's actual reconstruction statements selected by AST,
with tiny NPZ/HDF5 fixtures and real `TrackSet` reconstruction. The untouched baseline
returns 20 A bases instead of the expected 20 C bases; the fix returns 20 C with the
correct scores. Earlier separate-copy rollback checks restored the original failure.
All 22 additional local review-oracle cases also pass; they are not part of this patch.

The default suite excludes CLI-marked tests; the explicit all-markers command includes
them. Skips comprise 28 existing known-bug cases and seven external MEME `tomtom` cases.
The existing environment and dependency manifests are unchanged. No GPU, TensorFlow,
model training/inference, or external dataset download was needed. This does not claim
execution of the original notebook's installation/download cells or interactive kernel,
nor local macOS/native Windows testing. Hosted CI status is separate from these local runs.

## Assistance
GPT assistance was used for diagnosis, implementation, review and testing. The contributor
reviewed and reran the earlier coordinate work with GPT assistance; the subsequent
metadata correction and final verification were performed with assistant support.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2e788b9a8d54`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.