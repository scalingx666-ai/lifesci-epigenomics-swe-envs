# Issue

**Title:** Update NEWS.md to 6.38.0 and What's New to November 2026

## Description

> **Merge after #419 and #421.** Both files list their changes.

## Summary

- `NEWS.md`: add versions 6.20.0 to 6.38.0. The file stopped at 6.18.0 in 2021, so `news(package = "mixOmics")` and the Bioconductor release announcements have shown no changes since then. Each change is listed under the release that first included it, found by comparing the Bioconductor `RELEASE_3_xx` branches.
- `inst/README-WhatsNew.Rmd`: add August to November 2026.

## Test plan

- [x] `tools:::.build_news_db_from_package_NEWS_md("NEWS.md")` reads all 16 versions, 6.8.0 to 6.38.0
- [x] All new issue and PR links open

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6009ca6bb734`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.