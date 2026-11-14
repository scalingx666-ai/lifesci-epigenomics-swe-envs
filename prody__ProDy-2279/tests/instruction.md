# Issue

**Title:** VMD visualization of CaviTracer

## Description

I created the writeVmdCaviTracerScript() function, which creates a VMD state for the results obtained by CaviTracer.

How to use it:
from prody import *
p = parsePDB('1tqn')
atoms = p.select("protein")

# For channels:
channels, surface = calcChannels(atoms)
writeVmdCaviTracerScript(channels, atoms)

# Surface cavities:
cavities, surface = calcSurfaceCavities(atoms)
writeVmdCaviTracerScript(cavities, atoms, object_type='surface_cavities', surface=surface)

It will also work for pores and reconstruction of channels+surface cavities.

The user is obtaining a TCL script (vis_channels.tcl for channels, vis_pores.tcl for pores, etc.) that contains a visualization-ready state of the analysis.

To run the script:
vmd -e vis_channels.tcl

Visualization:
<img width="2591" height="1955" alt="cavitracer_figure41" src="https://github.com/user-attachments/assets/f37f04b5-be56-4324-80d4-a754bd4e8111" />

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b18d28502216`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.