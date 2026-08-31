# ASSETS

Reconciliation between what `num_analysis_main.tex` and `num_analysis_supp.tex` reference and what
exists in `figures/`. Built on 2026-08-31 by reading disk.

`paper\figures` and `D:\Projects\predictFTS\figures` are **byte-identical**: 228 files, md5-compared
tree against tree, no file on either side only. So "which copy" never arises below; a path resolves
in both or in neither.

The tree holds **132 plot assets** — 96 `.pdf` and 36 `.png` — plus 96 `.tex` tikz sidecars, one per
pdf. Of the 132, **16 are referenced** (Table A) and **116 are not** (Table B).

This is a generated map. It goes stale the moment predictFTS is re-run. Where it disagrees with
disk, disk wins.

Companion file: [RUNLOG.md](RUNLOG.md), what was actually run.

---

## The rename that explains most of Table A

`Numerical_analysis-2025-07-v1.tex` — the superseded baseline — names its figures with a
`_new_design_` infix and `_4last_curves`. The current scripts write neither. The correspondence is
established by the producing script in every case below, not by name similarity:

| Superseded name | Current name | Established by |
|---|---|---|
| `real_data_conso_4last_curves` | `real_data_conso_3last_curves` | `10_simulation_design.R:92` |
| `local_exponent_conso` | `local_exponent_simulation_conso` | `10_simulation_design.R:231` |
| `generated_conso_4last_curves` | `generated_conso_3last_curves` | `10_simulation_design.R:507` |
| `variance_FTS_new_design_conso_lag=0` | `variance_FTS_conso_lag=0` | `30_figures_autocov.R:40` |
| `plt_autocov_risk_conso_new_design_*` | `plt_autocov_risk_conso_design_*` | `30_figures_autocov.R:144` |
| `plt_cov_risk_conso_new_design_*` | `plt_cov_risk_conso_design_*` | `31_figures_cov.R:77` |
| `plt_diag_cov_risk_conso_new_design_N=150_all_lambda_all_s` | `plt_diag_cov_risk_conso_N=150_all_lambda_all_4locations` | `31_figures_cov.R:374` |
| `boxplot_conso_autocov_ratio_new_design_lag=1_all_bis` | `boxplot_conso_autocov_ratio_lag=1_second_4locations` | `30_figures_autocov.R:307-308`, where the figure object is `gall_bis` |
| `boxplot_conso_cov_ratio_new_desing_lag=0_all_bis` | `boxplot_conso_cov_ratio_lag=0_second_4locations` | `31_figures_cov.R:241-242`, likewise `gall_bis` |

The `_bis` → `_second_4locations` chain matters: it is the reason the supplement's risk panels and
the unreferenced boxplots show the **same** four locations, and it is not guessable from the names.

---

## Table A — every `\includegraphics` in the two live `.tex` files

All 16 paths resolve. `Exists` is therefore `yes` throughout and the column is dropped; the work is
in the last two columns.

