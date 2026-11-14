# Issue

**Title:** updated build system and deprecated modules

## Description

## Description

No change in functionality. Did the following: 
* Update the build system to pyproject.toml 
* Remove six and joblib dependencies to keep deps lean 
* deprecated polymerutils in favor of HDF5_format and issued deprecation warnings, getting ready for removing polymerutils.
* Added dependensies and build deps to the build systems
* Made a warning about installing openmm (it's not a dep because openmm vs openmm[cuda13] are options 
* Bumped max python version to 3.13 (it technically works on 3.14 pending that mp method is set to fork)

Black and better py314 compatibility are coming in the next PR. 


## PR Checklist

- [] apply "black" to the whole codebase (`black .`) 
- [] apply isort to the codebase (`isort .`)
- [] fun `flake8` and try to resolve all the issues (work in progress!)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `320f25e97a27`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.