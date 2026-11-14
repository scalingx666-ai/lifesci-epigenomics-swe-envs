# Issue

**Title:** Cavitracer multimodel

## Description

Adds multi-model output to CaviTracer and lets the VMD, PyMOL and ChimeraX viewer scripts show it frame by frame.

**New**
- `mergeFramesPQR` merges the per-frame PQR files of a multi-frame run into one multi-model PQR, one MODEL per frame. Frames that found nothing are kept as placeholders, so frames stay in step in every viewer. Chamber links can be merged too (`links=True`).
- `writeVmdMultiModelScript`, `writePyMolMultiModelScript` and `writeChimeraXMultiModelScript` write a viewer script for a PQR on disk. The frames are stepped through with each program's own slider or state controls.

**Changed**
- The VMD, PyMOL and ChimeraX scripts open both a single structure and a multi-model PQR.
- All viewer scripts share one colour palette, so a channel has the same colour in every program.
- `writePyMolCaviTracerScript` can write a PQR (`output_format='pqr'`); mmCIF stays the default.
- ChimeraX scripts set the sphere radii with a single command, so large results load in seconds rather than minutes.
- `calcChannels` and `calcPoresFromChannels` no longer write a PyMOL script automatically; the `write*CaviTracerScript` functions do that.
- Pore files of multi-frame runs are numbered by the frame they come from.

**Example**
```python
channels_all, surfaces_all = calcChannelsMultipleFrames(atoms, trajectory=dcd,
                                                        output_path='frames')  # an existing folder
mergeFramesPQR('frames', 'channels_frames.pqr', frames=range(len(channels_all)))
writeVmdMultiModelScript('channels_frames.pqr', atoms)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b5da31837b13`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.