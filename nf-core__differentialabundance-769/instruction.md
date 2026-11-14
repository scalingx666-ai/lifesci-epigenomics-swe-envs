# Issue

**Title:** Fix tower.yml report patterns for per-paramset output folders

## Description

## What's broken

On 2.0.0, the Reports list in a run's Outputs tab on Seqera Platform is missing the main HTML report and the differential tables. With `-profile rnaseq` it only showed the exploratory/differential plots and `pipeline_info`.

## Why

Outputs now go under a per-paramset subfolder (e.g. `report/rnaseq/`), and most `tower.yml` patterns still expect the files one level up. `processed_counts` was also renamed to `processed_abundance`.

## Fix

Add the paramset level to each pattern that was missing it. I used `*/` for that level rather than `**/`, because `report/gsea/<paramset>/<contrast>/<gene_set>/` holds the GSEA HTML, so `**/report/**/*.html` would pull in ~150 GSEA pages across the test profiles. I also made the table display names method-neutral, since these tables come from limma, dream and propd too.

| Pattern before | Pattern after | Example real path |
| --- | --- | --- |
| `**/report/*.html` | `**/report/*/*.html` | `report/rnaseq/study_differentialabundance_report.html` |
| `**/report/*.zip` | `**/report/*/*.zip` | `report/rnaseq/study.zip` |
| `**/tables/differential/*.tsv` | `**/tables/differential/*/*.tsv` | `tables/differential/rnaseq/<contrast>.deseq2.results.tsv` |
| `**/tables/processed_counts/*.tsv` | `**/tables/processed_abundance/*/*.tsv` | `tables/processed_abundance/rnaseq/all.vst.tsv` |
| `**/plots/qc/*.dispersion.png` | `**/plots/qc/*/*.dispersion.png` | `plots/qc/rnaseq/<contrast>.deseq2.dispersion.png` |
| `**/shinyngs_app/data.rds` | `**/shinyngs_app/**/data.rds` | `shinyngs_app/rnaseq/study/data.rds` |
| `**/shinyngs_app/app.R` | `**/shinyngs_app/**/app.R` | `shinyngs_app/rnaseq/study/app.R` |

The plot patterns and `pipeline_info/**` already matched and are unchanged.

## Verification

Checked old and new patterns with Java's NIO glob matcher against the output paths in every pipeline-level nf-test snapshot (rnaseq deseq2/limma/dream/propd, affy, maxquant, soft, generic_matrix, nogtf, many) plus a real 2.0.0 `-profile rnaseq` output listing. Every changed pattern matched nothing before; afterwards each matches only its intended files, one report HTML and zip per paramset, and none of the GSEA pages.

Also ran `-profile test` on Seqera Platform, once with 2.0.0 and once with this branch, same settings. Both runs wrote the same files under `report/rnaseq_deseq2_gsea/`, `tables/...` and so on. The Reports list differed:

| Reports entry | 2.0.0 | This branch |
| --- | --- | --- |
| Final differential abundance report (HTML) | 0 | 1 |
| Report bundle (zip) | 0 | 1 |
| Differential abundance table | 0 | 6 |
| Processed abundance table | 0 | 2 |
| DESeq2 dispersion plot | 0 | 2 |
| Shiny app (`data.rds`, `app.R`) | 0 | 2 |
| Exploratory / differential plots (PNG) | 8 | 8 |
| Pipeline run info | 5 | 5 |
| **Total** | **13** | **27** |

No GSEA pages are listed. (Both runs used a small `QUARTONOTEBOOK` config override that Fusion needs for Quarto to render. It has nothing to do with this change.)

## PR checklist

- [x] This comment contains a description of changes (with reason).
- [x] `CHANGELOG.md` is updated.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b54dae450a86`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.