# Issue

**Title:** fix: remove compromised emSign trust

## Description

## Summary
- update all locked certifi copies to 2026.10.19, removing trust in the compromised emSign chain
- update pinned uv container images to 0.13.4 where present
- preserve all unrelated dependency versions

## Security context
Addresses GHSA-6q48-6c7w-9gw2 (certifi) and GHSA-hq85-5cpj-mr62 (uv). Older releases trust an emSign root whose intermediate CA private keys were compromised, allowing an on-path attacker to impersonate HTTPS services.

## Validation
- lockfile diffs are limited to the certifi package stanza
- no certifi version below 2026.10.19 remains
- no pinned uv image below 0.13.4 remains
- affected lockfiles were checked with uv lock --check --offline where their existing dependency graph is current

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b3768e65c6d4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.