| # | tex | line | referenced path | producing script | what the script actually plots | verdict |
|---|---|---|---|---|---|---|
| 1 | main | 26 | `figures/real_data_conso_3last_curves.pdf` | `10_simulation_design.R:78-95` | The last **three** consumption curves, `id_curve` = N−2..N, normalised to [0,1], arranged one row by `ggarrange` | **RENAMED** from `real_data_conso_4last_curves` |
| 2 | main | 35 | `figures/local_exponent_simulation_conso.pdf` | `10_simulation_design.R:200, 219-231` | `get_local_exponent(t)` — the analytic ramp `hurst_linear(0.4, 0.8)` — as a single line on y ∈ [0,1]. **Not an estimate** | **SUPERSEDED** |
| 3 | main | 35 | `figures/smooth_mean_conso.pdf` | `02_energy_conso.R:136-138` | The Fourier-smoothed mean `get_conso_mean`, which the simulator uses | **MATCH** |
| 4 | main | 36 | `figures/far_kernel_conso.pdf` | `02_energy_conso.R:380-382` | The fitted FAR kernel `get_far_kenel_conso` at `operator_norm = 0.5`, which the simulator uses | **MATCH** |
| 5 | supp | 7 | `figures/generated_conso_3last_curves.pdf` | `10_simulation_design.R:463-507` | Simulated curves 148–150 from `simulate_conso`, seed `12345`, `L2 = 0.01066979`, `intercept_var = 0.05`, `sig_obs = 0.007`, `t_common = seq(0,1,length.out=100)`; solid line the path, red dots the noisy random-design points | **SUPERSEDED** |
| 6 | supp | 14 | `figures/variance_FTS_conso_lag=0.pdf` | `30_figures_autocov.R:14-45` | **One** `geom_line` of `autocovtilde` against t from `dt_true_variance_conso_lag=0.RDS`, y clipped to [0, 0.015) | **SUPERSEDED** |
| 7 | supp | 90 | `figures/autocov/plt_autocov_risk_conso_design_N=150_lambda=100_s=0.2_t=0.3.png` | `30_figures_autocov.R:82-150` | 3-D plotly surface of the mean over 400 replications of the lag-1 risk on the (h_s, h_t) grid, windowed to h ≤ 0.09 | **RENAMED** + **SUPERSEDED** |
| 8 | supp | 90 | `…_s=0.5_t=0.2.png` | same | same, at (0.5, 0.2) | **RENAMED** + **SUPERSEDED** |
| 9 | supp | 96 | `…_s=0.1_t=0.4.png` | same | same, at (0.1, 0.4) | **RENAMED** + **SUPERSEDED** |
| 10 | supp | 97 | `…_s=0.8_t=0.5.png` | same | same, at (0.8, 0.5) | **RENAMED** + **SUPERSEDED** |
| 11 | supp | 119 | `figures/cov/plt_cov_risk_conso_design_N=150_lambda=100_s=0.2_t=0.3.png` | `31_figures_cov.R:18-80` | Same construction at lag 0 | **RENAMED** + **SUPERSEDED** |
| 12 | supp | 119 | `…_s=0.5_t=0.2.png` | same | same, at (0.5, 0.2) | **RENAMED** + **SUPERSEDED** |
| 13 | supp | 125 | `…_s=0.1_t=0.4.png` | same | same, at (0.1, 0.4) | **RENAMED** + **SUPERSEDED** |
| 14 | supp | 126 | `…_s=0.8_t=0.5.png` | same | same, at (0.8, 0.5) | **RENAMED** + **SUPERSEDED** |
| 15 | supp | 136 | `figures/cov/plt_diag_cov_risk_conso_N=150_all_lambda_all_4locations.pdf` | `31_figures_cov.R:335-374` | 2×2 of the diagonal-line risk against h_s at s ∈ {0.2, 0.4, 0.5, 0.8}, four lines each for (N, λ) ∈ {(150,25), (150,50), (150,100), (150,200)} | **RENAMED** + **SUPERSEDED** |
| 16 | supp | 152 | `figures/blup/hist_lag-1_facf_conso.pdf` | `36_figures_facf.R:22-71` | **3×3** grid of lag-1 FACF histograms, N ∈ {150,300,600} × λ ∈ {25,50,100}; one shared set of Sturges breaks from the pooled values | **SUPERSEDED** |

### What each SUPERSEDED figure now shows that it did not before

One line each, so the surrounding prose can be judged.

- **#2 `local_exponent_simulation_conso`** — a straight line from 0.4 to 0.8. It was a fitted,
  non-monotone curve. The column header `$\widehat{H}_t$` at `main:34`, the word "estimated" in the
  caption at `:38`, and the blanket "estimated from consumption curves" are all now wrong for
  panel (a). This is the single change that invalidates the most prose.
- **#5 `generated_conso_3last_curves`** — three curves, not four, drawn under the linear exponent,
  so they roughen to the left and smooth to the right instead of following the old fitted shape.
- **#6 `variance_FTS_conso_lag=0`** — one line, drawn from the simulated series alone. The caption
  at `supp:15` promises "the constructed FTS process … **and the consumption curves**"; nothing of
  the consumption curves is in the figure.
- **#7–#14 risk surfaces** — the bowls sit at different (h_s, h_t) because the exponents changed.
  The four `[TBC: (H_s, H_t)]` and four `[TBC: (h*(s|t), h*(t|s))]` slots around each figure are
  now fillable from [RUNLOG.md](RUNLOG.md) §2.2. The old quoted pairs — H(0.2) = 0.199,
  H(0.3) = 0.844, H(0.5) = 0.784, H(0.8) = 0.445 — are dead.
