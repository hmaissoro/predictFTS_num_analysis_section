# RUNLOG

What was actually run in `D:\Projects\predictFTS`, established by reading files on disk on
2026-08-31, and extended on 2026-09-01 when the two real data applications were written. Every entry
names the file it comes from. Anything that could not be established from a file is recorded as
**not established**, with the question it raises.

This is a generated map. It goes stale the moment predictFTS is re-run. Where it disagrees with
disk, disk wins.

Companion file: [ASSETS.md](ASSETS.md), the figure and value reconciliation.

---

## 0. Provenance of the stored results

**The design on disk is the linear exponent.** Not assumed — measured. Three independent readings
agree:

1. `data-simulation/estimates/locreg/dt_mc_locreg_estimates_conso_N=*_lambda=*.RDS` carries a
   `Ht_true` column: 0.48, 0.56, 0.64, 0.72 at t = 0.20, 0.40, 0.60, 0.80. That is
   `0.4 + 0.4t` exactly. `Lt2_true` is 0.01 throughout.
2. The local exponent read directly off the stored noiseless curves of the infeasible benchmark
   sample `data-simulation/generated/dt_mc_FAR_conso_N=5000.RDS` — 5000 curves on the 99-point
   grid, second differences at lags 1 and 2, log-log slope halved — gives Hhat = 0.516 at t = 0.30
   and 0.548 at t = 0.50 against the linear 0.52 and 0.60. Over the range t ≤ 0.6, where the
   second-difference method has not yet saturated, the OLS fit of Hhat on t has intercept 0.404
   against the target 0.400. The superseded estimated exponent quoted at
   `Numerical_analysis-2025-07-v1.tex:125` — H(0.2) = 0.199, H(0.3) = 0.844, H(0.5) = 0.784,
   H(0.8) = 0.445 — is off by 0.24 to 0.84 at those same locations.
3. `scripts/11_simulation_generate.R:57` calls `hurst_linear(t, h_left = 0.4, h_right = 0.8)`, and
   git dates that change to commit `a3984f1`, 2026-08-26 15:58, "Take the local exponent from the
   literature instead of fitting it to the curves". Every file under `data-simulation/generated/`
   is stamped 2026-08-27 03:45, twelve hours later.

**The stored results were produced on the AWS Linux image, and the curve column does not reproduce
bit-for-bit on Windows. This is expected, measured and documented.** Regenerating replication
(N, λ, id_mc) = (150, 25, 1) here, with the current script and the locally installed adaptiveFTS,
using the repo's own probe columns (`aws/reproducibility_probe.R:31`):

| Column | Identical | Max abs diff | sd stored / sd repro |
|---|---|---|---|
| `tobs` | **TRUE** | 0 | 0.289725 / 0.289725 |
| `Eobs` | **TRUE** | 0 | 0.006999 / 0.006999 |
| `process_mean` | FALSE | 1.11e-16 | 0.076987 / 0.076987 |
| `X` | FALSE | 3.84e-01 | 0.113009 / 0.108888 |
| `Yobs` | FALSE | 3.84e-01 | 0.113230 / 0.109108 |

`aws/Dockerfile:19-28` states this outcome in advance, from the same probe: the design (`tobs`) and
the noise (`Eobs`) match bit-for-bit because the seeds are pinned with an explicit `kind`,
`normal.kind` and `sample.kind`; `process_mean` agrees to the last digit or two; and **`X` does not
agree across platforms at all** — "Windows, OpenBLAS and reference each give a different `X`, so
cross-platform bit-identity is not achievable by choosing a BLAS: the residual difference is in
floating-point association inside compiled code, not in the library."

The mechanism is `MASS::mvrnorm` at `adaptiveFTS/R/01_generateFTS.R:257`, which draws the mfBm path
by an eigen-decomposition of the covariance matrix. The `rnorm` draws feeding it are identical; the
orthogonal square root applied to them is not, so the same law is realised along a different path.
That is why `X` moves while every column drawn straight from the RNG stream does not.

Two further checks, both passed. The generator is **internally deterministic**: two runs in one
session give a bit-identical `X`, which is the property `aws/Dockerfile:26-28` relies on when it
says a study generated entirely on one platform is self-consistent. And the generator code is
**unchanged** between the pinned `ADAPTIVEFTS_REF=6acbac5` (`aws/Dockerfile:91`) and the current
working copy `d56b7c7`: `git diff 6acbac5 d56b7c7` touches 33 files, none of them
`R/01_generateFTS.R`, and every changed `src/` file is an estimator.

**So this is not a finding against the results.** The stored tree is self-consistent, carries the
linear exponent, and is the study of record. It simply cannot be re-derived on Windows, by design
of the floating-point world rather than by any defect here. No number below is in doubt.

**Ruling (2026-08-31): the study is taken as reproducible, and reproducibility is not mentioned in
the text.** It is implicit in a paper of this kind — the seeds are pinned, the image is in
`aws/`, and the code is public. The author will verify separately. Nothing in this section is to be
written up, and no reproducibility statement, caveat or platform note goes into either `.tex` file.
This section exists so that a future reader of the repository knows why a local re-run gives a
different `X`, not to raise a question for the paper.

---

## 1. Simulation

### 1.1 Local exponent function

| | |
|---|---|
| Function | `adaptiveFTS::hurst_linear(t, h_left = 0.4, h_right = 0.8)` |
| Closed form | `H_t = min(0.4 + 0.4 t, 1)`, so `0.4 + 0.4t` on [0,1] |
| Defined | `D:\Projects\adaptiveFTS\R\01_generateFTS.R:56-72` |
| Called, Monte Carlo | `scripts/11_simulation_generate.R:56-58`, via `get_local_exponent()` |
| Called, illustrations | `scripts/10_simulation_design.R`, same two-line `get_local_exponent()` |

`hurst_linear` computes `a = (h_right - h_left) / (t1 - t0)` with `t0 = 0`, `t1 = 1`, then
`b = h_right - a*t1`, then `pmin(a*t + b, 1)`. With (0.4, 0.8) that is `a = 0.4`, `b = 0.4`. It is
an analytic ramp. It is **not** estimated from the consumption curves.

