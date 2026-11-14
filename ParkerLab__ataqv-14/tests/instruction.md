# Issue

**Title:** Make the build reproducible

## Description

> Hi,

> Whilst working on the Reproducible Builds effort [0] we noticed that
ataqv could not be built reproducibly.

> This is because it embedded the absolute build path when generating
a manual page:

>│ │ │ │ │  \fB\-t\fR TEMPLATE_DIRECTORY, \fB\-\-template\-directory\fR TEMPLATE_DIRECTORY
>│ │ │ │ │  The location of the web app directory template.
>│ │ │ │ │  (default:
>│ │ │ │ │ -\fI\,/build/1st/ataqv\-1.1\/\fP.1+ds/src/scripts/src/web)
>│ │ │ │ │ +\fI\,/build/2/ataqv\-1.1\/\fP.1+ds/2nd/src/scripts/src/web)

>  [0] https://reproducible-builds.org/

(Original report from @lamby: https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=959714 )

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `468def93c12c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.