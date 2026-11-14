# Issue

**Title:** CaviTracer - visualization for PyMol and ChimeraX

## Description

PyMol visualization was already done by Jan from the calcChannels() level (PR2278), but since I created writeVmdCaviTracerScript() in PR2279 to provide a TCL script with automatic visualization of the CaviTracer results, I decided to create analogous functions for both PyMol (using functions created by Jan) and Chimera.

In this pull request, we have two functions:
- writePyMolCaviTracerScript - generate pdb, cif, and PY file to open CaviTracer objects (channels, pores, etc.) in PyMol
- writeChimeraXCaviTracerScript - generate pdb, pqr, and cxc files to visualize CaviTracer objects in ChimeraX


<img width="2548" height="1439" alt="cavitracer_figure42" src="https://github.com/user-attachments/assets/a69a0644-c7e8-4892-b335-bd1ad2fc0579" />

<img width="2548" height="1439" alt="cavitracer_figure41B" src="https://github.com/user-attachments/assets/4b5db411-9de5-44d6-b900-d8a3b04fd2bf" />

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1f52bbecd359`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.