Values at the locations the study uses:

| t | 0.1 | 0.2 | 0.3 | 0.4 | 0.5 | 0.6 | 0.7 | 0.8 |
|---|---|---|---|---|---|---|---|---|
| H_t | 0.44 | 0.48 | 0.52 | 0.56 | 0.60 | 0.64 | 0.68 | 0.72 |

### 1.2 One FTS per replication

Yes. `scripts/11_simulation_generate.R:125-159` (`simulate_data_fun`) makes one `simulate_far` call
per `id_mc` and writes one `.RDS` per replication under
`data-simulation/generated/N{N}lambda{lambda}/`. There is no shared sample and no regenerated last
curve.

Each sample carries **both designs on the same curves**: `ttag == "trandom"` for the per-curve
random design, `ttag == "tcommon"` for the shared grid `(1:lambda)/lambda`. Both are observed with
the same noise in `Yobs`; `X` holds the noiseless curve on every row
(`11_simulation_generate.R:147-150`).

### 1.3 Innovation process

From `scripts/11_simulation_generate.R`:

| Quantity | Value | Line |
|---|---|---|
| Process | multifractional Brownian motion, via `simulate_far` | `:132-144` |
| `L2` | `0.01`, so the Hölder constant is `sqrt(L2) = 0.1` | `:19` |
| `intercept_var` | `0.05` | `:28` |
| Per-curve intercept sd | `sqrt(L2 * intercept_var) = sqrt(0.0005)` | `:24-27` |
| Observation noise sd | `0.007`, centred Gaussian | `:22` |
| `n_int_grid` | `100L` | `:142` |
| Burn-in | `100L`, `remove_burnin = TRUE` | `:143-144` |
| `M_n`, random design | uniform on the integers in `[floor(0.8λ), floor(1.2λ)]` | `:48-54` |
| `T_{n,i}`, random design | `runif` | `:135` |
| Common grid | `(1:lambda)/lambda` | `:136` |
| FAR operator norm | `0.5` (`get_far_kenel_conso` default; `op_norm = 1.269403` before scaling) | `:87, :115-117` |

**Discrepancy on the intercept sd.** `sqrt(0.01 * 0.05) = 0.0223607`. The script's own comment at
`:25-26` rounds it to `0.022`; `num_analysis_main.tex:91` states `0.0224`. Both are roundings of
the same number, to different precision and in different directions. Not reconciled here.

### 1.4 Seeding

`mc_seed(N, lambda, mc_i) = as.integer(N*1e6 + lambda*1e3 + mc_i)`,
`scripts/11_simulation_generate.R:43-46`. One seed per replication, not per curve, so the
innovations stay successive draws from one advancing stream. Generators pinned:
`kind = "Mersenne-Twister"`, `normal.kind = "Inversion"`, `sample.kind = "Rejection"` (`:129-130`).
The seed depends on the replication's identity, never on its position in the `foreach` loop.

`scripts/22_simulation_blup.R:97-98` re-seeds with the same integer to displace duplicate `tobs`
values by at most 1e-6, so the displacement is stable across re-runs.

The infeasible benchmark uses a literal seed, `20260801L`
(`scripts/11_simulation_generate.R:211`), N = 5000 being outside the range `mc_seed` encodes.

### 1.5 The grid actually run

Counted from the filenames on disk, not from the intended design. Every count below is *distinct*
`id_mc` values, verified complete over 1..400 with no duplicates.

| Family | Path | Cells | Per cell |
|---|---|---|---|
| Generated samples | `data-simulation/generated/` | 12 | 400 |
| Lag-1 autocovariance | `data-simulation/estimates/autocov/` | 12 | 400 |
| Lag-0 covariance | `data-simulation/estimates/cov/` | 12 | 400 |
| Local regularity | `data-simulation/estimates/locreg/` | 12 | 400 |
| BLUP, independent design | `data-simulation/estimates/blup/random/` | 12 | 400 |
| BLUP, common design | `data-simulation/estimates/blup/common/` | 12 | 400 |
| BLUP fits, both designs | `.../blup/{random,common}/fit/` | 12 each | 400 |
| RP20 export | `data-simulation/estimates/RP20/export/` | 12 | 400 |
| **FACF** | `data-simulation/estimates/facf/` | **9** | 400 |

The twelve cells are `(N, λ) ∈ {150, 300, 600} × {25, 50, 100, 200}`. The FACF study has nine:
λ = 200 was not run. `scripts/36_figures_facf.R:25` sets `lambdavec <- c(25, 50, 100)`, which is why
the histogram figure is a 3×3 grid.

The infeasible benchmark is a single sample: N = 5000, λ = 70, common design, noiseless, on
`seq(0.01, 0.99, len = 99)`, `scripts/11_simulation_generate.R:214-227`. The file
`data-simulation/generated/dt_mc_FAR_conso_N=5000.RDS` carries 5000 curves on 99 grid points, all
`ttag == "tcommon"`.

### 1.6 Failed or missing replications

**No failure ledger exists on disk.** `scripts/22_simulation_blup.R:203-206` catches per-replication
errors so one bad replication does not abort the configuration, and `:215-218` prints a count;
`aggregate_blup_mc` at `:236-239` prints the missing indices. Neither is written to a file, so the
only durable evidence is the file census in §1.5, which is complete.

Downstream, both benchmark tables state **"400 of 400 replications retained in every
configuration"** — `data-simulation/estimates/table/latex_blup_wise_benchmark_conso.tex` and
`..._conso_common.tex`, final row. So no replication produced a non-finite score either.

The only run log on disk is `logs/sync.log`: S3 syncs from 2026-08-27T00:07Z to 2026-08-29T03:12Z.
`/data/predictFTS/data-simulation` is reported "absent" on every sync up to and including
2026-08-27, and present from the 2026-08-28 syncs onward. It records transfers, not computation.

> **Open question.** There is no per-replication failure ledger. The census is complete and the tables
> say 400 of 400, so nothing is missing — but if a replication had failed silently and been retried,
> nothing on disk would say so. Should `22_simulation_blup.R` persist its caught errors to a file
> before the study is re-run?

