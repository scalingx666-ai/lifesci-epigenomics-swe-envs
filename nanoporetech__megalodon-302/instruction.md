# Issue

**Title:** use help command to test samtools

## Description

This PR updates the samtools test to use the command `samtools help`. This command fulfills the purpose of sanity checking the installation of SAMtools in a more reliable way than `samtools sort` and `samtools index`:
* `samtools sort` can cause a false negative on correct installations under some conditions, such as the environment inside a computational job in our HPC cluster (Slurm). In those cases `samtools sort` fails with an exit code 1 and the output
    ```
    samtools sort: failed to read header from "-"
    ```
* `samtools index` always returns an exit code 1

This PR also limits the code in `try` to the execution of SAMtools and moves all code that should only be executes if the `try` is successful (*i.e.* the command `samtools` exists) into the `else` statement.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6acab40c72ac`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.