- **#15 diagonal risk** — same four s values and same four (N, λ) as before, but recomputed under
  the linear exponent. The caption at `supp:137` still describes the figure correctly.
- **#16 FACF histograms** — a 3×3 grid. λ = 200 was never run (`36_figures_facf.R:25`), so the
  twelve setups the surrounding text implies are not shown. The claim at `supp:146` that most
  values fall between 0.2 and 0.4 **does hold** under the new design: 98.4% of the 3600 pooled
  replications, and 95.0% in the worst single cell — see Table C.

**No ORPHAN and no AMBIGUOUS row.** Every referenced path resolves and every producing script was
identified from the script's own `filename =` argument, not from name similarity.

---

## Table B — assets present but referenced by no `.tex` file

116 plot assets. Grouped by producing script. "Candidate section" names the place in the current
`.tex` where a TBC is standing in for exactly this material.

| Group | Assets | Producing script | What it shows | Candidate section |
|---|---|---|---|---|
| Accuracy boxplots, lag-1 | `autocov/boxplot_conso_autocov_ratio_lag=1_s=*_t=*.pdf` (8) + `_first_4locations` + `_second_4locations` | `30_figures_autocov.R:249, 299, 307` | Error ratio of the two-bandwidth estimator against the single-bandwidth one, boxplots by (N, λ) | **`main:72`** — the lag-1 error-ratio TBC. `_second_4locations` matches the four locations the risk figures use |
| Accuracy boxplots, lag-0 | `cov/boxplot_conso_cov_ratio_lag=0_s=*_t=*.pdf` (8) + the two `_4locations` | `31_figures_cov.R:184, 233, 241` | Same at lag 0, against the estimator's own single-bandwidth version | **`main:76`** — the lag-0 error-ratio TBC |
| Risk surfaces, unused four | `autocov/plt_autocov_risk_…_s=∈{(0.2,0.4),(0.4,0.7),(0.7,0.8),(0.8,0.2)}.png` (4), same in `cov/` (4) | `30_figures_autocov.R:143`, `31_figures_cov.R:76` | The other four of the eight locations | none — the supplement shows the second set |
| Local regularity | `locreg/boxplot_conso_locreg_{Ht,Lt2}_t=*.pdf` (8) + two `_4locations` | `35_figures_locreg.R:90, 110` | Estimated H_t and L_t² at t ∈ {0.2, 0.4, 0.6, 0.8} against the design's own values, by (N, λ) | **`supp:75`** — the local-regularity TBC, which says the figures do not exist. They do |
| ISE distributions | `blup/plot_log_ise_blup.pdf`, `plot_log_ise_blup_common.pdf` | `32_figures_blup.R:441` | log ISE boxplots grouped by λ, shaded by N, one per design | **`main:128`** — the weighted-ISE results TBC |
| ISE against the mean baseline | `blup/plot_log_wise_ratio_mean_over_blup.pdf` | `32_figures_blup.R:380` | log ratio of the mean curve's wISE to the BLUP's; below zero favours the BLUP | **`main:128`** |
| Pointwise sd | `blup/plot_sd_blup_and_mean_common.pdf` | `32_figures_blup.R:346` | Standard deviation of predictor and of the mean curve, common design only | **`main:128`** |
| Per-replication curves | `blup/plot_blup_mc_conso_N=*_lambda=*{,_common}.pdf` (24) | `32_figures_blup.R:585` | Four replications per setup, true curve against adaptive prediction, both designs | **`supp:160`** — which says `figures/blup/` is empty. It is not |
| Energy application | `blup/plot_adaptive_blup_{conso,solaire,hydrau,energy}.pdf`, `blup/plot_tikhonov_cv_{…}.pdf` (8) | `40_app_energy.R:186-253` | Prediction of each series' last curve, and the Tikhonov cross-validation criterion | **`main:180`**, **`supp:174`** |
| NOTPERP, volume | `notperp/NOTPERP_volume_*.pdf` (7) + `Fboxplot_volume_round_{1,2,3}.png` | `46_app_notperp_volume.R:383-438`, `45_notperp_volume_clean.R:221-241` | Local regularity, mean function, density CV, Tikhonov CV, adaptive prediction; and the three outlier-removal rounds | **`main:174`**, **`supp:168`** — the current application |
| NOTPERP, log-return | `notperp/NOTPERP_{locreg_Ht,locreg_Lt2,local_regularity,mean_function,density_cv,adaptive_pred,tikhonov_cv}.pdf` (7) + `Fboxplot_round_{1,2,3}.png` | `42_app_notperp.R:370-425`, `41_notperp_download_clean.R:180-210` | The same set for the log-return quantity | **none — reference only.** Decided 2026-08-31: the paper reports volume only. These are the author's reference and are not paper content. Do not propose them for any section |
| NOTPERP, quantity screen | `notperp/NOTPERP_quantity_facf.pdf` | `44_notperp_quantity_selection.R:227` | Functional autocorrelation of each candidate quantity — the evidence for choosing volume | **`supp:168`**, if the screen is reported |
| Consumption data | `real_data_conso.pdf`, `empirical_mean_conso.pdf`, `empirical_cov_conso.png`, `empirical_lag1_autocov_conso.png` | `02_energy_conso.R:27, 120, 189, 240` | Raw curves, empirical mean, empirical covariance and lag-1 autocovariance surfaces | **`main:21`** — the Fourier-estimation paragraph |
| Design diagnostics | `plot_locreg_conso_data.pdf`, `plot_sig_conso_data.pdf`, `plot_Ht_L2t_and_sig_conso_data.pdf` | `10_simulation_design.R:193, 285, 297` | Estimated local regularity, σ, and the three together, on the consumption data | **`main:38`** — the caption TBC asks explicitly whether `plot_locreg_conso_data` belongs there |
| FTS autocovariance | `autocov_FTS_conso_lag=0.png`, `autocov_FTS_conso_lag=1.png` | `30_figures_autocov.R:70` | Surfaces of the true lag-0 and lag-1 autocovariance of the constructed FTS | **`supp:14`** neighbourhood |
| Sparse-regime illustration | `hist_mixture_density_of_T.pdf` | `33_figures_sparse_regime.R:38` | Histogram of the mixture design density | none |
| Calibration | `calibration/*.png` (10) | `15_calibration_grids.R:43-844` | Where each selection falls inside its grid — the evidence the grids are wide enough | none; internal, and `data-simulation/estimates/calibration/` predates the current run |
| **No producing script** | `plot_diagonal_band_subdomains.pdf` | **none found** | Diagram of the diagonal-band subdomains, dated 2025-05-22 | **ORPHAN** — a grep across `scripts/` and `archive/` returns nothing |

