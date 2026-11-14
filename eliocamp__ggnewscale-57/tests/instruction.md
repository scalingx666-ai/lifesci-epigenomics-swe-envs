# Issue

**Title:** Compatibility with future release ggplot2

## Description

Hi Elio,

In the development version of ggplot2, there has been a large rewrite of the guide system.
Unfortunately, that means that ggnewscale sometimes looked into places that have moved location.
This PR aims to make ggnewscale compatible with the current development branch of ggplot2.
The unit tests pass with both CRAN ggplot2 and dev branch ggplot2.

Best wishes,
Teun

A small demonstration. This PR with CRAN ggplot2:

``` r
devtools::load_all("~/packages/ggnewscale/")
#> ℹ Loading ggnewscale
library(ggplot2)

ggplot(mtcars) + aes(mpg, disp) +
  geom_point(aes(colour=factor(cyl)),  size=7) +
  scale_colour_brewer(type='qual', guide = guide_legend(order = 0)) +
  new_scale_colour() +
  geom_point(aes(colour=factor(gear)), size=3) +
  scale_colour_brewer(palette='Set1', guide = guide_legend(order = 0))
```

![](https://i.imgur.com/5wHB32i.png)<!-- -->

This PR with development ggplot2:

``` r
devtools::load_all("~/packages/ggplot2/")
#> ℹ Loading ggplot2
packageVersion("ggplot2")
#> [1] '3.4.2.9000'

ggplot(mtcars) + aes(mpg, disp) +
  geom_point(aes(colour=factor(cyl)),  size=7) +
  scale_colour_brewer(type='qual', guide = guide_legend(order = 0)) +
  new_scale_colour() +
  geom_point(aes(colour=factor(gear)), size=3) +
  scale_colour_brewer(palette='Set1', guide = guide_legend(order = 0))
```

![](https://i.imgur.com/DndFctl.png)<!-- -->

<sup>Created on 2023-06-28 with [reprex v2.0.2](https://reprex.tidyverse.org)</sup>

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d5b083687ede`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.