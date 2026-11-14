# Issue

**Title:** Tests cpp interface

## Description

adds line // [[Rcpp::interfaces(r, cpp)]] in src/dist_matrix.cpp and then run devtools::document() -- this adds the rest of the files exporting all cpp functions in the cpp file

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9e171b253f76`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.