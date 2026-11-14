# Issue

**Title:** Clustering information for NGSplot

## Description

Clustering information is now stored in the RData file
This can be Extracted using ExtractGName.R, which has been rewritten to work with multiple plots

ExtractGName.R now works with both ngsplot.R and replot.R (replot doesn't zip contents)
zip file, Rdata file or root name of zip file can al be supplied to ExtractGname.R on command line

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4748056dd104`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.