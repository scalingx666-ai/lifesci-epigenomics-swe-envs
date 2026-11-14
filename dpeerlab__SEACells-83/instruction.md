# Issue

**Title:** GPU-accelerated SEACells (opt-in, end-to-end on GPU)

## Description

This adds a GPU version of SEACells. It's off by default; turn it on with `use_gpu=True, use_unified=True` and nothing else changes:

```python
model = SEACells.core.SEACells(ad, build_kernel_on="X_pca", n_SEACells=90,
                               use_gpu=True, use_unified=True)
```

## What's different on the GPU

**The kernel stays on the GPU.** The old GPU code kept the kernel on the CPU, copied it to the GPU every iteration, and computed the reconstruction error on the CPU. Keeping everything on the GPU makes each iteration about 18x faster. cd34 finishes in 6.5s instead of 110s.

**The error avoids the big n×n matrix.** The error used to be computed by building an n×n matrix, which gets huge at scale (174 GB for 200k cells). It's now computed with the identity `||M - MBA||^2 = ||M||^2 - 2 tr(KBA) + tr(B^T K B A A^T)`, which only needs n×s memory (about 10 GB for 200k cells). Same number, far less memory, so it runs on a single GPU at sizes the old code couldn't.

## Same results

The GPU path gives the same answer as the CPU path (see `tests/test_unified_parity.py`), and on cd34 the CPU and GPU runs converge to the same error.

## Install

`uv sync` for CPU, or `uv sync --extra gpu` for the GPU version (installs RAPIDS, CuPy, FAISS). Details in `docs/gpu_speed_and_scale.md`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a0a00d779ab5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.