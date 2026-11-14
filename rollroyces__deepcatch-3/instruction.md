# Issue

**Title:** 📄 Paper + strand scoring fix + validation docs (v2.2.0 final)

## Description

## What this adds (8 commits on top of the merged PR #2)

### 📄 The paper (bioRxiv-ready)
- `paper/PAPER.md` — complete manuscript: Abstract, Intro, Methods, Results, Discussion, Conclusions, References
- `paper/paper.tex` — LaTeX version (compiles on bioRxiv server-side)
- All numbers cross-verified against `results/real_tcga_validation.json`

### 🎯 Strand scoring fix (verified)
- Z-score (Normal) approximation to binomial test replaces old 2×min/max formula
- Correctly handles low-read-count positions (1-2 reads) where strand bias is undetectable
- **+0.011 AUC at 0.1% ctDNA** (0.820 → 0.831)
- 10× faster (6 min → 7.5s for panel detection)

### 🧬 Real-data validation work
- GSE185307 cfDNA methylation: genome-wide AUC 0.357 (coverage-confounded) → **CpG-island-aware AUC 0.738**
- `validation/cfdna/validate_gse185307.py` + `validate_gse185307_regions.py` (reproducible)

### 📚 Docs for community & impact
- `REVIEWERS.md` — call for expert review (10 named researchers)
- `docs/PATH_TO_IMPACT.md` — 6-month plan to clinical validation (no institution needed)
- `docs/PREPRINT_ABSTRACT.md` — bioRxiv abstract
- `NEXT_STEPS.md`, `data/JIANG_DATA_REQUEST.md`, `data/FLARE_CONTACT_TEMPLATE.md`

### ✅ Verified
- 228/228 tests green
- README synced to verified JSON (strand 0.831, Sens95 0.770)
- Release v2.2.0 tagged at this exact commit

**Note:** main has `paper/deepcatch_final.tex` (from PR #1). This PR adds `paper/PAPER.md` + `paper/paper.tex` — no filename conflicts, both can coexist. The release notes at https://github.com/rollroyces/deepcatch/releases/tag/v2.2.0 reference the paper files in this PR.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3244e9f976ac`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.