### 1.7 Weighted ISE, as implemented

`scripts/22_simulation_blup.R:135-153`.

Design weights `rho`, formed on the **target** curve `n0+1` at its own design points `t0`:

```
common design:       rho <- rep(1 / length(t0), length(t0))
independent design:  ghat <- estimate_density(x = t0, h = density_bw,
                                              kernel_name = "epanechnikov", lower = 0, upper = 1)$estimate
                     rho  <- 1 / (length(t0) * pmax(ghat, 1e-6))
                     rho  <- rho / sum(rho)
```

Scores, two variants per replication:

```
ise_blup            <- sum(rho * (Ytrue - prediction)^2)     # against the noisy target
ise_blup_noiseless  <- sum(rho * (Xtrue - prediction)^2)     # against the noiseless target
```

**Against `eq:weighted_ise` at `num_analysis_main.tex:115-117`, three differences.**

**Ruling (2026-08-31): both stand.** `eq:weighted_ise` is the theoretical formula and stays as
written; the renormalisation and the density floor are implementation choices that guard against
numerical degeneration, and they belong in the implementation-details prose, not in the equation.
Neither is an error to fix. What the paper owes the reader is a sentence saying the implementation
normalises the weights exactly and floors the estimated density, and why.

1. **Renormalisation.** The paper writes the independent-design weight as
   `{M_{n0+1} ghat(T_{n0+1,i})}^{-1}`. The implementation divides that by its own sum, so the
   weights sum to 1 exactly. `def_Qn` requires `sum_i rho_{n,i} = 1`, which the unnormalised form
   satisfies only in the limit; normalising enforces at finite `M` what the theory has
   asymptotically. **Documented, not reconciled away.**
2. **Density floor.** `pmax(ghat, 1e-6)` caps the weight a point in a sparse region can receive,
   which is what stops a near-zero density estimate from taking the score to infinity.
   **Documented, not reconciled away.**
3. **Which target.** The paper's `eq:weighted_ise` scores against `Y_{n0+1,i}`, the noisy value.
   The implementation stores both variants, and `scripts/32_figures_blup.R:9-11` states that
   everything downstream except two legacy tables uses the noiseless `Xtrue`. Both are reported in
   `latex_blup_ise_estimates_conso{,_common}.tex`, under the headings "Against `Y_N`" and
   "Against `X_N`", so the paper can carry both columns and needs no choice made for it.

`best_predictor_aout24.tex` defines no ISE, weighted or otherwise; `eq:weighted_ise` is written in
`num_analysis_main.tex` itself. Its own TBC read the normalisation as a disagreement between paper
and implementation; under the ruling above it is not one, and that TBC was to be replaced by the
explanatory sentence rather than by a change to the equation.

**Done, 2026-08-31.** `num_analysis_main.tex:152` now states both guards in prose — the weights are
normalised to sum to one exactly, and the estimated design density is floored at `1e-6` — beside an
unchanged `eq:weighted_ise`. The equation itself was not touched.

The remaining open point in that TBC is untouched by this ruling: the authority defines `rho` and
`D` on the conditioning curve `n0`, whereas the score needs them on the target curve `n0+1`. That,
and nothing else, is now the whole of the TBC at `num_analysis_main.tex:158`.

### 1.8 Comparisons: run and not run

| Competitor | Design | Export | Fitted | Scored | Figure |
|---|---|---|---|---|---|
| `RP20` (Rubin & Panaretos 2020) | independent | **yes** — 4800 files under `data-simulation/estimates/RP20/export/`, 400 per cell, all twelve cells | **no** | no | no |
| `Z26` / `ZH26` (Zhao 2026) | common | no | no | no | no |

`RP20`: the export ran to completion. The comparison did not. `scripts/32_figures_blup.R:526` would
write `log_ise_ratio_blup_over_RP20_prediction`; no such file exists in `figures/blup/`. The
benchmark table `latex_blup_wise_benchmark_conso.tex` carries `\texttt{RP20} --- TBC` in Panel B.

`Z26`: **not implemented at all.** A grep for `zhao|z26|zh26` across `scripts/` returns two hits,
both in `scripts/32_figures_blup.R` — a comment at `:49` ("Neither has been run") and the label
function `competitor_of` at `:50`. There is no estimator, no cross-validation and no export. The
benchmark table carries `\texttt{ZH26} --- TBC`.

Both tables do carry a third panel that **was** run: the estimated mean curve as a naive baseline,
which is the predictor's limit as the Tikhonov parameter grows.

**Ruling (2026-08-31): both comparisons stay TBC in the text.** The author will run `RP20` and
implement `Z26`, and will fill the two result slots then. They are deliberate placeholders, not
oversights, and nothing about them is to be written around, softened or filled from the surrounding
material. The two `--- TBC` panels in the benchmark tables match, and should stay as they are.

**Done, 2026-08-31.** The two comparison subsections were merged into one, and the two result slots
became the single mandated placeholder at `num_analysis_main.tex:240`, standing alone with no result
prose around it. The protocol prose describing both competitors was kept, not dropped.

The one correction the old TBCs still needed was factual, not editorial: they said
`data-simulation/estimates/RP20/` does not exist. It does — the export is complete, 4800 files. It
is the fit and the scoring that are outstanding. **Corrected** at `num_analysis_main.tex:235`, which
now says so and records that `Z26` has no implementation at all.

One inconsistency surfaced in the doing, and is flagged rather than settled: the text writes `Z26`
and `32_figures_blup.R:50` writes `ZH26` into Panel B of the common-design benchmark table, which
the paper now `\input`s. See the TBC at `num_analysis_main.tex:237`.

### 1.9 Tikhonov and density-bandwidth grids

| Parameter | Grid | Line |
|---|---|---|
| Tikhonov, simulation | `exp(seq(-16, 3, length.out = 40))` | `22_simulation_blup.R:70` |
| Design density, simulation | 20 points, log-spaced from `1/lambda` to `1` | `22_simulation_blup.R:61-64` |
| BLUP bandwidth | 20 points; `b0 = 0.5/lambda` (common) or `0.5*(N*lambda)^(-1/1.4)` (random), down to `bK = 0.05` | `22_simulation_blup.R:47-54` |
| (Auto)covariance bandwidth | 25 points; `b0 = (N*lambda)^(-1/1.1)`, `bK = 0.15` | `20_simulation_autocov.R:33-40` |

