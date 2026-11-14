# Issue

**Title:** Add `id` global

## Description

Hi there, we are working on the next version of dplyr and your package was flagged in our reverse dependency checks.

Your package does one of two things:

- It re-exports `dplyr::id()`, which has been defunct for many years and has now been removed from dplyr.

- It references a column named `id`, likely in a `mutate()` or `summarise()`, but does not note this as a global variable with `utils::globalVariables("id")`. In this case, you got lucky that dplyr exported `id()`, meaning that you did not need a global variable for `"id"`. Since we have removed `dplyr::id()`, your package will need this now.

dplyr will be released on January 31, 2026. If you could please send an update of your package to CRAN before then, that would help us out a lot! Thanks!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `672e79d64973`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.