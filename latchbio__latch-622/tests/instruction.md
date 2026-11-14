# Issue

**Title:** Remove direct-upload Nextflow registration

## Description

## Summary
Remove the native Nextflow registration command completely.
Remove its options, uploader implementation, and unused imports.
Do not retain a retirement handler or migration-message stub.
Keep Flyte-backed registration through `latch register --nf-script`.
Keep the other Nextflow subcommands unchanged.

## Validation
Python syntax and diff checks passed.
AST checks confirm command removal and preservation of the remaining commands.
An isolated Click check confirms unknown-command behavior.
No authenticated registration or production validation occurred.
No tests were committed.

Review coordinator:

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e068e6bb167c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.