Selections, from `data-simulation/estimates/table/latex_blup_tikhonov_conso.tex`: median α runs
from 2.7e-4 down to 3.9e-5 under the independent design and from 3.9e-5 down to 1.5e-5 under the
common design, falling with both N and λ. No independent-design configuration reaches the grid
floor. Under the common design the floor is reached in 22.8% of replications at (600, 25), 16.8% at
(300, 25) and 8.5% at (600, 50).

Tikhonov selection is confined to a trailing subset of `max_fit_rows = 1e5`
(`22_simulation_blup.R:31`), which bites only at (600, 200), and only for the selection — the fit
itself always uses every curve before the target.

---

## 2. Covariance and autocovariance

### 2.1 Locations available

Eight `(s, t)` pairs, `scripts/20_simulation_autocov.R:26-27`, confirmed present with 400
replications each in every aggregate:

```
svec <- c(0.2, 0.8, 0.4, 0.7, 0.2, 0.5, 0.1, 0.8)
tvec <- c(0.4, 0.2, 0.7, 0.8, 0.3, 0.2, 0.4, 0.5)
```

They are the union of the pairs used by the two versions of the numerical section; the figure
scripts pick four at a time.

### 2.2 Exponents and selected bandwidths

`H_s` and `H_t` from `H_t = 0.4 + 0.4t`, cross-checked against the `Ht_true` column of
`data-simulation/estimates/locreg/`. Bandwidths are the **median over the 400 replications** of the
selection made in each — the paper's `h*` is one number, the study's is a distribution.

**Lag-1 autocovariance, (N, λ) = (150, 100)**, from
`estimates/autocov/dt_mc_autocov_estimates_conso_lag=1_N=150_lambda=100.RDS`:

| s | t | H_s | H_t | \|H_s − H_t\| | h_1*(s\|t) | h_1*(t\|s) | single bw |
|---|---|---|---|---|---|---|---|
| 0.1 | 0.4 | 0.44 | 0.56 | 0.12 | 0.00109 | 0.00971 | 0.00188 |
| 0.2 | 0.3 | 0.48 | 0.52 | 0.04 | 0.00325 | 0.00738 | 0.00325 |
| 0.2 | 0.4 | 0.48 | 0.56 | 0.08 | 0.00427 | 0.00561 | 0.00325 |
| 0.4 | 0.7 | 0.56 | 0.68 | 0.12 | 0.00427 | 0.00971 | 0.00427 |
| 0.5 | 0.2 | 0.60 | 0.48 | 0.12 | 0.00971 | 0.00427 | 0.00325 |
| 0.7 | 0.8 | 0.68 | 0.72 | 0.04 | 0.00738 | 0.00650 | 0.00561 |
| 0.8 | 0.2 | 0.72 | 0.48 | 0.24 | 0.00971 | 0.00427 | 0.00325 |
| 0.8 | 0.5 | 0.72 | 0.60 | 0.12 | 0.00971 | 0.00738 | 0.00427 |

**Lag-0 covariance, (N, λ) = (150, 100)**, from
`estimates/cov/dt_mc_cov_estimates_conso_lag=0_N=150_lambda=100.RDS`:

| s | t | H_s | H_t | h_0*(s\|t) | h_0*(t\|s) | single bw |
|---|---|---|---|---|---|---|
| 0.1 | 0.4 | 0.44 | 0.56 | 0.00143 | 0.00738 | 0.00188 |
| 0.2 | 0.3 | 0.48 | 0.52 | 0.00427 | 0.00738 | 0.00325 |
| 0.2 | 0.4 | 0.48 | 0.56 | 0.00427 | 0.00738 | 0.00325 |
| 0.4 | 0.7 | 0.56 | 0.68 | 0.00561 | 0.00971 | 0.00427 |
| 0.5 | 0.2 | 0.60 | 0.48 | 0.00971 | 0.00427 | 0.00325 |
| 0.7 | 0.8 | 0.68 | 0.72 | 0.00971 | 0.00971 | 0.00561 |
| 0.8 | 0.2 | 0.72 | 0.48 | 0.00561 | 0.00427 | 0.00325 |
| 0.8 | 0.5 | 0.72 | 0.60 | 0.00738 | 0.00971 | 0.00561 |

**The minimum of the risk surface the supplement actually draws.** A third quantity, and the one the
figure captions promise. The panels of `fig:autocov_risk_2bw` and `fig:cov_risk_2bw` show the *mean*
over the 400 replications of the risk on the `(h_s, h_t)` grid, windowed to `h <= 0.09`
(`30_figures_autocov.R:90, 94-95`; `31_figures_cov.R:23, 27-28`). Its argmin is not the median of the
per-replication selections tabulated above: the first is one optimum of one averaged surface, the
second the middle of a distribution of optima. Read from
`estimates/autocov/dt_mc_autocov_risk_conso_lag=1_N=150_lambda=100.RDS` and
`estimates/cov/dt_mc_cov_risk_conso_lag=0_N=150_lambda=100.RDS` on 2026-08-31.

| s | t | H_s | H_t | \|H_s − H_t\| | lag 1 h_1*(s\|t) | lag 1 h_1*(t\|s) | lag 0 h_0*(s\|t) | lag 0 h_0*(t\|s) |
|---|---|---|---|---|---|---|---|---|
| 0.1 | 0.4 | 0.44 | 0.56 | 0.12 | 0.00143 | 0.01276 | 0.00143 | 0.01276 |
| 0.2 | 0.3 | 0.48 | 0.52 | 0.04 | 0.00427 | 0.00971 | 0.00561 | 0.01276 |
| 0.2 | 0.4 | 0.48 | 0.56 | 0.08 | 0.00427 | 0.00738 | 0.00561 | 0.00738 |
| 0.4 | 0.7 | 0.56 | 0.68 | 0.12 | 0.00561 | 0.00971 | 0.00561 | 0.01276 |
| 0.5 | 0.2 | 0.60 | 0.48 | 0.12 | 0.01276 | 0.00427 | 0.01276 | 0.00427 |
| 0.7 | 0.8 | 0.68 | 0.72 | 0.04 | 0.00971 | 0.00971 | 0.01276 | 0.01276 |
| 0.8 | 0.2 | 0.72 | 0.48 | 0.24 | 0.01276 | 0.00427 | 0.01276 | 0.00427 |
| 0.8 | 0.5 | 0.72 | 0.60 | 0.12 | 0.00971 | 0.00971 | 0.01276 | 0.00971 |

