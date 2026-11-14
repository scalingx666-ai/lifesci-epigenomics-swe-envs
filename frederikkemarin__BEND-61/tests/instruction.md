# Issue

**Title:** Enable embedders to return logits

## Description

So far, embedders only returned embeddings. Logits and crossentropy can also be useful.

First enabled for HyenaDNA. New options for HyenaDNAEmbedder:

```python3
return_logits: bool=False, return_loss: bool=False
```

Other models WIP.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8665d447a754`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.