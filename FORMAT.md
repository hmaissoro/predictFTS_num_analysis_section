# FORMAT.md — the typesetting and notation contract

Attested from `best_predictor_aout24.tex` and the `notations.tex` it inputs at line 83; references are `bp:N`
and `nt:N`. `Numerical_analysis-2025-07-v1.tex`, inputted at bp:498, is **excluded** as the baseline to
dismantle. Anything absent here is **not attested — decide before use**, never imported from elsewhere.

## Custom macros

All in `notations.tex`; the authority declares only `\rouge{…}` = `\textcolor{red}{…}` (bp:69), drafting only.
`\Xtemp`/`\X`/`\hX` (nt:187–221) build superscripted process variants but are **unused in the authority** — do
not adopt without checking what they render.

| Macro | Replaces | At |
|---|---|---|
| `\EE \PP \RR \ZZ \NN \LL \MM \VV \XX \YY` | `\mathbb{E}` `\mathbb{P}` … | nt:6, 21, 23, 36, 18, 17, 19, 27, 29, 33 |
| `\Hh \Cc \Dd \Ll \Oo \Gg \Ss \Rr \Pp \Xx` | `\mathcal{H}` `\mathcal{C}` … | nt:13, 4, 5, 16, 20, 11, 25, 24, 22, 30 |
| `\Xf` `\Mmu` `\Yb` `\Xbf` `\epsb` `\1` | `\mathfrak{X}` (the predictor) `\mathfrak{m}` `\mathbf{Y}` `\boldsymbol{X}` `\boldsymbol{\epsilon}` `\mathbf{1}` | nt:31, 242, 35, 32, 8, 171 |
| `\e` `\Tni` `\Yni` `\eni` `\T` `\HT` `\hHT` | `\varepsilon` `{T_{n,i}}` `{Y_{n,i}}` `{\varepsilon_{n,i}}` `{t_0}` `H_{t_0}` `\widehat{H}_{t_0}` | nt:231, 226, 224, 233, 179, 180, 183 |
| `\EEMT` `\Var` `\Cov` `\argmin` `\argmax` | `\mathbb{E}_{M,T}`; the rest via `\DeclareMathOperator`(`*`) | nt:168–170, 252–253 |
| `\vertiii` `\numberthis` `\lf` `\rf` | triple-bar norm; number an `align*` line; `\left\lfloor` `\right\rfloor` | nt:39, 43, 244–245 |

## Theorem-like environments

`\theoremstyle{plain}` throughout (nt:76). Declared nt:77–88: `theorem`, `corollary`, `proposition`,
`definition`, `lemma`, `example`, `remark`, `conjecture`, plus `lemmaA` ("Lemma A.", nt:82) and `lemmaSM`
("Lemma S.", nt:83). None takes `[section]`, so **every counter runs sequentially through the whole document**,
each kind independently. Used: `theorem` ×3 (bp:798, 816, 835), `proposition` ×3 (bp:309, 378, 1024), `remark`
×2 (bp:345), `lemma` ×1 (bp:687), `proof` ×3 — `\begin{proof}[Proof of Proposition \ref{…}]` is the attested
opening (bp:507); a bare `\medskip` line separates consecutive blocks (bp:307, 343, 814).

**Assumptions are not a theorem environment** — they are an `enumerate` wrapper whose counter persists across
instances, so numbering continues document-wide: `assumptionH` → `(H1)`, `(H2)`, … (nt:122–129), used at
bp:179, 389, 419; `assumptionHt` → `(G1)`, `assumptionE`, `assumptionD` (nt:112, 132, 143) are unused.
Reference only with `\assrefH{label}`, rendering `(H⟨n⟩)` as a hyperlink (nt:154; used bp:346, 534, 799);
likewise `\assrefHt`, `\assrefE`, `\assrefD` (nt:153, 155, 156). `\newtheorem{assump}` with
`\renewcommand\theassump{H\arabic{assump}}` (nt:262–265) and `\newtheorem{step}` (nt:103) are declared but
unused — **do not use them**, they produce a second, conflicting (H⟨n⟩) series.

## Label naming

No single scheme; two are live. Both recorded — pick one before writing.