**Ruling (2026-08-31): these are the values that go under the risk panels.** They are the minimum of
the surface the reader is looking at, so the reader can check them against the picture, which is what
the caption claims. The medians of §2.2 above stay the record of what the selection rule does across
replications, and are not quoted under the panels.

Two consequences worth recording. The scripts' own commented tables — `30_figures_autocov.R:182-191`
and `31_figures_cov.R:114-123` — take the argmin of the **median** surface rather than the mean one,
at `30:169` and `31:100`. The two agree at six of the eight locations per lag, and at all four of the
locations the paper reports; they differ at (0.1, 0.4) and (0.2, 0.3) at lag 1, and at (0.1, 0.4) and
(0.5, 0.2) at lag 0. And on this table the separation orders with `|H_s − H_t|` over
(0.7, 0.8), (0.2, 0.4), (0.4, 0.7), (0.8, 0.2): ratios 1.00, 1.31, 2.27, 2.99 at lag 0, strictly
increasing, and 1.00, 1.73, 1.73, 2.99 at lag 1.

**How the supplement's bandwidth-separation claim should read** (`:81` for lag 1, `:112` for
lag 0). **Ruling (2026-08-31): the claim holds**, and the lag-0 prose should say that the difference
is *lower there, and in one case does not exist*, rather than assert the same strength at both lags.

At lag 1 the two selected bandwidths differ at all eight pairs — no pair has `h_s = h_t` — and the
separation is widest at (0.1, 0.4) and (0.8, 0.2).

At lag 0 it is weaker. At (0.7, 0.8), the smallest `|H_s − H_t|` of the eight at 0.04, the two
bandwidths are **equal**: both `0.00971`. That is the claim working, not failing — where the
regularities are close, there is nothing for the two bandwidths to separate over. The lag-0 prose
should say so.

One pair does not follow the monotone reading. (0.8, 0.2) has the largest `|H_s − H_t|` of all,
0.24, and yet its ratio `h_s / h_t` is 1.31, against 5.2 at (0.1, 0.4) where `|H_s − H_t|` is only
0.12. So "the difference grows with `|H_s − H_t|`" is not a statement the lag-0 numbers carry
across all eight pairs, even though the extremes behave as expected. Say the difference is smaller
at lag 0 and vanishes when the regularities are close; do not claim monotonicity there.

Aggregates exist for all twelve `(N, λ)` cells in both families; only (150, 100) is tabulated here
because that is the configuration the supplement's risk figures show.

### 2.3 Risk plots and accuracy boxplots

**Risk surfaces**, PNG, one per location at (N, λ) = (150, 100):

- `figures/autocov/plt_autocov_risk_conso_design_N=150_lambda=100_s=*_t=*.png` — all eight
  locations. `scripts/30_figures_autocov.R:143-145`.
- `figures/cov/plt_cov_risk_conso_design_N=150_lambda=100_s=*_t=*.png` — all eight locations.
  `scripts/31_figures_cov.R:76-78`.
- `figures/cov/plt_diag_cov_risk_conso_N=150_all_lambda_all_4locations.pdf` — the diagonal-line
  risk at s ∈ {0.2, 0.4, 0.5, 0.8} across the four λ at N = 150.
  `scripts/31_figures_cov.R:373-374`.

**Accuracy boxplots**, one per location plus the two four-panel arrangements:

- `figures/autocov/boxplot_conso_autocov_ratio_lag=1_s=*_t=*.pdf` — all eight.
  `scripts/30_figures_autocov.R:249-250`.
- `figures/cov/boxplot_conso_cov_ratio_lag=0_s=*_t=*.pdf` — all eight.
  `scripts/31_figures_cov.R:184-185`.

**The two `_4locations` variants, and which locations each holds.** This is the point that most
easily goes wrong, because the names do not say.

| Variant | Panels, in reading order | Script |
|---|---|---|
| `..._first_4locations` | (0.2, 0.4), (0.4, 0.7), (0.7, 0.8), (0.8, 0.2) | `30_figures_autocov.R:283-291`, `31_figures_cov.R:218-226` |
| `..._second_4locations` | (0.2, 0.3), (0.5, 0.2), (0.1, 0.4), (0.8, 0.5) | same |

The arrangement is `ggarrange(g1, g3, g4, g2)` for the first and `ggarrange(g5, g6, g7, g8)` for the
second, so the *first* panel set is not in the order the `g` objects were created.

**Both now use the first set.** Until 2026-08-31 the supplement's risk figures showed the second set
and neither `_4locations` boxplot was referenced anywhere. Since the rewrite,
`num_analysis_supp.tex:93-103` and `:126-136` show (0.2, 0.4), (0.4, 0.7), (0.7, 0.8), (0.8, 0.2),
and `boxplot_conso_autocov_ratio_lag=1_first_4locations` and
`boxplot_conso_cov_ratio_lag=0_first_4locations` are main-text figures at `num_analysis_main.tex:85`
and `:102` — see [ASSETS.md](ASSETS.md) Table A. The reason for the choice is that the first set is
the only quadruple whose four `|H_s − H_t|` are distinct: 0.04, 0.08, 0.12 and 0.24, against
0.04, 0.12, 0.12, 0.12 for the second, which cannot exhibit a gradient. The second set's four
panels remain on disk, unreferenced on both sides.