### Non-figure assets: eight LaTeX tables, written and unreferenced

Under `D:\Projects\predictFTS\data-simulation\estimates\table\`, all written 2026-08-31 by
`32_figures_blup.R` and `35_figures_locreg.R`. No `\input` in either `.tex` file points at them.

| File | Content | Candidate section |
|---|---|---|
| `latex_blup_ise_estimates_conso.tex` | Mean, Median, Sd of both ISE variants, twelve setups, independent design | **`main:128`** — exactly the five-decimal table the TBC describes |
| `latex_blup_ise_estimates_conso_common.tex` | Same, common design | **`main:128`** |
| `latex_blup_wise_benchmark_conso.tex` | Median wISE, IQR, relative, "BLUP better (%)"; Panel B `RP20 --- TBC` | **`main:143`** |
| `latex_blup_wise_benchmark_conso_common.tex` | Same; Panel B `ZH26 --- TBC` | **`main:162`** |
| `latex_blup_tikhonov_conso.tex` | Median α and floor/top grid percentages, both designs | **`supp:63`** |
| `latex_blup_bias_sd_conso_common.tex` | Pointwise bias and sd at t ∈ {0.2,0.4,0.6,0.8}, common design only | **`main:128`**, whose TBC says a bias/variance split is unavailable — true of the *independent* design only |
| `latex_locreg_Ht_estimates_conso.tex` | True, median, sd, bias, RMSE of H_t, and clamp hits, 48 rows | **`supp:75`** |
| `latex_locreg_Lt2_estimates_conso.tex` | Same for L_t², 48 rows | **`supp:75`** |

### Declared by a script but absent from disk

An absent output is evidence a block did not run. Four:

| Would-be asset | Script | Consequence |
|---|---|---|
| `blup/log_ise_ratio_blup_over_RP20_prediction` | `32_figures_blup.R:525-526` | The `RP20` comparison was never scored, though all 4800 export files exist |
| `cov/estimated_diagonal_line_conso` | `34_figures_corrected_cov.R:82-83` | The corrected-covariance block did not run |
| `local_exponent_simulation_conso_with_title` | `10_simulation_design.R:212-213` | Titled variant not produced; the untitled one is the referenced figure |
| `plot_raw_curves_new_simu` | `10_simulation_design.R:456-457` | Not produced |

Also absent: every figure of `03_energy_solaire.R` and `04_energy_hydrau.R`
(`real_data_solaire.png`, `empirical_*_solaire.png`, `far_kernel_solaire.png`, and the `hydrau`
equivalents). Neither script has been run against the current tree.

---

## Table C — every simulation- or analysis-derived number in the two `.tex` files

"In TBC" marks a value stated inside a magenta `[TBC: …]` note rather than in running prose.

### `num_analysis_main.tex`

| Line | Value | Traceable to | Valid? |
|---|---|---|---|
| 4 | Epanechnikov `K(u) = (3/4)(1−u²)` | `kernel_name = "epanechnikov"` in `20`, `21`, `22_simulation_*.R`, `40`, `46_app_*.R` | **OK** |
| 8 | 122 consumption curves | `data-energy/raw/dt_conso.RDS`, 122 distinct dates | **OK** |
| 8 | 96 points per curve, quarter-hourly | same file, min = max = 96 per date | **OK** |
| 8 | June to September 2024 | same file, 2024-06-01 to 2024-09-30 | **OK** |
| 8, 27 | "last **three**" curves | `10_simulation_design.R:78-95`, three panels | **OK** |
| 43 | `M_n` uniform on `[0.8λ, 1.2λ]` | `11_simulation_generate.R:48-54`, `p = 0.2` | **OK** |
| 43 | `L_t = 0.1` | `11_simulation_generate.R:19`, `L2 = 0.01`, so `L_t = 0.1` | **OK** |
| 43 | "the mean of the estimates of the local Hölder constant" | `10_simulation_design.R:305`, `mean(Lt) = 0.01066979`, i.e. `L_t = 0.1033` | **MISMATCH** — the Monte Carlo uses 0.01, not 0.0107. 0.1 is a rounding of a number the study does not use |
| 43 | "equidistant grid of 99 points between 0.01 and 0.99" | `10_simulation_design.R:99`, `t0 <- seq(0.01, 0.99, len = 99)` | **OK** |
| 43 | `σ(T_{n,i}) = 0.007` | `11_simulation_generate.R:22`, `sig_conso <- 0.007` | **OK** as the value used |
| 43 | "corresponding to the mean standard deviation of the consumption curves" | `10_simulation_design.R:308` comment, `sig = 0.00651048` | **MISMATCH** — the estimated mean is 0.0065; 0.007 is 7.5% above it and is a chosen round number, not that mean |
| 43 | 100 burn-in steps | `11_simulation_generate.R:143`, `n_burnin = 100L` | **OK** |
| 43 | `(N, λ) = (150, 25)` for the illustration | `10_simulation_design.R:464` | **OK** |
| 44 | `L_t = 0.1` consistent with `L2 = 0.01` (in TBC) | as above | **OK** |
| 47 | 400 replications, twelve configurations, seed `N·10⁶ + λ·10³ + id_mc`, 100 burn-in (in TBC) | file census, `11_simulation_generate.R:43-46` | **OK** |
| 53 | `R = 400` | 400 distinct `id_mc` in every cell of `estimates/autocov/` | **OK** |
| 68 | `N' = 5000`, common design, noiseless, `seq(0.01, 0.99, len = 99)` (in TBC) | `11_simulation_generate.R:214-227`; `dt_mc_FAR_conso_N=5000.RDS` holds 5000 curves on 99 points, all `tcommon` | **OK** |
| 72 | `(H_{0.2}, H_{0.3}) = (0.199, 0.844)`, `(H_{0.5}, H_{0.2}) = (0.784, 0.199)`, `(H_{0.1}, H_{0.4}) = (0.529, 0.527)`, `(H_{0.8}, H_{0.5}) = (0.445, 0.784)` (in TBC, quoted as superseded) | superseded estimated exponent | **INVALIDATED by the linear exponent.** Correct values in [RUNLOG.md](RUNLOG.md) §2.2 |
| 72 | "`estimates/` has no `autocov/` directory" (in TBC) | `data-simulation/estimates/autocov/` — 12 cells × 400 | **MISMATCH — stale.** It exists and is complete |
| 76 | "no `cov/` directory" (in TBC) | `estimates/cov/` — 12 cells × 400 | **MISMATCH — stale** |
| 89 | `R = 400`, `(N, λ) ∈ {150,300,600} × {25,50,100,200}` | file census, all twelve cells complete | **OK** |
| 91 | `H_t = 0.4 + 0.4t`, rising 0.4 → 0.8 | `hurst_linear(0.4, 0.8)`; `Ht_true` column of `estimates/locreg/` gives 0.48, 0.56, 0.64, 0.72 at t = 0.2, 0.4, 0.6, 0.8 | **OK** |
| 91 | Hölder constant `0.1` | `L2 = 0.01`; `Lt2_true` column = 0.01 | **OK** |
| 91 | intercept sd `0.0224` | `sqrt(0.01 × 0.05) = 0.0223607` | **OK**, though `11_simulation_generate.R:25` rounds the same number to `0.022` |
| 91 | operator norm `0.5` | `get_far_kenel_conso(operator_norm = 0.5)` default, `11_simulation_generate.R:87` | **OK** |
| 91 | burn-in `100` | `n_burnin = 100L` | **OK** |
| 93 | `0.8λ`–`1.2λ`, shared grid of λ points, noise sd `0.007` | `11_simulation_generate.R:48-54, 136, 22` | **OK** |
| 123 | `R = 400`; "replications on which the score is not finite are dropped, and their number is reported" | `latex_blup_wise_benchmark_conso{,_common}.tex`: "400 of 400 replications retained in every configuration" | **OK** — and the dropped count is zero |
| 124 | `22_simulation_blup.R:143-144` renormalises the weights (in TBC) | `22_simulation_blup.R:144`, `rho <- rho / sum(rho)` | **OK as a fact, wrong as a verdict.** The renormalisation is real, and so is the `1e-6` density floor the TBC omits. But the TBC reads them as "the paper and the implementation disagree", and the ruling of 2026-08-31 is that they do not: `eq:weighted_ise` is the theory, these are the implementation's guard against numerical degeneration. Both stand. See [RUNLOG.md](RUNLOG.md) §1.7 |
| 128 | "`estimates/` has no `blup/` directory and `figures/blup/` is empty" (in TBC) | both populated; 24 `plot_blup_mc_*` files and three tables | **MISMATCH — stale** |
| 141 | `RP20` returns on a regular grid of `241` points, with a B-spline projection (in TBC, flagged unattested) | nothing on disk | **UNVERIFIABLE** — correctly left out of the prose |
| 143 | "`estimates/RP20/` does not exist" (in TBC) | `estimates/RP20/export/` — 12 cells × 400 | **MISMATCH — stale.** The export exists; the *comparison* does not |
| 162 | "No implementation exists" for `Z26` | grep for `zhao|z26|zh26` over `scripts/`: two hits, both labels in `32_figures_blup.R:49-50` | **OK** |
| 171 | section heading "NOTPERP intraday **log-return** curves" | volume is the analysed quantity, `46_app_notperp_volume.R:1`; decided 2026-08-31 that the paper reports volume only | **MISMATCH** — the heading names the wrong quantity |
| 174 | "`data-NOTPERP/estimates/` is empty" and "`42_app_notperp.R` does not run to completion" (in TBC) | ten `.RDS` files, five of them `*_volume_*`; and `42` is the reference-only log-return script, not the one the paper uses | **MISMATCH — doubly stale.** The directory is populated and the named script is no longer the relevant one |
| 180 | 122 consumption at 96 pts, 122 photovoltaic restricted to 61 over 04:00–19:00, 152 hydroelectric at 96 (in TBC) | `data-energy/raw/`; `40_app_energy.R:38-46` | **OK** |
| 180 | target curve stepped back by 10 for photovoltaic, 12 for hydroelectric, per `40_app_energy.R:26-28` (in TBC) | `40_app_energy.R:54-56` predicts each series' own final curve; lines 26-28 are the Tikhonov grids | **MISMATCH — stale.** No offset exists |
| 180 | "`data-energy/estimates/` predates the current script" (in TBC) | 7 files from 2026-08-28 00:55; the rest from 2024–2025 | **PARTLY OK** — the three `dt_blup_*.RDS` and three CV curves are current; everything else is stale |

