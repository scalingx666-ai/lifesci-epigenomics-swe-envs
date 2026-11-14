# Issue

**Title:** Compatability with ggplot2 4.0.0

## Description

Hi Elio!

Sorry for the bother, we've been preparing a new major release for ggplot2 and found an issue during a reverse dependency check.
We have a new mechanism in ggplot scales that when `palette = NULL`, which is the default for default scales, then the palette will be pulled from the theme. Because ggnewscale renames aesthetics, the mechanism we use for this doesn't easily transfer to ggnewscale's new scales. This PR fixes this problem, by having fallback to ggplot2's mechanism with the old/original aesthetic name.

It misses one use case, when people have set a local default palette by doing e.g. `+ theme(palette.colour.discrete = <palette>)`. However, I'm assuming most people's workflow with ggnewscale involves actually providing proper scales, so this should fly under the radar.

You can test your code with the development version of ggplot2 by installing it as follows:

```r
# install.packages("pak")
pak::pak("tidyverse/ggplot2")
```

We aim to release the new ggplot2 version in about 2 weeks, and hope you can submit an update to CRAN around that time. Hopefully this will inform you in a timely manner.

Best wishes,
Teun

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0235697b9a3c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.