**A truncation the boxplots do not declare.** `30_figures_autocov.R:228` and `31_figures_cov.R:162`
filter `ratio < 1.5` before plotting and `ylim(0, 1.5)` clips again, so every unfavourable ratio
above 1.5 is dropped. Over the four reported locations and the twelve setups that is 5456 of the
18668 finite ratios at lag 1 and 6027 of 18729 at lag 0; a further 532 and 471 of the 19200
combinations are non-finite, the estimator returning `NaN` for one or both bandwidth choices. The
text declares both. Replotting `log(ratio)` unclipped would remove the need for the declaration.

Also present, from `scripts/35_figures_locreg.R`: `figures/locreg/boxplot_conso_locreg_Ht_t=*.pdf`
and `..._Lt2_t=*.pdf` at t ∈ {0.2, 0.4, 0.6, 0.8}, plus a `_4locations` arrangement of each.

---

## 3. Real data

### 3.1 French electricity — common design

Source `data-energy/raw/`, read directly. All series normalised by the maximum over the period, so
values land in [0, 1].

| Series | File | Curves | Points per curve | Period | Value range |
|---|---|---|---|---|---|
| Consumption | `dt_conso.RDS` | 122 | 96, all days | 2024-06-01 to 2024-09-30 | 0.546 to 1 |
| Photovoltaic | `dt_solaire.RDS` | 122 | 96 raw, **61 used** | 2024-06-01 to 2024-09-30 | 0 to 1 |
| Hydroelectric | `dt_hydrau.RDS` | 152 | 96, all days | 2023-11-01 to 2024-03-31 | 0.2675 to 1 |
| Wind | `dt_eolien.RDS` | 152 | 96 | 2023-11-01 to 2024-03-31 | — |

Wind is on disk but is **not analysed**: `scripts/40_app_energy.R` runs `conso`, `solaire`,
`hydrau` only.

Photovoltaic output is zero outside daylight, so `scripts/40_app_energy.R:38-46` restricts the solar
curves to 04:00–19:00 and renormalises their observation times over the 61 points retained.

Analysed with 96 quarter-hourly points for consumption and hydroelectricity and 61 for
photovoltaic, which is why `blup_fit()` takes its common-design path
(`scripts/40_app_energy.R:1-6`).

**Target curve.** Each series predicts its own final curve from the one before it:
`scripts/40_app_energy.R:54-56`, "Every series is complete to its last day". There is **no
step-back offset**. The claim at `num_analysis_main.tex:180` that `40_app_energy.R:26-28` steps the
target back by 10 for photovoltaic and 12 for hydroelectric is **stale** — lines 26-28 now hold the
per-series Tikhonov grids.

Per-series Tikhonov grids, `scripts/40_app_energy.R:26-31`:

| Series | Grid | Selected α | Predicted points | Lag-1 FACF |
|---|---|---|---|---|
| conso | `exp(seq(-15, -6, length.out = 40))` | 2.454e-05 | 96 | **0.4328799** |
| solaire | `exp(seq(-13, -6, length.out = 40))` | 5.718e-05 | 61 | **0.4306196** |
| hydrau | `exp(seq(-13, -5, length.out = 40))` | 9.072e-05 | 96 | **0.3649574** |

Read from `data-energy/estimates/dt_blup_{conso,solaire,hydrau}_tikhonov_cv_curve.RDS` and
`dt_blup_{conso,solaire,hydrau}.RDS`, all stamped 2026-08-28 00:55. The lag-1 FACF is constant down
the `lag1_facf` column of each file — one value per series, not a curve — and each file carries one
row per predicted point, so `t` runs `1/96, …, 1` for consumption and hydroelectricity and
`1/61, …, 1` for photovoltaic.

All three sit well above the noise level, so the autoregressive term contributes to the prediction
in every series; hydroelectricity is the weakest of the three. For comparison, the NOTPERP volume
application gives 0.3368356 and the simulation's twelve setups give medians from 0.251 to 0.328
(§1.5, and `estimates/facf/`).

`num_analysis_supp.tex:174` refers to "the lag-1 FACF reference values recorded at
`40_app_energy.R:24`". Line 24 is now a comment about the Tikhonov grids, so the pointer is stale —
but the values themselves are the three above.

**`data-energy/estimates/` mixes two eras.** Seven files are from the current run, 2026-08-28
00:55: the three `dt_blup_*.RDS`, the three `dt_blup_*_tikhonov_cv_curve.RDS`, and one empty
directory entry. Everything else predates it — 2024-11-19, 2024-11-20, 2024-11-27, 2024-12-05,
2025-05-19, 2025-06-04, 2025-07-07, 2025-07-08. That includes every
`dt_*_estimates_blup_rp_*.csv`, every `dt_blup_*_thr0.*.RDS`, every `dt_blup_conf_band_*.RDS` and
every `dt_estimates_*blup_*.RDS`. **No number should be quoted from those** without re-running.

**Prediction accuracy, recomputed 2026-09-01.** No script scores the energy predictions; the three
`dt_blup_*.RDS` store the target curve, the prediction and the mean curve, and nothing else. The
design being common, the weights of `eq:weighted_ise` are $M^{-1}$ and the weighted ISE is the plain
mean over the target's own points. Computed from those three files:

| Series | RMSE, predictor | RMSE, mean curve | Squared-error ratio | Predictor closer at |
|---|---|---|---|---|
| conso | 0.04386 | 0.06408 | 2.134 | 89.6% of 96 points |
| solaire | 0.1212 | 0.1883 | 2.411 | 100% of 61 points |
| hydrau | 0.1394 | 0.1478 | 1.125 | 57.3% of 96 points |

The ordering follows the lag-1 FACF exactly. These are single curves, not distributions, and are
reported as such.

**Done, 2026-09-01.** Consumption is written up in the main text at `num_analysis_main.tex:284-297`;
photovoltaic and hydroelectric are the supplement's deferral at `num_analysis_supp.tex:228-255`. The
stale offset claim is gone from both files. The stale pointer to `40_app_energy.R:24` is gone with
the TBC that carried it.

### 3.2 NOTPERP — independent random design

**The analysed quantity is VOLUME.** Confirmed on disk. **Ruling (2026-08-31): the
paper reports the volume application only. The log-return material is the author's own reference
and is not paper content — it is not to be written up, not cited as a negative result, and not
carried into any figure or table.** It is recorded below and in [ASSETS.md](ASSETS.md) Table B so
that its files are identifiable as reference-only, and for no other purpose.

