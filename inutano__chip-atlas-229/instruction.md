# Issue

**Title:** Fall back to plain .bed when no .bed.gz exists

## Description

## Summary

Since #227, assembled BED links point to `.bed.gz` for all genomes. The data server audit () found older entries in hg19, mm9, dm3 and ce10 that have only a plain `.bed`, so those links return 404.

This PR changes `PJ::Location#archived_bed_url` (used by Peak Browser "View on IGV" and "Download") to:
1. Check for `<file>.bed.gz` with a HEAD request.
2. Return `<file>.bed` if the check gets a 404, and `<file>.bed.gz` otherwise.
3. Handle stalls: the data server occasionally stalls on connect, so the check uses `open_timeout` 1s / `read_timeout` 3s and up to 3 attempts. If all attempts fail, it assumes the `.bed.gz` exists, which is the standard format.

The removed `fileformat` method was only used here.

## Testing (local server)
- `/download` for every entry that lacks `.bed.gz` (from the audit):

  | genome | entries | returned `.bed` |
  |---|---|---|
  | dm3 | 860 | 860 |
  | ce10 | 67 | 67 |
  | hg19 | 219 | 219 |
  | mm9 | 170 | 163 (see below) |

- 220 other entries (20 per genome across all 11 genomes) all returned `.bed.gz`, and the files exist.
- An earlier revision also tested `/browse` on a sample, with the same results.
- Latency: median about 0.08s per request (one HEAD).
- `smoke_test.sh` steps 1–7 (WABI job submission excluded): 20/20 passed.

## Known limitation
The data server sometimes refuses connections for short periods.
- Paced at one request per 0.5s, 7 of 170 mm9 entries (0.7%) failed all three attempts and got `.bed.gz`, which is 404 for those entries.
- Under burst load (continuous requests with no delay) the failures were more frequent.
- While the server is in that state, the user's own download is also likely to fail, so this change does not make things worse.
- Creating the missing `.bed.gz` files on the data side would remove the need for this fallback.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b85b7c02545e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.