| Object | Pattern | Two attested examples |
|---|---|---|
| Section | `sec:blup:⟨topic⟩` | `sec:blup:FTS_prediction` (bp:149) — the only one in the file |
| Assumption, grouped | `H:⟨group⟩:⟨name⟩` | `H:model:stationarity` (bp:180), `H:model:Tni` (bp:184) |
| Assumption, ungrouped | `H:⟨name⟩` | `H:C0` (bp:390), `H:source` (bp:394) |
| Proposition | `prop:⟨name⟩` | `prop:tikhonov` (bp:309), `prop:blup` (bp:1024) |
| Theorem | `thm:⟨quantity⟩_unif_cv` | `thm:mean_unif_cv` (bp:798), `thm:cov_unif_cv` (bp:835) |
| Lemma | `lem:⟨name⟩` | `lem:vc_bounded_mult` (bp:687) — the only one in the file |
| Equation, prefixed | `eq:⟨name⟩` | `eq:model` (bp:172), `eq:conf_interval` (bp:456) |
| Equation, bare | descriptive, no prefix | `def_Qn` (bp:203), `our_reg_blup` (bp:323) |
| Equation, in a proof | `eq:proof:⟨prop⟩:⟨name⟩` | `eq:proof:tikhonov:h` (bp:590), `…:alpha0` (bp:578) |
| Figure, table | — | **not attested — decide before use** |

Two defects to repair, not imitate: `H:chap3:gaussian_assumption_FTS` (bp:420) keeps a `chap3:` segment from
the thesis draft; `CP_bound2b` is declared twice (bp:655, and bp:662 in a comment).

## Equation layout

`\mathtoolsset{showonlyrefs}` is on (bp:71): **an equation is numbered if and only if it is `\eqref`-ed**;
never add or drop a number by switching environment. `\numberwithin{equation}{section}` is commented out
(nt:64), so numbers run 1, 2, 3, … across the whole document, appendix included. `\allowdisplaybreaks` (bp:23).
Counts: `equation` 39, `align*` 21, `multline*` 5, `multline` 3, `align` 2, `cases` 3, `array` 8. Use
`equation` for a single display that is or may be referenced (bp:172, 295, 323); `$$ … $$` for one never
referenced (bp:160, 207, 273) — frequent and attested, not `\[ … \]`, which occurs only inside the Lemma proof
(bp:714); `align*` for a derivation (bp:217, 517), `align` only when a line is referenced (bp:535);
`multline`/`multline*` for one long formula that must break (bp:380, 819); `cases` for branches (bp:317);
`array` for block matrices (bp:934, 1012). In `align*` the `&` sits before the relation (`&=`, `&\leq`), one
per line (bp:536–540); paired definitions on one line are joined by `\quad\text{and}\qquad` (bp:220) or
`\qquad\text{where}\quad` (bp:117). **Punctuation.** Displays are punctuated as sentence parts — comma if the
sentence continues (bp:174, 204), period if it ends (bp:162, 166) — with any domain qualifier trailing inside
after `\quad`/`\qquad`: `,\qquad t\in I.` (bp:161), `,\quad \forall s,t\in I.` (bp:165).

## Figure and table markup

**Both not attested — decide before use.** The authority contains no `figure`, no `table`, no `tabular`, no
`\includegraphics` and no `\caption`; `graphicx` (bp:15), `float` (bp:39), `placeins` (bp:40), `pgfplots`
(bp:43) and `tikz` (bp:18) are loaded and never exercised. Undecided: placement specifier, width, graphics
path, caption position and font; and for tables, rules, numeric alignment, decimal count, the layout of
`(N, lambda)` configurations. `booktabs` is not loaded (bp:1–66); `subcaption` is loaded by `notations.tex`
(nt:52) but commented out in the authority preamble (bp:28) — settle that clash before using subfigures.

## Citations and cross-references

`natbib`, `\bibliographystyle{apalike}`, `\bibliography{references_clean}` (bp:36, 876–877). `\citet{key}` puts
the authors in subject position (bp:114, 424); `\citep{key}` is parenthetical (bp:285, 292). The locator goes
in the optional argument, spelled out: `\citet[Section 1.6]{…}` (bp:272), `\citet[Eq. (20) in Lemma 6]{DP2016}`
(bp:622), `\citep[and references therein]{…}` (bp:292). Bare `\cite{key}` occurs 11 times (bp:129, 358, 434,
559, 643, 1026 …); it renders as `\citet` under `apalike` but is **not a third form** — treat each as an error.
Keys are author-plus-year: `Maissoro2025`, `rubin2020sparsely`, `DP2016`. `Maissoro2024` is **not** a key.

`\eqref{…}` for equations, `\ref{…}` for everything else, each preceded by its spelled-out type: *"Proposition
\ref{prop:tikhonov}"* (bp:299), *"Theorems \ref{…}, \ref{…}, and \ref{…}"* (bp:379). **The tie is unsettled —
decide before use.** 12 of 13 `\ref` occurrences use a plain space (bp:299, 358, 379, 507, 749); one uses a
tie, `Lemma~\ref{…}` (bp:649); `\eqref` is 14 plain to 1 tied (bp:288). A break between "Proposition" and its
number is what a tie prevents, so the tie is the better rule — a decision to take, not one readable off file.