`scripts/45_notperp_volume_clean.R:6-9` and `scripts/46_app_notperp_volume.R:8-11` both state it:
the response at time `T_{n,i}` on day n is **the log of the USDT notional traded since midnight UTC
up to and including that trade** — cumulative traded notional in logs. The level is the day's
liquidity; the shape is when in the day it arrives.

The application, read from `data-NOTPERP/`:

| Quantity | Value |
|---|---|
| Cleaned file | `raw/dt_NOTPERP_volume_fts_cleaned.csv` |
| Curves, raw | 712 |
| Curves, cleaned | **424** |
| Rows, cleaned | 84 131 |
| M_n, cleaned | min 105, mean **198.42**, median 200, max 200 |
| Lag-1 FACF | **0.3368356** |
| Target curve M_n | 198 |
| Selected α | 0.01599, on a 60-point grid from 2.06e-09 to 54.6 |
| Design-density bandwidth | 0.005 |
| Local regularity H_t | min 0.254, median 0.806, max 1 |
| Local regularity L_t² | min 0.195, max 28.8 |

`scripts/42_app_notperp.R` and `scripts/41_notperp_download_clean.R` are the **reference-only**
log-return pair, and `scripts/44_notperp_quantity_selection.R` is the screen that led to volume.
Their outputs are on disk — `data-NOTPERP/estimates/dt_blup_notperp*.RDS` without the `_volume_`
infix, and `figures/notperp/NOTPERP_*` without it — and none of it is paper content. For
identification only: the log-return cleaned file holds 696 curves and 137 979 rows, its lag-1 FACF
is 0.1130, and its local regularity runs 0.138 to 0.830 with median 0.460.

**Period.** The ByBit archive on disk is `data-NOTPERP/raw/NOTPERP*.csv.gz`, 712 daily files from
**2024-09-18 to 2026-08-30**. One raw curve per archive day.

**How the observation times arise.** Trades arrive at irregular instants that differ from day to
day, and the number per day varies by two orders of magnitude — the raw volume file has M_n from
132 to 49 768, mean 1556. That is what makes the design genuinely random rather than a grid
(`46_app_notperp_volume.R:3-6`). Cleaning applies an iterative functional boxplot over three rounds
to drop outlying curves, keeps one observation per instant, and caps M_n at **200** by a uniform
draw among a curve's instants — which leaves the times an i.i.d. sample from the same design
density, and costs nothing for a cumulative response
(`45_notperp_volume_clean.R:21-22, 36-46`). The cap is what makes the study tractable: the
(auto)covariance estimators sum over within-curve pairs, so cost grows about as M_n^4.

The lag-1 to lag-3 functional autocorrelations quoted in `45_notperp_volume_clean.R:16` are 0.47,
0.38, 0.32 — decaying, which is the property the application rests on. (Those three are the
script's own figures for the cleaned series; the 0.3368356 in the table above is the lag-1 FACF
computed inside the fit, on the curves before the target, and is the value to quote.)

**The stored estimates and the stored sample are from different runs.** Established 2026-09-01 from
file timestamps, and it is the one thing about this application that has to be fixed before
submission.

| Artefact | Written | Which run |
|---|---|---|
| `estimates/dt_notperp_volume_locreg.RDS` | 2026-08-31 00:56 | old |
| `estimates/dt_notperp_volume_mean.RDS` | 2026-08-31 00:58 | old |
| `estimates/dt_notperp_volume_density_cv.RDS` | 2026-08-31 01:02 | old |
| `estimates/dt_blup_notperp_volume{,_tikhonov_cv_curve}.RDS` | 2026-08-31 01:24 | old |
| `figures/notperp/NOTPERP_volume_*.pdf` | 2026-08-31 00:56–01:24 | old |
| `raw/NOTPERP2026-08-30.csv.gz` | 2026-08-31 12:33 | — |
| `raw/dt_NOTPERP_volume_fts.csv` | 2026-08-31 13:43 | new |
| `figures/notperp/Fboxplot_volume_round_{1,2,3}.png` | 2026-08-31 13:43–13:44 | new |
| `raw/dt_NOTPERP_volume_fts_cleaned.csv` | 2026-08-31 13:44 | new |

Two further archive days arrived between the two, and `45_notperp_volume_clean.R:62` re-selects the
stationary window on every run, so the target day moved: the stored prediction carries **198** points
and the current cleaned file's last curve carries **200**. The sizes above — 424 curves, 84 131 rows
— are the *new* file; the α, the FACF, the local regularity and the prediction are the *old* run.
Re-run `45` then `46` and restate both halves from that one run.

**The stationary window, re-derived 2026-09-01** by replaying the rule of `45:152-172` on
`raw/dt_NOTPERP_volume_fts.csv`. The archive is 712 contiguous days carrying 1 107 894 trades.

| Quantity | Value |
|---|---|
| `L_star` | 430 days, starting **2025-06-27** |
| Trend R², window / whole archive | 0.0273 / 0.0929 |
| ADF against a unit root | rejected at 0.01 on every window tried |
| Aggregation grid for the boxplot, `median(M_n)` | 828 points |
| Functional boxplot | round 1 six curves, rounds 2 and 3 none; 430 → **424** |
| Rows sharing a timestamp, inside the window | 14.4% |
| Curves on the cap of 200 | 408 of 424, 96.2% |

The script's own comments at `45:37-38, 56-58` read 2025-06-25 and "some 690 curves". The date is the
same selection two archive days earlier; the 690 is the log-return count carried over and not
updated, as noted below.

**`eq:weighted_ise` degenerates on this application.** Measured 2026-09-01. At the selected
`density_bw = 0.005` the leave-one-out Parzen–Rosenblatt estimate is exactly zero at **21 of the
target's 198 points**, so after the `1e-6` floor and the renormalisation of §1.7 those 21 points
carry **100%** of the weight and the largest single weight is 0.0476 against 0.0051 for a uniform
one. The weighted score is therefore not reported for this application; the text uses the unweighted
mean over the target's own points and says why. This is a property of a sparse random design at a
bandwidth pinned to the grid floor, and it does not arise under the common design, where the weights
are $M^{-1}$ by construction.

