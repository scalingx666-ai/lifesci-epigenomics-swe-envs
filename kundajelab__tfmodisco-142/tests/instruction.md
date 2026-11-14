# Issue

**Title:** Fix seqlet FASTA export coordinates and lengths

## Description

## Problem
`write_fasta_from_h5` uses a one-based header start (`start + 1`) to index a zero-based
array, dropping the first base. It also applies crop-relative seqlet positions directly
to full-length input sequences. An untouched-baseline fixture with
`A*400 + C*200 + G*400`, a 200-base crop and stored `[10,30)` exports **19 A bases**
instead of **20 C bases**.

## Fix
- Extract `[crop_start + start, crop_start + end)`, using
  `crop_start = input_length // 2 - window_size // 2`.
- Apply `+1` only to the genomic header start. Preserve existing one-based inclusive
  headers and forward-genomic output, including when `dir=-`.
- Validate array shape, integer window/range and seqlet bounds; normalize NumPy integer
  metadata before arithmetic. Preserve explicit-window precedence and legacy support.
- Add documentation, 42 parametrized regression cases (one CLI-marked), and enable the
  two existing skipped FASTA regressions. Correct old assertions that encoded the bug.

Only `write_fasta_from_h5` changes in the library. BED export, strand conventions,
motif merging and the separate odd-window issue in #134 are unchanged.

## Verification
WSL Python 3.12.3 / NumPy 2.4.6 / h5py 3.16.0 / pytest 9.2.0:

| Command | Result |
|---|---|
| `python -m pytest tests/test_fasta_coordinates.py tests/test_io.py -k fasta -m 'cmd or not cmd' -q -ra --tb=short` | 55 passed, 67 deselected |
| `python -m pytest -ra` | 1426 passed, 31 skipped, 70 deselected, 16 warnings |
| `python -m pytest -m 'cmd or not cmd' -ra` | 1494 passed, 33 skipped, 18 warnings |

- Eight principal regression cases fail before the fix and pass afterward; restoring
  only the exporter on a separate copy restores all eight failures, including the 19 A output.
- Four actual `modisco seqlet-fasta` invocations on previously generated small fixtures
  checked 90 distinct seqlets / 180 exported records against stored HDF5 sequences.
  Full and already-cropped inputs produce byte-identical FASTA outputs; both strands pass.
- Pristine upstream all-markers suite: 1450 passed, 35 skipped, 18 warnings.
- `git diff --check` and dependency compatibility checks pass. Dependencies/manifests unchanged.

The default suite excludes CLI-marked tests; the all-markers run includes them. The
remaining 33 skips are 26 unrelated known-bug cases and seven external MEME `tomtom`
cases. No GPU, TensorFlow installation, model training/inference or new external
dataset download. Local verification is WSL Python 3.12 only; hosted CI is separate.

## Assistance
GPT assistance was used for implementation, review and testing. These results describe
executed local checks, not independent human approval or maintainer acceptance.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `28154f18ec42`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.