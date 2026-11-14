# Issue

**Title:** Moved preprocessing code into preprocess_logits_for_metrics function

## Description

This will reduce memory consumption as outlined in this thread: https://discuss.huggingface.co/t/cuda-out-of-memory-when-using-trainer-with-compute-metrics/2941/13

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6c7512b7494b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.