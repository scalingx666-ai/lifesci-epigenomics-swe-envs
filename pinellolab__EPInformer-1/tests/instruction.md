# Issue

**Title:** Correct running errors while reproducing the paper results

## Description

Hi, thanks for your great work. I successfully reproduced your results by correcting some misleading codes. I correct them and feel free to merge if you think they are correct.

Moreover, I have a question about the selection of promoter and enhancer. I notice that you have input shape as (61,2000,4), and 2000 is the sequence window, is it correct? How to select the window length? Thanks.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `43b0fd07cbbd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.