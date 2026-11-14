# Issue

**Title:** docs(bench): say what the nutrigx-advisor 0.0 baseline measures

## Description

## Summary

- Adds a paragraph to the `_comment` block of `bench_baseline.json` that says what the `nutrigx-advisor: 0.0` entry is: the pinned harness runs `skills/nutrigx_advisor/nutrigx_advisor.py`, a path that has not existed since the rename in `2725981`, so the ten verdicts describe a script that never started.
- No number changes, and the gate behaves exactly as before.

. This is option 3 from .

## Skill affected

None. The change is to `bench_baseline.json` only; the note concerns `nutrigx`.

## Why

The file already explains the two other entries that are not what they seem (clawbio-finemapping and pharmgx-reporter). The nutrigx entry had no note, and its ten verdicts (`score_incorrect` ×4, `snp_invalid` ×3, `threshold_mismatch` ×2, `repro_broken` ×1) read as ten scientific failures. They are one missing path, and a gate at 0.0 cannot regress, so the skill has had no bench coverage since June.

The note also says when to re-record: once CI pins a bench revision that resolves the skill path. biostochastics/clawbio_bench#187 does that and is waiting on review.

Not in this PR: the path fix itself (bench side), a compat shim under `skills/` (option 2 in the comment), and the pin bump.

## Evidence

- **Reproduced live (CI):** `scientific-audit` on `main` `a4a8c72` ([run 43182422784](https://github.com/ClawBio/ClawBio/actions/runs/43182422784)) grades nutrigx-advisor 0/10 with those four categories. In the verdict artifact of the run on `fa206fb` ([42852181777](https://github.com/ClawBio/ClawBio/actions/runs/42852181777)) all ten cases have the same stderr: `python: can't open file '.../skills/nutrigx_advisor/nutrigx_advisor.py': [Errno 2] No such file or directory`. I did not download the `a4a8c72` artifact; nothing under `skills/nutrigx*` or the bench pin changed between the two commits.
- **Read-only (bench at the pinned revision `49a0f11`):** `nutrigx_harness.py:511` builds the path as `repo_path / "skills" / "nutrigx_advisor" / "nutrigx_advisor.py"`. Lines 294-300 return `harness_error` for an unexpected non-zero exit only when stderr is a traceback (`_is_genuine_crash`); "can't open file" is not one, so the cases fall through to the rubric and `harness_errors` stays 0.
- **Read-only (this repo):** `fdd781e` (3 May) added `skills/nutrigx_advisor` as a compat symlink for the bench; `2725981` (11 Jun) renamed the skill to `skills/nutrigx/` and removed it.

One correction to my comment on #106: there I wrote that the bench scores "a non-zero exit as a tool failure, not a harness error". The exact rule is the one above (a harness error needs a traceback), and the note in this PR uses it.

## Checklist

- [x] I have searched for existing PRs
- [ ] SKILL.md: not applicable, no skill is changed
- [x] Tests pass. No test is added: the change is a comment, and `tests/test_bench_baseline.py` already checks that the committed baseline loads and keeps its schema
- [ ] Demo data: not applicable
- [x] I have not committed any patient/sensitive data
- [x] I have followed the CONTRIBUTING.md conventions

## Test output

```text
uv run pytest tests/test_bench_baseline.py -q
12 passed            # Python 3.14

# gate against the aggregate report of CI run 43043560118
uv run python scripts/check_bench_baseline.py aggregate_report.json --baseline bench_baseline.json
| nutrigx-advisor | 0.0% | 0.0% | 0.0 |
No regression against bench_baseline.json.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a4a8c7281c8d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.