### `num_analysis_supp.tex`

| Line | Value | Traceable to | Valid? |
|---|---|---|---|
| 8 | curves `148` to `150`, seed `12345`, `intercept_var = 0.05`, `sig_obs = 0.007`, `L2 = 0.0107` (in TBC) | `10_simulation_design.R:463-465, 487, 305` | **OK** — and the `L2` split against the Monte Carlo's `0.01` is real |
| 8 | `(N, λ) = (150, 25)` | `10_simulation_design.R:464` | **OK** |
| 15 | "the figure draws one line, not two" (in TBC) | `30_figures_autocov.R:24-25`, a single `geom_line` | **OK** |
| 50 | `ℓ_max = 3` | not established from any script | **UNVERIFIABLE** — it is a formula constant, not a run parameter |
| 63 | Tikhonov grid `exp(seq(-16, 3, length.out = 40))` (in TBC) | `22_simulation_blup.R:70` | **OK** |
| 69 | design-density grid log-spaced `1/λ` to `1` over 20 points (in TBC) | `22_simulation_blup.R:61-64` | **OK** |
| 75 | "no `locreg/` directory and `figures/locreg/` is empty" (in TBC) | `estimates/locreg/` 12 × 400; `figures/locreg/` 10 pdfs | **MISMATCH — stale** |
| 81, 100, 129 | `R = 400`, `(N, λ) = (150, 100)`, four locations | `estimates/autocov/`, `estimates/cov/` | **OK** |
| 82 | "20 and 21 have not been run" (in TBC) | both families complete | **MISMATCH — stale** |
| 89, 95, 118, 124 | eight `[TBC: (H_s, H_t)]` slots | fillable from [RUNLOG.md](RUNLOG.md) §2.2 | **now available** |
| 91, 98, 120, 127 | eight `[TBC: (h*(s\|t), h*(t\|s))]` slots | fillable from [RUNLOG.md](RUNLOG.md) §2.2 | **now available** |
| 112 | "the optimal pairs are typically such that `h₀*(s\|t) ≠ h₀*(t\|s)` when `H_s ≠ H_t`, the difference more pronounced for large `\|H_s − H_t\|`" | [RUNLOG.md](RUNLOG.md) §2.2, lag-0 table | **NEEDS REWORDING, not correction.** The claim holds; its strength does not carry from lag 1. Say the difference is **lower at lag 0, and at (0.7, 0.8) does not exist** — both bandwidths are 0.00971, which is the claim working, the regularities being closest there. Drop the monotonicity: (0.8, 0.2) has the largest `\|H_s − H_t\|` and a ratio of 1.31, against 5.2 at (0.1, 0.4) |
| 137 | `s ∈ {0.2, 0.4, 0.5, 0.8}`, `(N, λ) ∈ {(150,25),(150,50),(150,100),(150,200)}`, 400 replications | `31_figures_cov.R:363-368` and the `scale_linetype_manual` levels | **OK** |
| 146 | `R = 400`; "most of the lag-1 FACF values are concentrated between 0.2 and 0.4" | `estimates/facf/`, 9 cells × 400. Pooled, **98.4%** fall in [0.2, 0.4]; worst cell (300, 25) 95.0%; medians 0.251 to 0.328 | **OK** — the claim survives the change of design |
| 147 | "`23_simulation_facf.R` has not been run, no `facf/` directory" (in TBC) | `estimates/facf/` — 9 cells × 400 | **MISMATCH — stale** |
| 153 | 3×3 grid, `lambdavec <- c(25, 50, 100)`, λ = 200 absent, pooled Sturges breaks (in TBC) | `36_figures_facf.R:25, 44-46`; only 9 result cells exist | **OK** |
| 160 | "`figures/blup/` is empty" (in TBC) | 24 `plot_blup_mc_*` files present | **MISMATCH — stale** |
| 174 | lag-1 FACF reference values "recorded at `40_app_energy.R:24`" (in TBC) | line 24 is a comment on the Tikhonov grids; the values are the `lag1_facf` column of `data-energy/estimates/dt_blup_{conso,solaire,hydrau}.RDS`, read as **0.4328799**, **0.4306196**, **0.3649574** | **MISMATCH — stale pointer.** The values themselves are now established, [RUNLOG.md](RUNLOG.md) §3.1 |

