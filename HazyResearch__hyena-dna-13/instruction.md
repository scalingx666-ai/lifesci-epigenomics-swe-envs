# Issue

**Title:** Fix moving torchmetrics to cuda device 0

## Description

This PR removes `.to("cuda")` calls that cause the metric to be forcefully placed on GPU 0, causing a memory imbalance. A `torchmetrics` Metric is a `nn.Module`, and therefore it will be automatically placed on the right device when used in a LightningModule together with the Trainer.

The [best practice](https://lightning.ai/docs/pytorch/2.0.7/accelerators/accelerator_prepare.html#delete-cuda-or-to-calls) in Lightning is to not manipulate module or tensor placement unless there is a good reason for it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f6ea78112f25`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.