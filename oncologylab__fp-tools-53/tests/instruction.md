# Issue

**Title:** Fix GUI rendering, responsive navigation, and config synchronization

## Description

## Summary\n- make every main-panel field label and value explicitly readable across light/native browser rendering\n- refresh every specialized and generic form after loading YAML while preserving unedited config fields\n- restore accessible narrow-screen sidebar navigation and simplify the persistent workspace header\n- expand frozen Windows/macOS browser audits to every GUI destination at desktop and mobile widths\n- prepare the synchronized v0.2.0rc13 release metadata\n\nCloses #50\nCloses #51\nCloses #52\n\n## Local verification\n- 412 tests passed; 10 skipped\n- all console-script smoke checks passed\n- pip check passed\n- run-yaml-workflow dry run passed\n- strict MkDocs build passed\n- sdist build and twine check passed\n- live all-page geometry/label audit passed at 1920, 1280, and 390 px\n\nThe cross-platform frozen-app workflow is the final source of visual screenshots and normal browser-action verification.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4cbdebeaf07a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.