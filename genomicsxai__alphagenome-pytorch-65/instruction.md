# Issue

**Title:** Junction head: apply RoPE in float32 under autocast, as in JAX

## Description

## Problem

Under `torch.autocast(device_type="cuda", dtype=torch.bfloat16)`, `SpliceSitesJunctionHead` computes its RoPE angles in bfloat16. `apply_rope` computes `theta = einsum('bs,f->bsf', positions, inv_freq)`, and CUDA autocast runs that einsum in bfloat16. Splice-site positions go up to 2**20, but bfloat16 has an 8-bit mantissa, so a position near 500,000 is rounded to a multiple of 2,048. That makes the rotation essentially random, and the error in the rotated embeddings is as large as their magnitude.

The JAX head casts the selected embeddings to float32 before RoPE (`_index_embedding(x, indices).astype(jnp.float32)` in `alphagenome_research.model.heads.SpliceSitesJunctionHead`). That keeps the angles in float32 whatever the compute policy is. Inference under the port's own `DtypePolicy` without autocast is unaffected. Autocast is the usual way to fit fine-tuning in memory, though, and it silently degrades junction predictions.

On a fine-tuning dataset of allelic splicing effects (mouse, 131 kb windows), the zero-shot correlation between predicted and observed junction effects:

| Setup | Correlation |
|---|---|
| Port under autocast | 0.135 |
| JAX reference | 0.167 |
| Port in float32 | 0.165 |

## Fix

`_apply_rope` in the junction head now indexes the embeddings, casts them and the scale/offset parameters to float32, and applies RoPE with autocast disabled, as the JAX head does. The donor-acceptor einsum after it stays under autocast, which matches JAX's `BF16_BF16_F32` precision there.

## Test

`test_autocast_matches_float32_at_large_positions` compares CUDA bfloat16 autocast with float32 output at positions near 2**20. It is skipped without CUDA because CPU autocast keeps this einsum in float32.

- Before the fix, 30 of 72 outputs mismatch and the largest relative difference is 0.24.
- After the fix, it passes.
- `tests/unit/test_heads.py` and `tests/unit/test_attention.py` pass.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `72268c011d37`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.