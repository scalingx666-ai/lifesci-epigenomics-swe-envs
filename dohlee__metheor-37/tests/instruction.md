# Issue

**Title:** Fix reference-skip and deletion-edge XM tagging

## Description

## Summary

. Fix reference-skip and deletion-edge XM calls, and document/test the five reported indel-rich CIGARs.

```text
N                → advance the reference cursor
internal D gaps  → skip deleted bases when collecting context
reference flanks → retain both flanks when collecting context
query bases      → retain one XM position per query base
```

Preserve the existing insertion and soft-clip policies and the shared serial/threaded checked-output path.

## Evidence

- **Before:** `1M1N3M`, read `CGCG`, reference `CAGCGTCC` yields incorrect forward `X...`.
  **After:** `Z.Z.` forward and `.Z.Z` reverse, in SAM and BAM.
- **Before:** `1M1N1M1D1M`, read `ACA`, reference `AGCTAGC` can produce shortened `..`.
  **After:** correct `.X.`; five independent deletion-edge counterexamples pass in SAM and BAM.
- All five reported complex CIGARs on both strands match literal [Bismark v0.24.2 caller](https://github.com/FelixKrueger/Bismark/blob/acf965cdf9e0a2814f09c087811683c1651c36c1/bismark#L4772) expectations with Metheor's existing soft-clip/flank policy retained. These ten reported-CIGAR cases already passed before this patch.
- Independent oracle comparison after the context fix: all 400 cases match, fixing all four initially found edge failures. No original reporter BAM was supplied.
- Latest-master integration `5dd61a1` exactly matches the independently reviewed final tree `9586146`. All 227 test executions, strict Clippy, formatting and whitespace checks pass. Three-worker stdout/N+D/order/downstream-failure probes pass.
- Final integration re-review: **Ship**. Actual-head probes pass for native calls with overlap/target/reference filtering, PM counts and ME values; 4,103 mixed N/D records preserve order/CIGAR, emit `.X.`, and produce identical finalized BAM stdout with one/three workers. All eight hosted checks passed for updated head `5dd61a1` before merging.

## Merge Danger

**Door:** two-way

**Blast Radius:** tagging

Calls change for the confirmed reference-skip and deletion-edge bugs. Context across deletions/skips follows the remaining aligned sequence, as described in [Bismark's context FAQ](https://felixkrueger.github.io/Bismark/faq/changing-context/). The reporter's original BAM remains unverified.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `15434b58df35`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.