### Summary

- **11 values are stale P5 verdicts** asserting that results do not exist. They all do. These are
  the most misleading entries in the two files, because they read as settled.
- **4 are genuine MISMATCHes against the run**: the σ provenance (`main:43`), the `L_t` provenance
  (`main:43`), the photovoltaic/hydroelectric offsets (`main:180`), and the log-return section
  heading (`main:171`).
- **1 needs rewording rather than correction**: the lag-0 bandwidth-separation claim (`supp:112`).
  The claim holds; only its strength does not carry over from lag 1.
- **1 set is invalidated by the linear exponent**: the four `(H_s, H_t)` pairs at `main:72`, and
  every bandwidth quoted alongside them.
- **1 verdict is itself wrong**, not the value under it: the TBC at `main:124` frames the weight
  renormalisation as a paper-versus-code disagreement. It is not one — see the ruling in
  [RUNLOG.md](RUNLOG.md) §1.7.
- **2 claims were checked and survive**: the FACF range at `supp:146`, 98.4% pooled; and the three
  energy lag-1 FACF values behind the stale pointer at `supp:174`.
- **2 are UNVERIFIABLE and correctly flagged as such** in the text already: the RP20 241-point grid
  and `ℓ_max = 3`.
- **2 result slots are TBC by decision, not by oversight**: `main:143` (RP20) and `:162` (Z26). The
  author will run and implement both. Do not fill, soften or write around them. The one factual
  correction they still need is that `:143` calls `estimates/RP20/` absent when the export is in
  fact complete — it is the fit and the scoring that are outstanding.

