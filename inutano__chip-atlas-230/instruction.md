# Issue

**Title:** Remove node switch note from EA pages and update what's new

## Description

## Summary
Changes requested in review comments.

- Removed this note from the Enrichment Analysis input page (`views/enrichment_analysis.haml`) and result page (`views/enrichment_analysis_result.haml`):
  > Note: Jobs submitted before April 2026 may not complete due to a node switch. Please re-submit your job …
  - The production server had these lines removed by hand, as an uncommitted change on the AMI. The removal here is the same: the new `views/enrichment_analysis.haml` has the same blob hash (88a0fb3) as the production file.
- What's new: `Added new genome assembly **A. thaliana**, welcome!` → `Added new genome assembly **TAIR12** of **A. thaliana**, welcome!`

## Testing
- Both haml files compile.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b85b7c02545e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.