## Spelling, numbers, units

**Spelling is unsettled — decide before use.** Mixed on both axes: "centred" (bp:176) against "centered"
(bp:186, 420); "minimize"/"minimized" (bp:281, 804) against "minimise"/"minimised" (bp:822, 842), alongside
"formalise" (bp:177). `babel` is `english`, not `british` (bp:7). British `-ise` matches "centred", "formalise"
and the JTSA lineage. **Numbers:** digits inside maths, always — `$100$`, `$400$`; not attested outside maths
mode. **Percentages and units: not attested — decide before use.** No `\%`, `\SI{}{}`, `\num{}` or `\si{}`
occurs; `siunitx` is loaded twice (bp:13, nt:53) and configured for byte counts (nt:91–92), irrelevant here.

## Appendix — Symbols

| Symbol | Meaning | Defined at |
|---|---|---|
| `\alpha` | **three meanings, all live.** Tikhonov parameter; Hölder exponent of the class; confidence level | bp:295–297 `formula_tik_th`; bp:690; bp:456 (commented) |
| `\beta` | Hölder exponent of the trajectories of $X_n$; also `\boldsymbol{\beta}_{n_0}`, the solution vector | bp:310–312 `Lisch_cst`; bp:572 |
| `\gamma`, $a$ | source-condition exponent in $\psi = F C_0^\gamma$; eigenvalue decay exponent | bp:394 (H7); bp:390–391 (H6) |
| $H_t$, $L_t$ | local Hölder exponent and constant | **neither attested** — $H_t$ used bp:799 but never defined; nearest relative of $L_t$ is $\mathfrak L_n(\beta)$, bp:311–312 |
| `\psi`; $C_0$, $C_1$; $c_0$, $c_1$; $c_1$, $c_2$ | solution of $C_1=\psi C_0$; autocovariance **operators**; autocovariance **functions**; generic decay constants — **the last two collide on $c_1$** | bp:282–285 `psi_bosq`; bp:286; bp:163–166; bp:391 |
| $\Gamma_\ell$, `\widehat\Gamma_{N,\ell}` | matrix-entry autocovariance, superseded block; the estimator is **not attested** — a stray use survives at bp:826 in `thm:autocov_unif_cv`, whose statement otherwise uses $\widehat c_{N,\ell}$ | bp:1019; bp:826 |
| $\mathcal D_{n_0}$, $\varrho_{n,i}$, $Q_n$, $\Pi_{n,0}$, $\Pi_{n,\ell}^\star$ | diagonal weight matrix; design weights, positive and summing to one; weighted empirical measure and its operators | bp:208, 314; bp:203–205 `def_Qn`; bp:214, 218–221 |
| $g$, $\widehat g_{n_0,i}$ | design density, $g_{\min}=\inf_t g(t)>0$; its leave-one-out Parzen–Rosenblatt estimator | bp:184 (H3); bp:302–303 `PR_weights` |
| $N$, $M_n$, $M_{n_0}$ | curves; points on curve $n$; points on the curve used for prediction | bp:174 `eq:model`; bp:263–265 |
| `\lambda` | expectation of $M$ — **but also** `\lambda_k`, eigenvalues of $C_0$, and `\lambda_{\max}` | bp:182 (H2); bp:390; bp:514 |
| `\sigma(t)`, `\sigma^2(t)` | measurement-error scale and variance, Lipschitz | bp:173, 186 |
| $h$; $h_s$, $h_t$; `\Hh_N` | Parzen–Rosenblatt bandwidth; per-argument bandwidths; the grid is **not attested** | bp:305; bp:820; used bp:840 |
| $\mathfrak{X}_{n_0+1}(t;\alpha)$, $\mathfrak{X}^{(M_{n_0})}_{n_0+1}$ | theoretical and sample Tikhonov predictors | bp:295–297; bp:323–328 `our_reg_blup` |
| $\mathfrak{X}_{n_0}(t)$; $B$, $\mathbb{M}_{n_0}$, $\mathbb{V}_{n_0}$, $\Sigma(s,t)$ | BLUP of the **current** curve, and its weight vector, mean, variance, MSE — superseded block | bp:1028–1037 `prop:blup` |

**Weighted ISE. Not attested — decide before use.** No integrated squared error, weighted or otherwise, occurs
in the authority; weight function, domain of integration and normalisation are all undefined, and the plain ISE
ratio in the baseline is not a source.
