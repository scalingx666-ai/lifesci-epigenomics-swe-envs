# Issue

**Title:** fix(fine-mapping): make SuSiE-inf's est_tausq path follow the reference

## Description

## Summary

`run_susie_inf(est_tausq=True)` departed from SuSiE-inf in three places. This PR brings all three back to the reference implementation (FinucaneLab/fine-mapping-inf, read through the gentropy port the bench vendors):

- **The τ² estimate was discarded.** It was applied to the variance structure only when it reached 1e-3. On per-SNP scale a real estimate is 1e-5 to 1e-4, so it never was, and the fit stayed plain SuSiE while reporting a non-zero `tausq`.
- **PIPs were not aggregated over all L effects.** With `est_tausq=True`, effects with a small prior variance and a flat α were dropped first. A null locus lost every effect and came back as a uniform `1/p`.
- **A non-positive moment solution kept the τ² it came in with.** The reference sets τ² to 0.

`est_tausq` still defaults to `False`, and on that path nothing changes: no pruning ran there, and τ² stays at its starting value. The CLI never calls `run_susie_inf`, so no report changes either.

 (`fm_20_susie_inf_est_tausq_activation`). **This PR does not move the bench number by itself**; the reason is in the next section.

## Skill affected

`fine-mapping`

## What `fm_20` measures today

`fm_20` fails in CI with `tausq = 0.0`. The cause is upstream of this module: the bench driver builds the arguments for `run_susie_inf` without `est_tausq` (`drivers/finemapping_driver.py:304-321` at the pinned revision `49a0f11`; bench `main` is the same), so ClawBio runs every SuSiE-inf case with its default, `False`. `fm_20` and `fm_21` share one locus and differ only in `est_tausq`, so they reach ClawBio as identical calls. No change on this side can pass both.

Forwarding the flag would have exposed the three defects above. I applied the harness's pass rule by hand to the five `susie_inf` cases, calling `run_susie_inf` as the driver does and then with `est_tausq` forwarded from each case's `inputs.json`:

| `run_susie_inf` called | `main` `a4a8c72` | this branch |
|---|---|---|
| as the pinned driver calls it | 4/5 (`fm_20` fails) | 4/5 (`fm_20` fails) |
| with `est_tausq` forwarded | 2/5 (`fm_17`, `fm_18`, `fm_20` fail) | 4/5 (`fm_17` fails) |

So the bench number is unchanged by this PR (first row), and the module is ready for a driver that forwards the flag (second row).

On the three cases whose expectation was derived from the reference (`derived/oracle_expected.json`), with the flag forwarded:

| Case | | Reference | `main` | this branch |
|---|---|---|---|---|
| `fm_20` (τ² on) | τ² | 5.464e-05 | 2.252e-05, not applied | 5.464e-05 |
| | σ² | 0.989078 | 0.994328 | 0.989078 |
| | PIP, outlier / other | 0.02168 / 0.02062 | 0.15610 / 0.01955 | 0.02168 / 0.02062 |
| `fm_18` (null locus, τ² on) | τ² | 0 | 0 | 0 |
| | PIP | 0.3847 | 0.2000 | 0.3847 |
| `fm_21` (τ² off) | σ² | 0.998841 | 0.998841 | 0.998841 |
| | PIP, outlier / other | 0.15426 / 0.01956 | 0.15426 / 0.01956 | 0.15426 / 0.01956 |

`fm_17` has no reference file. Its expectation is hand-set (`EXPECTED_TAUSQ: 0.0` in a case that asks for `est_tausq`), and with the flag forwarded it fails on `main` and on this branch alike. I did not run the reference on it, so I cannot say what its expectation should be.

## What this reverses

The floor and the pruning both came in with `237cbd9`. The floor's comment says tiny τ² estimates degrade the single-effect fits. On the `fm_20` locus the reference applies τ² = 5.5e-05 and the outlier's PIP goes from 0.154 to 0.022, which is the behaviour the method exists for; with the floor the answer is 0.156. If there was a locus where applying a small τ² gave a worse fit than the reference does, I would like to see it, because the tests here do not cover it.

## Not in this PR

- **Bench:** forward `est_tausq` in the driver and re-derive `fm_17` from the reference. That belongs in biostochastics/clawbio_bench; I can open it.
- **The default.** No caller turns `est_tausq` on. `tests/benchmark/finemapping_benchmark.py` calls `run_susie_inf` with defaults, so its "SuSiE-inf" arm is fitted without an infinitesimal component. Flipping the default is a separate decision, and with the pinned driver it would turn `fm_17` and `fm_21` red.
- **Overlap:** #782 also edits `skills/fine-mapping/SKILL.md`, in the Output Structure section; this PR edits the SuSiE-inf steps and the Gotchas list. The hunks do not touch, so I expect the two to merge cleanly in either order.

## Evidence

- **Reproduced synthetic:** every number in the tables above, from `run_susie_inf` on the bench's `inputs.json` files, read locally. `clawbio-bench` itself was not run.
- **Read-only:** the reference `_MoM` (`scripts/_reference/gentropy_susie_inf.py:311-316` in the bench: `(sigmasq, tausq) = (float(x[0] / n), 0.0)` on a non-positive solution) and `var = tausq * Dsq + sigmasq` with no floor (line 252); the driver lines cited above.

## Checklist

- [x] I have searched for existing PRs
- [x] SKILL.md updated: the SuSiE-inf method steps, and a new Gotcha on reading `tausq`. Frontmatter unchanged; `generate_catalog.py` produces no diff
- [x] Tests included and passing, with a Hypothesis property (PIP equals `1 - prod(1 - α)` over all L effects for any locus, with τ² estimated or not)
- [x] Demo data unchanged
- [x] I have not committed any patient/sensitive data
- [x] I have followed the CONTRIBUTING.md conventions

## Test output

```text
uv run pytest skills/fine-mapping/tests -q
119 passed            # Python 3.12, 3.13 and 3.14; 112 before this PR

uv run pytest tests -q
488 passed, 2 skipped, 2 xfailed            # Python 3.14

uv run pytest clawbio/tests clawbio/common/tests -q
740 passed            # Python 3.14

uv run agentskills validate skills/fine-mapping
Valid skill: skills/fine-mapping
```

The seven new tests are in `tests/test_susie_inf_tausq.py`. Six failed before the change; the seventh pins the `est_tausq=False` numbers, which do not move. The loci are rebuilt in the test from each case's prose description, and the expected values are the reference's, cited in the module docstring. No bench file is copied.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a4a8c7281c8d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.