---

## MPV24 → MPV25: occurrences, listed only

Nothing renamed. `MPV24` is a **display label** in `\texttt{}`, not a citation key. The key it
stands for is `Maissoro2025`.

**Five occurrences, all in `num_analysis_main.tex`.**

| File | Line | Occurrence | Kind |
|---|---|---|---|
| `num_analysis_main.tex` | 53 | `against the \texttt{MPV24} estimator, which uses a single bandwidth` | label, running prose |
| `num_analysis_main.tex` | 53 | `\widehat{c}_{\texttt{MPV24}, \ell}^{(i)}` | label, inline maths |
| `num_analysis_main.tex` | 53 | `the corresponding \texttt{MPV24} estimator` | label, running prose |
| `num_analysis_main.tex` | 65 | `\widehat{c}_{\texttt{MPV24}, \ell}^{(i)}` | label, inside `eq:error_ratio_autocov` |
| `num_analysis_main.tex` | 76 | `\texttt{MPV24}` | label, inside a TBC |
| `num_analysis_supp.tex` | — | none | — |
| `best_predictor_aout24.tex` | — | none | — |
| `notations.tex` | — | none | — |
| `references_clean.bib` | — | none | — |

Related, and not the same thing:

| File | Line | Occurrence | Note |
|---|---|---|---|
| `references_clean.bib` | 669–675 | `@article{Maissoro2025, …}` | **Already published**: *Journal of Time Series Analysis*, doi `10.1111/jtsa.70006`, year 2025. The entry needs no change |
| `num_analysis_main.tex` | 17 | `\citet{Maissoro2025}` | correct key |
| `num_analysis_main.tex` | 43 | `\citet{Maissoro2025}` | correct key |
| `num_analysis_supp.tex` | 168 | `\texttt{Maissoro2024}` | **stale key**, named inside a TBC as something to replace when the NOTPERP descriptive material is imported |

Superseded files, for completeness — not to be edited:
`Numerical_analysis-2025-07-v1.tex` has `\texttt{MPV24}` on lines 91, 114, 123, 125, 167, with the
key `Maissoro2025`; `Numerical_analysis-2025-06-v1.tex` has it on lines 77, 99, 108, 110, 162, with
the older key `Maissoro2024` on lines 10, 33, 77.

**So the change is confined to five `\texttt{MPV24}` occurrences in `num_analysis_main.tex` (three
of them on line 53) and one `\texttt{Maissoro2024}` in `num_analysis_supp.tex`.** The `.bib` needs
nothing: `Maissoro2025` already carries the published JTSA reference.
