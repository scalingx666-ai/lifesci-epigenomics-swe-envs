# Issue

**Title:** Rewrite circosPlot drawing helpers without OmicCircos code

## Description

## Summary

The drawing helpers of `circosPlot()` were taken or adapted from the OmicCircos package (v1.6.0), which is licensed GPL-2 only. Code under GPL-2 only can't be distributed under GPL-3 or any later licence, so it ties mixOmics to GPL-2.

- Remove the code that came from OmicCircos, all in `R/circosPlot.R`: `segAnglePo`, `bezierCurve`, `bez`, `scale.v`, `do.scale`, the `draw.*` functions, and `drawIdeogram`, `drawLinks` and `drawLinePlot`, which were adapted from `OmicCircos::circos()`. Six of them were never called (`draw.link2`, `draw.link.pg`, `draw.point.w`, `draw.text.w`, `draw.line`, `draw.line2`), and two only behind options that were never turned on (`draw.arc.pg`, `do.scale`).
- Add new internal helpers that draw the same plot: `.circos_layout()` computes the angle of each block and feature once, and `.circos_draw_blocks()`, `.circos_draw_links()` and `.circos_draw_expression()` draw the bands and labels, the links and the expression lines.
- Fix block names that contain "chr". The old code removed every "chr" from the block names, not only the prefix it added: a block named `chromatin` was labelled `omatin`, and `line = TRUE` failed with `dims [product 1] do not match the length of object [0]`.
- Fix `blocks.link` with several blocks. `genLinks()` compared the block of each feature with `blocks.link` using `==` instead of `%in%`, so R recycled `blocks.link` and kept only about half of the features. Even listing every block dropped some of the links. A new test covers this.

The arguments and return value of `circosPlot()` don't change.

## Plots

The plots are identical to master, pixel for pixel, in 25 scenarios that cover the help page examples, the tests and the arguments (`line`, `showIntraLinks`, `linkWidth`, colours, `var.adj`, `block.labels.adj`, `blocks`, `blocks.link` with one block, `var.names`, `block.spls` with `group`, `indY`, repeated feature names, one selected feature per block, more than 6 blocks, missing values, `par(mfrow)`). `circosPlot()` also returns identical similarity matrices.

<img width="500" alt="1-tcga_line" src="https://github.com/user-attachments/assets/1ab9c316-0796-490b-a389-610afbed9786" />

Only the plots affected by the two fixes change (click to enlarge):

<img width="32%" alt="2-chr_noline" src="https://github.com/user-attachments/assets/5a05a716-45cf-459d-83da-a7de43e82fc7" /> <img width="32%" alt="3-chr_line" src="https://github.com/user-attachments/assets/ff0ccc36-3652-4904-8d20-ed427de12316" /> <img width="32%" alt="4-blocks_link_two" src="https://github.com/user-attachments/assets/9bc4950b-0760-48b5-bb3a-109a2ffa18a3" />

The images above are cairo PNGs at 150 dpi; cairo PNGs at 72 and 300 dpi and quartz PNGs give the same result. PDF and SVG output is visually identical but not byte-identical. The old code drew a zero-length segment at the start of each expression line. In SVG, some label rotations are now an equivalent angle 360° apart, and angles are no longer rounded to 15 significant digits, so a few numbers differ in their last digits.

The comment below has a self-contained page with the 28 scenarios, and the scripts that produce it.

## Test plan

- [x] `tests/testthat/test-circsPlot.R` passes, including the new `blocks.link` test
- [x] Compare the plots with master in 28 scenarios (see above)
- [x] R CMD check (CI)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7e5939875fc8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.