**Prediction accuracy, recomputed 2026-09-01** from `estimates/dt_blup_notperp_volume.RDS`:

| Quantity | Value |
|---|---|
| RMSE, predictor / mean curve | 1.564 / 1.721 |
| Squared-error ratio, unweighted | **1.211** |
| Predictor closer at | 98.0% of the 198 points |
| Tikhonov criterion at α\* / at the upper edge | 0.4626 / 0.5616, a reduction of **17.6%** |
| Target curve against the mean function | 1.38 to 2.17 lower in logarithm at every point |
| Prediction against the mean function | departs by at most 0.27 |

The upper edge of the Tikhonov grid is the mean predictor, `exp(4) = 54.6` being large enough for the
correction to vanish; the energy grids stop at `exp(-5)` or below and their upper edges are not.
`46_app_notperp_volume.R:66` states the reduction as 21%; the criterion on disk gives 17.6%.

**Done, 2026-09-01.** The volume application is written up at `num_analysis_main.tex:248-279` and
`num_analysis_supp.tex:174-227`. The four consequences listed when this section was first written are
discharged: the heading names volume, the main-text TBC is replaced by the application itself, the
supplement's descriptive material is recomputed on the volume series, and the observation-time map
carries over unchanged as `eq:notperp:tni`. Two TBCs remain by decision — the run/sample split above,
and the `roahd2019` entry that `references_clean.bib` does not carry.

The discrepancy first recorded here stands: `45_notperp_volume_clean.R:37-38` says the cap "puts the
application at lambdahat near 200 over **some 690 curves**", where the cleaned volume file has **424**.
696 is the log-return cleaned count, so the comment was carried over from
`41_notperp_download_clean.R` and not updated.

---

## 4. Still open

**Four items, and three of them are predictFTS work rather than paper work.** The first is waiting on
the author's own compute and is deliberately left as a placeholder in the text; the second and third
are decisions about what the scripts emit; the fourth is a re-run.

1. **`RP20` and `Z26` results** (§1.8). RP20 is exported — 4800 files — but never fitted or scored;
   Z26 has no implementation at all. The author will run and implement both. Until then
   `num_analysis_main.tex:240` stays TBC by decision, and the two `--- TBC` panels in
   `latex_blup_wise_benchmark_conso{,_common}.tex` stay with it.

2. **Six generated tables cannot be floated** (§1.7, and [ASSETS.md](ASSETS.md)). `Hmisc::latex()`
   wraps each in its own `table` environment carrying no `\caption` and no `\label`, so the float
   cannot be captioned and the table cannot be `\ref`-ed. `get_wise_benchmark_table()` at
   `32_figures_blup.R:187-202` writes a bare `tabular` and is the pattern to follow. Until it is
   followed, the numbers of `latex_blup_ise_estimates_conso{,_common}.tex`,
   `latex_blup_tikhonov_conso.tex` and `latex_blup_bias_sd_conso_common.tex` reach the paper as
   prose. A predictFTS decision, not a paper one.

3. **The `Z26` / `ZH26` tag** (§1.8). The text and the generated table disagree. One has to change.

4. **The NOTPERP volume application must be re-run end to end** (§3.2). The stored estimates are from
   2026-08-31 ~01:00 and the stored cleaned sample from 13:44 the same day, with two further archive
   days in between and a window that re-selects itself on every run. Run `45_notperp_volume_clean.R`
   then `46_app_notperp_volume.R` in sequence, then restate the sample description and every number
   of `num_analysis_main.tex:248-279` and `num_analysis_supp.tex:174-227` from that one run. The TBCs
   in both files name this. Until it is done, no NOTPERP number and its sample size can be quoted in
   the same sentence.

**Settled since this file was first written**, and no longer open:

- *Cross-platform reproduction of the curve column.* §0. Documented in `aws/Dockerfile:19-28`,
  expected, not a defect.
- *What the paper says about reproducibility.* §0. Nothing. It is implicit, and no statement,
  caveat or platform note goes into the text. The study is taken as reproducible pending the
  author's own check.
- *Energy lag-1 FACF values.* §3.1. Read from disk: 0.4328799, 0.4306196, 0.3649574.
- *The NOTPERP quantity.* §3.2. Volume only; the log-return material is reference, not content.
- *Which ISE is authoritative.* §1.7. Both stand — `eq:weighted_ise` is the theory, the
  renormalisation and the density floor are the implementation's guard against degeneration.
- *Whether the weighted ISE can be used on the NOTPERP application.* §3.2. It cannot: 21 of the
  target's 198 points carry the whole of the weight at the selected bandwidth. The text reports the
  unweighted mean and says why. No change to `eq:weighted_ise`, which is a statement about the
  simulation study, where the degeneration does not occur.
- *The lag-0 bandwidth-separation claim.* §2.2. It holds; the lag-0 prose says the difference is
  lower there, and at (0.7, 0.8) does not exist. No monotonicity claim at lag 0.
- *The prediction section's result slots.* §1.7, §1.8. Written up 2026-08-31 from the completed BLUP
  study: the protocol as run, the weighted ISE with both implementation guards stated, the accuracy
  under both designs against the estimated mean curve, and the selected Tikhonov parameters. The two
  competitor slots became the one mandated placeholder.
- *The two real data applications.* §3.1, §3.2. Written up 2026-09-01. NOTPERP volume and
  consumption are the main text, in that order, so that the reader meets the independent and common
  designs in the order the comparison subsection takes them; solar and hydroelectric are the
  supplement's deferral. Facebook is gone from every live file.
- *Whether error summaries not written by any script may be quoted.* §3.1, §3.2. They may, recomputed
  from the stored prediction files and reported as single curves rather than distributions. The six
  ratios are tabulated in those two sections.

The one open question elsewhere in this file, at §1.6, is a repository matter and not a paper one:
`22_simulation_blup.R` catches per-replication failures but does not persist them, so a silent
retry would leave no trace. The census is complete, so nothing is missing today.
