# Issue

**Title:** Respect column names in mutate

## Description

This pull request ensures non-standard column names are respected when calling `mutate_mcols()`. The call to  `S4Vectors::DataFrame` will use `check.names = TRUE` which calls `make.names()` and non-standard names are modified as a side-effect (see [here](https://github.com/Bioconductor/S4Vectors/blob/devel/R/DataFrame-class.R#L365)). This suggestion ensures column names as passed are restored.

``` r
library(plyranges)
GRanges("chr1:1") |> mutate(`A+B` = 1)
```

In addition, I have removed the logical test `if (is.null(mcols(.data)))` as it is redundant. The mcols element cannot be `NULL` but is instead a `DataFrame` with 0 columns. I'm not sure if that's a new introduction but it seems to be the way it is & I can't enforce a `NULL` any way I try to.

``` r
GRanges("chr1:1") |> mcols()
```

I'm not sure if there are downstream classes which modify this behaviour though.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c2ff9bc597c8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.