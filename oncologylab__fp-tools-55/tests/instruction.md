# Issue

**Title:** Simplify and harden the fp-tools GUI

## Description

## Summary
- simplify the GUI home page and centralize concise field labels
- move secondary settings into Advanced options and clear machine-specific fresh defaults
- improve responsive layout, mobile navigation, input contrast, disabled actions, and validation messages
- strengthen desktop audits for neutral initial forms and collapsed advanced controls
- prepare v0.2.0rc14 metadata and download links

## Verification
- 416 passed, 1 skipped
- all 21 console commands passed help smoke tests
- strict MkDocs build and 32-page/3-viewport docs audit passed
- GUI pages passed at 1920, 1280, and 390 px with no exceptions, hidden labels, or horizontal overflow
- mobile sidebar, validation layout, YAML synchronization, and launch-state transitions passed
- sdist and twine checks passed

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a3df162f75b6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.