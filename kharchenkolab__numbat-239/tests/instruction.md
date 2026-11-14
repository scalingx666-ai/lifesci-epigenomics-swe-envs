# Issue

**Title:** cnv_state check in segs_consensus_fix

## Description

I had a typo in my segs_consensus_fix file and numbat ran without error, but silently set the prior to 0 so the event was never called in the per-cell data. This PR adds a check to ensure that the cnv_state field in segs_consensus_fix contains only valid states.

If run with a segs_consensus_fix file that uses `gain` instead of `amp`, you will get the following error. 
```
Error in check_segs_fix(segs_consensus_fix) :
  Invalid cnv_state values: gain
  Allowed values are: neu, amp, del, loh, bamp, bdel
Calls: run_numbat -> check_segs_fix
Execution halted
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `93b1369f3d1d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.