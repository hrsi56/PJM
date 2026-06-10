# Accelerated DS Syllabus — v2.3 — for capstone_V5_5.md v5.5 (PJM Western Hub Lean DS Capstone with Cloud-Backed Showcase)

**Header.** Yarden's target is an industry Data Scientist role within ~6.5 months at NIS 35K. His formal math baseline is high-school level — applied stats and signal processing in practice, no university calculus or linear algebra. Month 0 is the binding constraint and is realistically **5–6 weeks at full depth, not 4**; treat 6 weeks as on-plan. Learning Months 0–1 are math-only and front-loaded; Months 2–5 interleave directly with capstone milestones M1–M5 from `capstone_V5_5.md` v5.5. **Pace decision: accept calendar slip from 6 months to ~6.5 months** rather than tighten KEEP-list authoring blocks. (v2.2 adds two short interview-readiness blocks — a raw-SQL authoring block and a standalone DL mini-project — costing ~half a week total, absorbed within existing slack; v2.3 adds the four C4 interview-surface blocks at ~24–28h of parallel filler across Months 1–4; the ~6.5-month envelope still holds.) The added Month 1 Bayesian change-point applied block is a materially stronger interview asset than the time it costs, and the capstone diagnostics (SHAP, permutation, calibration) are where the artifact wins or loses. Cuts come from the KILL and COMPRESS lists, not from KEEP-authoring depth.

Every sub-topic carries a depth label: **[AUTH]** (authoring — go deep), **[REC]** (recognition — compressed coverage), **[APPLIED-AUTH]** (use a library, understand output, recognition-level theory — you defend the result in interview), **[APPLIED-REC]** (use a library or config, recognize the pattern, no theory required — you follow a recipe and debug it). Every sub-topic carries a one-sentence link to a `capstone_V5_5.md` v5.5 deliverable.

**v2 → v2.1 delta.** Three surgical edits + two new rows to align with `capstone-lean.md` v5.4: (1) Month 4 MLflow row upgraded from local file-store to DagsHub-hosted remote tracking + Model Registry; (2) Month 5 showcase-format row collapsed from a three-way choice to marimo-mandated; (3) Month 5 reproducibility row updated from SHA-256 hashing to tagged-commit + committed-snapshot patterns; (4) two new [APPLIED-REC] rows added in Month 5 for Docker multi-stage and HF Spaces deployment mechanics; (5) the "Does NOT Teach" MLOps paragraph split into "now at recognition level" (Docker, DagsHub, HF Spaces) vs. "remains out of scope" (DVC, Evidently, FastAPI, drift monitoring, CI/CD). No changes to Months 0, 1, 2; no changes to the math arc; no changes to the conformal/SHAP/permutation/calibration authoring depth.

**v2.1 → v2.2 delta.** Two additive interview-readiness blocks, no changes to the existing month structure, the math arc, the depth-label vocabulary, or the capstone interleave: (1) a raw-SQL authoring block at [APPLIED-AUTH], parallel filler across Months 2–3, gating the interview funnel rather than a capstone milestone; (2) a standalone deep-learning mini-project at [AUTH] (CNN, outside capstone scope by design), slotted at the Month 1→2 seam. Both are placed to complete before active applications begin (start of Month 5). The blocks live in a dedicated "Supplementary Interview-Readiness Blocks" section after Month 5, with parallel-block pointers added inside Months 1–3 so they surface at the point of use. Pace impact: ~half a week, absorbed within existing slack. The "Does NOT Teach" section gains a note reconciling the standalone CNN with the capstone's no-neural-challenger boundary.

**v2.2 → v2.3 delta.** Alignment release for `capstone_V5_5.md` v5.5 plus five accepted amendments from the 2026-06-10 adjudication (`change-register-decisions.md`); four proposed additions were reviewed and **rejected** and are recorded here so they do not resurface. *Accepted:* (1) **C5-min** — Month 0 resource span corrected to Strang Lectures 1–17 (least squares is OCW Lecture 16, Gram–Schmidt 17; verify against the live OCW index at briefing); (2) **F8** — the Month 4 conformal row now teaches **CQR** explicitly (Romano, Patterson & Candès 2019, §2 minimum) including the **negative-Q subtlety**; (3) **C4** — four interview-surface blocks (classification & metrics, A/B testing, two timed take-homes, mock interviews) added to the Supplementary section with parallel-block pointers at point of use, Months 1–4; (4) **CL5** — CNN mini-project default dataset → **CIFAR-10**; (5) **Rehearse** — Month 5 interview prep adds the "why PJM, in Israel" transfer answer and the "why marimo" one-liner. *Alignment edits:* all operative capstone references → `capstone_V5_5.md` v5.5; the stale `gas_to_load_ratio` mentions (Month 2 interleave, Month 4 SHAP row, Month 5 marimo row) replaced with the v5.5 headline pair (`temperature_deviation_from_normal` primary, `scarcity_stress` secondary) and the three-slider set; `predict_next_week.py` → `predict_next_day.py`; the M2 interleave gains the thin-CI deliverable; the EnbPI rows carry v5.5's single pre-committed reopen condition. *Rejected (do not re-add):* **LO3** (Month-0 SVD/PCA upgrade to [AUTH] + Eckart–Young — re-inflates the binding Month 0 against a junior/mid funnel), **LO2** (standalone causal block — the SEM-mediation background carries the interview answer; Months 3–4 filler slack is spoken for by C4 + SQL), **SC3-learn** (neural-baseline block — falls with capstone SC3), **SC5-learn** (EnbPI block — falls with capstone SC5; revives only under the §13 reopen condition). No changes to the math arc, the depth-label vocabulary, the Month 0–1 structure, or the capstone interleave map. Pace impact: ~24–28h of parallel filler (C4), absorbed; the ~6.5-month envelope holds.

---

## Month 0 — Linear Algebra Foundations + SVD/PCA Recognition Extension

**Time:** 5–6 weeks at 20–22 hrs/week.
**Goal:** Read linear algebra cleanly enough to follow Ridge regression geometry, OLS via normal equations, PCA, and the SHAP "linear in feature space" approximation. Plus a brief SVD/PCA + condition number extension that pays off in interviews.

| Sub-topic | Depth | Resource | Capstone link |
|---|---|---|---|
| Vectors, matrix operations, four fundamental subspaces, OLS via normal equations | [AUTH] | MIT 18.06 (Strang) **Lectures 1–17** (least squares is OCW Lecture 16, Gram–Schmidt 17 — verify against the live OCW index at briefing; Lectures 18–20 are determinants, skim per the [REC] row below); Strang *Introduction to Linear Algebra* Ch. 1–4 | OLS/Ridge baseline (capstone Section 7); feature standardization for the Ridge benchmark |
| Eigenvalues, eigenvectors, diagonalization | [AUTH] | MIT 18.06 Lectures 18–22; Strang Ch. 5–6 | Required prerequisite for the SVD/PCA extension below and for reading correlation structure in the feature catalog |
| SVD as generalization of eigendecomposition; PCA as SVD on centered data matrix; variance-explained interpretation | [REC] | Selected fragments from MIT 18.065 (Strang) Lectures 6–8; 3Blue1Brown SVD video | Used as the diagnostic tool for detecting multicollinearity in the engineered feature catalog (Section 4 of capstone) |
| Condition number of a matrix; what it implies for multicollinearity among features | [REC] | Same as above; one section of Strang *Linear Algebra and Learning from Data* | Answers the standard interview question *"how would you detect collinearity between forecast temperature and forecast load?"* — *"I'd check the condition number of the standardized feature matrix and inspect the singular value spectrum."* |
| Determinants, abstract eigenvalue properties, Jordan forms | [REC] | Skim only — Strang Ch. 5 last sections | None directly; recognition only for completeness |

**Coding deliverables (Yarden ships at the end of the month):**
1. OLS from the normal equation `A^T A x̂ = A^T b` in NumPy on a small synthetic dataset. Plot the projection error vectors.
2. PCA on a 5–10-feature toy correlated dataset (e.g., synthetic load / temperature / gas series with engineered correlations). Report variance-explained per component, the condition number of the standardized feature matrix, and the singular-value spectrum. This is the rehearsal for the same diagnostic on the real capstone feature matrix in Month 2.

**Explicit cuts vs. v1 syllabus:** the Eckart-Young low-rank approximation proof, Frobenius-norm bounds, special-matrix theory (Fourier, circulant, banded), and the bulk of MIT 18.065 are out. The interview-relevant SVD/PCA story is preserved at recognition depth.

---

## Month 1 — Probability, Statistics, and Applied Bayesian Change-Point

**Time:** 4 weeks at 20–22 hrs/week (no slip expected here).
**Goal:** Hypothesis testing fluency for the Diebold-Mariano test, MLE intuition for understanding why quantile loss converges to conditional quantiles, bootstrap for honest confidence intervals on metrics, and an applied change-point block that *validates* the `post_elliott` regime flag rather than assuming it.

| Sub-topic | Depth | Resource | Capstone link |
|---|---|---|---|
| Distributions Yarden will actually meet: Normal, Bernoulli, Poisson, Student-t, Empirical | [AUTH] | Stanford CS109 (Piech) selected lectures on random variables, joint distributions, continuous distributions | Foundation for everything else this month |
| MLE as an optimization principle; likelihood vs. probability; consistency intuition | [AUTH] | CS109 lectures on parameter estimation; one chapter of any standard probability text | Why fitting a quantile regression converges to conditional quantiles; foundation for change-point block |
| Hypothesis testing, p-values, one-sided vs. two-sided, multiple testing awareness | [AUTH] | CS109 inference lectures | Diebold-Mariano test (capstone Section 7.1) — p-value reporting in the final results table |
| Bootstrap for confidence intervals on metric estimates | [AUTH] | One short tutorial (Efron's original is overkill; pick the chapter from *All of Statistics* or a focused blog post) | Bootstrap CIs around MAE / pinball loss / coverage estimates in the final results table |
| Combinatorics, full Bayes derivations, conjugate priors | [REC] | CS109 first lectures only | None — keep at recognition for the change-point block context |
| Bayesian vs. frequentist contrast | [REC] | One short essay (Jaynes or a modern blog post) | Frame for the change-point block |
| **Bayesian change-point detection on the Western Hub price series** | **[APPLIED-AUTH]** | `ruptures` library docs + one short paper on PELT / BinSeg; *Bayesian Online Changepoint Detection* (Adams & MacKay 2007) at skim depth | **Capstone Section 4 (regime indicators) and Section 8.3 (regime-stratified error analysis).** The interview payoff is *"I didn't just hard-code the post-Elliott regime — I detected it empirically and the detector landed within a few days of 2022-12-26. The flag is validated, not assumed."* |

**Parallel blocks this month:**
- **Pandas / Python time-series practical block** [AUTH] (~1 week, ADD-1). Index/timezone handling, resampling, joins on timestamps, vectorized lag/rolling features, parquet I/O. Used immediately in every subsequent capstone block.
- **PJM domain reading** [REC] (~3 days, spread across Months 1 and 2). Olivares 2023 §2 (PJM-COMED setting), Lago 2021 §2–3 (EPF benchmark methodology). Foundation for the capstone narrative.
- **A/B testing / experimentation** [REC] (~4h, v2.3 C4) — rides on this month's hypothesis-testing block while the machinery is fresh: power, sample size, peeking, randomization units. Full detail in the *Supplementary Interview-Readiness Blocks* section.
- **DL mini-project (non-capstone)** [AUTH] — slotted at the **Month 1 → Month 2 seam**, after the math slog and before the capstone M1 build ramps. Standalone CNN (**CIFAR-10 default, v2.3**), explicitly outside capstone scope. Full scope, red line, and routing in the *Supplementary Interview-Readiness Blocks* section below. 16-hour hard cap.

**Coding deliverables:**
1. Hypothesis testing playground notebook: t-tests, chi-square, p-value calculation by hand and by `scipy`.
2. Bootstrap CIs around a synthetic MAE estimate; demonstrate that bootstrap CIs and the textbook normal-approximation CI agree when the residual distribution is well-behaved and diverge when it's heavy-tailed.
3. **Change-point detector on Western Hub DA LMP series.** Run `ruptures` with a sensible cost function and number of breakpoints; report detected breakpoints; verify that one of them lands within a few days of 2022-12-26. Save the output as the artifact that validates the `post_elliott` flag in the capstone feature catalog.

---

## Month 2 — Time-Series, Cross-Validation, and Tree-Based ML — Capstone M1 Interleaves Here

**Time:** ~4 weeks at 20–22 hrs/week.
**Goal:** Walk-forward CV with embargo at full authoring depth, leakage taxonomy that Yarden can recite, gradient-boosting mechanics deep enough to defend LightGBM in an interview, and the "why not classical methods?" answer for ARIMA/SARIMA at the 30-second level.

**Parallel block (non-capstone):** the **SQL raw-query authoring block** begins as light-week filler this month and continues into Month 3 — full detail in the *Supplementary Interview-Readiness Blocks* section. It feeds no capstone milestone; it gates the interview funnel and must be complete before active applications (start of Month 5).

| Sub-topic | Depth | Resource | Capstone link |
|---|---|---|---|
| Walk-forward expanding-window CV; gap/embargo windows; autocorrelation leakage; the leakage taxonomy (target leakage, train-test contamination, look-ahead bias, group leakage) | [AUTH] | Hyndman & Athanasopoulos *Forecasting: Principles & Practice* Ch. 5; Lago 2021 §3 | Capstone Section 5.4 (24h embargo) and Section 5.2 (KFT/LAG leakage audit) |
| Decision tree splitting: entropy, Gini, MSE, **and quantile loss / pinball loss** | [AUTH] | Stanford CS229 (Ng) decision tree and boosting lectures; Hastie, Tibshirani, Friedman *ESL* Ch. 9–10 selected sections | Capstone Section 6.1 (LightGBM with 9 quantile heads) |
| Gradient boosting: how trees are fit to negative gradients; learning rate, max_depth, min_child_samples, L1/L2 leaf penalties; categorical handling; monotone constraints | [AUTH] | CS229 boosting; Ke et al. 2017 (LightGBM paper) end-to-end | Same — capstone Section 6.1 |
| Random forests, bagging, AdaBoost | [REC] | One 5-minute compare to LightGBM | Recognition only — used to answer "why LightGBM over random forest?" |
| ARIMA / SARIMA / ETS classical methods | [REC] | Hyndman Ch. 8–9 at skim depth | **Interview-question framing only:** Yarden must answer *"Why not classical methods here?"* in 30 seconds with the structural reasons (heavy non-linearity in regime-stress days, large feature spaces from weather + load + gas + lags, mature evidence in EPF literature that GBMs match or beat classical on this data). |
| FFT / spectrograms | [REC] | None new — leverage Yarden's existing signal-processing background | Recognition only; not used in the lean capstone |

**Capstone interleave (this is when M1 lands):**
- PJM Tools account + Data Miner 2 subscription key (B-Manual block — executed in Month 0, ahead of the data-layer de-risking spike).
- DagsHub account creation (B-Manual block, must precede M2 logging — schedule clerically in Months 0–2).
- Historical bulk pull from Data Miner 2 + EIA + NOAA via `gridstatus` — feed statuses pre-confirmed by the Month-0 de-risking spike; window 2019-01-01 → the pull date, Fold 5 dates pinned at the pull (capstone §3, §5.1).
- Feature catalog v1 implementation, including **`temperature_deviation_from_normal`** (train-only Fourier climatology) as the primary domain-insight candidate, **`scarcity_stress`** as the secondary, the **train-only temperature-aggregate load weights**, and the regime indicator features (with the **R-4** alert-sourcing outcome — full records or the pre-authorized day-after fallback — applied; capstone §4, §11).
- KFT/LAG leakage audit drafted (uses the leakage taxonomy from this month's reading).
- CP-1 closes M1.

**Coding deliverable (learning-side, not capstone):**
- LightGBM on a single feature set (need not be the full capstone catalog) with walk-forward CV and 24h embargo, reporting MAE and pinball loss on at least two folds. This is the rehearsal before capstone M2.

---

## Month 3 — Quantile Regression, Isotonic, Diebold-Mariano — Capstone M2 Lands

**Time:** ~4 weeks at 20–22 hrs/week.
**Goal:** The mathematics behind capstone Section 6 and Section 7 at full authoring depth, plus the Diebold-Mariano test wired into the results.

**Parallel blocks (non-capstone):** the **SQL raw-query authoring block** (begun in Month 2) completes this month — see *Supplementary Interview-Readiness Blocks*. 20-hour hard cap across the two months; done when the LAG/LEAD window-function queries can be written cold. **Also this month (v2.3 C4):** the **Classification & metrics block** [AUTH-light, **10–12h hard cap**] lands early in the month — the program's biggest interview-funnel gap — and **timed take-home rehearsal #1** (~4h box) lands late in the month, consuming the classification block's output. Full detail in the Supplementary section.

| Sub-topic | Depth | Resource | Capstone link |
|---|---|---|---|
| Quantile loss / pinball loss derivation; asymmetric penalty intuition; conditional quantile interpretation; why the median minimizes MAE while quantile loss generalizes this | [AUTH] | Koenker *Quantile Regression* Ch. 1 (selected sections); LightGBM docs on `objective='quantile'` | Capstone Section 6.1 (9 quantile heads), Section 6.2 (motivates split conformal) |
| Uncalibrated quantile estimates vs. coverage guarantees — what each means and why they differ | [AUTH] | Angelopoulos & Bates *Gentle Introduction to Conformal Prediction* §1–2 (preview only this month; full read in Month 4) | Capstone Section 6.2 (split conformal motivation) |
| Isotonic regression for quantile crossing; `sklearn.isotonic.IsotonicRegression` | [AUTH] | sklearn docs; one short tutorial | Capstone Section 6.1 (isotonic post-processing) |
| LightGBM mechanics deepening: tree growing strategy (leaf-wise vs. depth-wise), histogram-based splits, GOSS, EFB | [AUTH] | Ke et al. 2017 second pass; LightGBM source code skim | Defends LightGBM choice in interview |
| Diebold-Mariano test: loss differential series, autocorrelation correction (HAC variance), test statistic derivation, one-sided vs. two-sided framing; mention Giacomini-White as the conditional alternative | [AUTH] | Diebold & Mariano 1995 original paper; `scipy.stats` implementation walkthrough | Capstone Section 7.1 (DM test reported in results) |
| Reading scientific papers efficiently (LightGBM, conformal, SHAP — what Yarden needs vs. exhaustively) | [REC] | Half-day meta-skill block at the start of the month | Pays for itself across the remaining papers this and next month |

**Capstone interleave (this is when M2 lands):**
- Seasonal-naïve 24h + 168h baselines.
- Ridge baseline with the engineered feature catalog (uses the OLS / normal-equation work from Month 0).
- LightGBM quantile ensemble end-to-end with walk-forward CV (24h embargo, **v5.5 fold scheme: winter Fold 4 Jan–Mar 2025, Fold 5 ending at the pull date**) and **MLflow logging to the DagsHub-hosted tracking URI**.
- Isotonic post-processing wired in.
- Diebold-Mariano test against seasonal-naïve 24h, with statistical significance reported. (CP-2 carries the **pre-committed secondary gate** — Fold-5 p ∈ [0.05, 0.15] + pooled-across-folds DM significant → pass, both numbers reported; capstone §7.1.)
- **Thin CI wired (v5.5):** GitHub Actions runs the capstone §9.4 invariant tests on every push — no regression matrix.
- CP-2 closes M2 (≥ 5 MLflow runs publicly visible on DagsHub UI; CI green on `main`).

---

## Month 4 — Calibration, Conformal, and Explainability — Capstone M3 Lands

**Time:** ~4 weeks at 20–22 hrs/week.
**Goal:** The diagnostic stack that distinguishes the artifact from a generic LightGBM tutorial. CQR conformal at full authoring depth (two papers, focused), SHAP at full authoring depth, permutation importance as a complement, reliability diagrams that visibly compare raw and conformal-adjusted intervals.

**Parallel blocks (non-capstone, v2.3 C4):** **timed take-home rehearsal #2** (~4h box) and the **first mock interview** land this month — see *Supplementary Interview-Readiness Blocks*.

| Sub-topic | Depth | Resource | Capstone link |
|---|---|---|---|
| **Conformalized Quantile Regression (CQR) — the construction the capstone ships:** exchangeability assumption; held-out calibration set; per-pair conformity scores `E_i = max{q̂_lo(x_i)−y_i, y_i−q̂_hi(x_i)}`; symmetric threshold adjustment per interval; finite-sample **per-interval marginal** coverage guarantee; **the negative-Q subtlety** — when the raw heads over-cover, `Q` (the empirical (1−α) quantile of conformity scores) can be negative and *narrow* the interval, which is why isotonic stays last; **why time-series mildly violates exchangeability and what that means for empirical coverage on regime-shift folds** | [AUTH] | Angelopoulos & Bates *A Gentle Introduction to Conformal Prediction and Distribution-Free Uncertainty Quantification* end-to-end; **Romano, Patterson & Candès 2019 (CQR), §2 minimum** | Capstone Section 6.2 — the exchangeability paragraph the artifact quotes verbatim is the interview centerpiece; the negative-Q point defends the CQR-then-isotonic post-processing order |
| EnbPI, SPCI, weighted conformal, online conformal | [REC — skip past] | One paragraph awareness, nothing more | Capstone explicitly does **not** include these; interview-quotable reason in Section 13 of capstone — which now carries the **single pre-committed reopen condition** (post-Elliott coverage divergence > 10 pp at M3 → EnbPI returns as remediation, comparison only) |
| SHAP for tree ensembles: Shapley values from cooperative game theory, the TreeExplainer algorithm, global vs. local explanations, summary plot, dependence plot, interaction interpretation, **common pitfalls (correlated features producing misleading attributions; SHAP is not a causal explanation)** | [AUTH] | Lundberg & Lee 2017 (KernelSHAP); Lundberg 2020 (TreeSHAP) | Capstone Section 8.1 (SHAP summary + top-10 + two dependence plots) |
| Permutation importance: `sklearn.inspection.permutation_importance` mechanics; why it answers a different question from SHAP (degradation under shuffling vs. attribution in expectation); when each is misleading | [AUTH] | sklearn docs + one focused tutorial | Capstone Section 8.2 (permutation importance alongside SHAP) |
| Calibration and reliability diagrams: conditional vs. marginal coverage, reliability plots, what coverage divergence indicates, how to read one honestly | [AUTH] | Conformal tutorial above + one short blog post on reliability diagrams | Capstone Section 8.4 (reliability diagram for raw and conformal-adjusted intervals) |
| Fan-chart and reliability-diagram plotting in matplotlib | [AUTH] | matplotlib docs; one short tutorial (ADD-2) | Capstone Section 8.4 and the 7-day quantile fan chart in the final artifact |
| **MLflow with cloud-backed tracking and a Model Registry** — remote tracking URI configuration against a DagsHub-hosted server; logging hyperparameters, per-fold metrics, and artifacts (SHAP plots, reliability diagrams, regime-stratified tables); Model Registry concepts (Staging / Production lifecycle tags); how the public DagsHub UI exposes experiment history to hiring managers via permalink | [REC] | MLflow docs on tracking server + Model Registry; DagsHub MLflow integration docs; one short walkthrough video | Capstone Section 9.1 (DagsHub-hosted MLflow tracking + Model Registry; public read-only UI linked from CV and LinkedIn) |

**Capstone interleave (this is when M3 lands):**
- Split conformal calibration layer on top of LightGBM heads, fit on the held-out calibration set.
- SHAP global summary + top-10 + two dependence plots (`cold_weather_alert` × hour-of-day; **headline domain feature × `cold_weather_alert`** — `temperature_deviation_from_normal` by default, `scarcity_stress` if SHAP selects it at M3; capstone §8.1).
- Permutation importance on validation tail.
- Regime-stratified error table.
- Reliability diagram for raw and conformal-adjusted intervals at the 50% / 80% / 95% levels.
- CP-3 closes M3.

---

## Month 5 — Artifact, Polish, Containerization, Deployment, Interview Prep — Capstone M4 and M5 Land; Track C Activates

**Time:** ~4 weeks at 20–22 hrs/week, with the final 1–2 weeks shifting partly to Track C (CV iteration, LinkedIn signal, interview prep, applications).
**Goal:** Ship the deployed marimo showcase on Hugging Face Spaces and begin applying. Two new [APPLIED-REC] rows this month cover the containerization and deployment patterns Yarden's CV will quote.

| Sub-topic | Depth | Resource | Capstone link |
|---|---|---|---|
| **Marimo as the mandated showcase format** — reactive notebook patterns; how reactive sliders update plots, SHAP attributions, and forecast curves live; UI element conventions (`mo.ui.switch`, `mo.ui.slider`, `mo.ui.dropdown`); state management vs. classical Jupyter | [REC] | marimo docs + one short comparison piece (marimo vs. Jupyter for reactive notebooks) | Capstone Section 10 — marimo is mandated, not chosen; the **three interactive sliders** (`cold_weather_alert` toggle, **headline-feature perturbation** — `temperature_deviation_from_normal` or `scarcity_stress` per the M3 SHAP selection, quantile-level selector), served from the precomputed lookup with the per-cell OOD flag, are the centerpiece of the deployed artifact |
| Reproducibility patterns: `pyproject.toml` version pinning via `uv` (or Poetry); seed-setting across NumPy / sklearn / LightGBM; tagged-release-commit convention; committed Parquet snapshot at the tagged release for reviewer one-step reproducibility (no SHA-256 manual hashing — git's content-addressing handles this) | [AUTH] | One short tutorial on `uv` + git tag workflows; pyproject.toml docs | Capstone Section 9.3 — reproducibility statement: pinned deps, tagged commit, committed snapshot, public DagsHub MLflow permalinks |
| Inference-script patterns: idempotent data pulls, defensive smoke tests, clean Parquet output schemas. `predict_next_day.py` (renamed from `predict_next_week.py` — the product is a D+1 forecast) ships inside the container as a fallback CLI for cold-start regeneration and local development — not the user-facing artifact | [AUTH] | Pattern reading from real OSS forecasting projects | Capstone Section 9.2 (CLI fallback inside the container) |
| **Docker multi-stage build patterns** — base layer with locked dependencies; application layer with notebook code + data snapshot; why multi-stage matters for HF Spaces image-size limits; layer caching for fast rebuilds; `.dockerignore` discipline; common gotchas (large layer leftovers, secrets in build context) | [APPLIED-REC] | Docker official multi-stage docs; one short HF Spaces Docker tutorial | Capstone Section 9.2 — containerized marimo showcase; the Dockerfile is the M5 deployable artifact |
| **Hugging Face Spaces deployment mechanics** — Spaces SDK selection (Docker mode vs. Gradio/Streamlit mode); `app.py` entry-point conventions for Docker Spaces; secrets management via Spaces Settings (DagsHub token for Model Registry access); free-tier hardware reality (~2 vCPU, 16 GB RAM, 48h sleep timeout); GitHub Actions keep-alive cron pattern (12h cadence ping); cold-start latency expectations and the README "~30s first-load on cold start" disclaimer; CPU Upgrade contingency ($0.03/hr ≈ $22/month) if cold start empirically fails CP-5 | [APPLIED-REC] | HF Spaces docs on Docker SDK + secrets + hardware tiers; one short reference deployment from another portfolio project | Capstone Section 9.2 (deployment) + CP-5 (deployed-URL stranger-test) |
| Interview prep: rehearse the leakage audit answer, the exchangeability paragraph, the regime-stratified error story, "why not classical methods?", "why not a neural challenger?", "why not DVC?", "why no live API pulls?", "how did you detect collinearity?", "what does the DagsHub MLflow UI show?", **"why PJM, in Israel?"** (the 30-second transfer answer: *consequential, noisy, regime-shifting time series with a published benchmark literature — same problem shape as demand forecasting, fintech risk, ad-spend pacing, anomaly detection on telemetry*), **"why marimo, not Streamlit/Gradio?"** (*reactive dataflow makes the slider-lookup pattern natural; HF Spaces Docker mode hosts it identically*), and **"why a thin CI but no regression matrix?"** (capstone §13 testing entry). **Mock interviews ×2–3 (~1h each, v2.3 C4) run here and in late Month 4**, before applications mature. | [AUTH] | Self-rehearsal against the "What this project is NOT" section of `capstone_V5_5.md` v5.5 | The capstone interview script, prepared explicitly |

**Capstone interleave (this is when M4 and M5 land):**
- HF account + initial Spaces setup (B-Manual block, scheduled at the start of Month 5).
- **M4 lands first — local end-to-end.** Marimo showcase runs locally pulling the registered Production model from DagsHub MLflow and applying conformal thresholds on the snapshot data; `predict_next_day.py` smoke-test passes inside the container. CP-4 closes.
- **M5 lands second — deployed.** Marimo showcase containerized via multi-stage Dockerfile and deployed to HF Spaces free tier; GitHub Actions keep-alive cron configured; CV and LinkedIn updated with the live Spaces URL and the public MLflow URL. CP-5 closes.
- **Stranger-test gate happens on the deployed HF Spaces URL, not locally.** Reviewer recruitment is a Track C-Manual block scheduled earlier in Month 4 / early Month 5.

**Track C interleave (begins this month):**
- Per `orchestrator-role.md` Phase-trigger rule, Track C activates continuously after M2 in the v5.5 timeline — but the application *pace* picks up in Month 5 as the diagnostic stack and the final artifact become showable.
- CV iteration windows (the three permitted across the year): after M2 (baselines + LightGBM + DM test + public MLflow UI link), after M3 (conformal + SHAP + regime errors), after M5 (deployed marimo showcase + Spaces URL). Each is a C-Claude block.
- LinkedIn signal: optional at M2 landing; surface to Yarden, his call.
- Active applications begin no later than the start of Month 5.

---

## Supplementary Interview-Readiness Blocks (v2.2 + v2.3 additions)

These blocks sit outside the math arc and outside the capstone interleave. None feeds a capstone milestone; all close gaps that surface specifically in the DS interview funnel. They are placed to land before or alongside active applications (start of Month 5) and to double as concrete artifacts during the math-heavy stretch.

### SQL — Raw-Query Authoring Against a Real DB

**Goal:** Author raw SQL cold (no ORM) at the level a DS SQL screen probes. The gap is *authoring fluency*, not relational fundamentals — those are already solid (PK/FK modeling, one-to-many with cascade, transactions, aggregations, conflict-check-before-insert with bulk update), but written through SQLAlchemy, so the raw-query muscle was never exercised. A SQL screen is cold authoring under pressure, which is why this is [APPLIED-AUTH] and not [REC]: you cannot pass "write me a query that does X" by recognizing patterns.

| Sub-topic | Depth | Resource | Link |
|---|---|---|---|
| Manual JOINs (inner/left/right/full), multi-table joins | [APPLIED-AUTH] | Mode Analytics SQL Tutorial (Intermediate); PostgreSQL docs (joins) | Mirrors the relational joins the ORM wrote implicitly in the RSVP app — now authored by hand |
| GROUP BY + HAVING, aggregations, DISTINCT, NULL filtering | [APPLIED-AUTH] | Mode Analytics; pgexercises.com (Aggregation set) | The same `func.max`/DISTINCT/NULL logic used via ORM, in raw form |
| Subqueries and CTEs (`WITH`), including chained CTEs | [APPLIED-AUTH] | Mode Analytics (Advanced); PostgreSQL docs (`WITH` queries) | Readable multi-step transforms — the SQL analog of a pandas method chain |
| Window functions: ROW_NUMBER, RANK, DENSE_RANK; **LAG / LEAD**; PARTITION BY + ORDER BY frames | [APPLIED-AUTH] | PostgreSQL docs (Window Functions); pgexercises.com (Window set) | **`LAG(price, 24) OVER (ORDER BY ts)` *is* the capstone lag-24h feature — same idea, SQL surface. Reinforces feature-engineering intuition rather than being disconnected drill.** Most common topic in DS SQL screens. |
| Query-planner internals, EXPLAIN/ANALYZE, indexing strategy | [REC] | Skim only — one short overview | Recognition only — not probed in a DS SQL screen. Do not author. |

**Placement & cap:** Parallel filler across Months 2–3, on the light weeks (B-Manual account-creation days, domain-reading slack). **20-hour hard cap.** Complete before active applications (start of Month 5) — fresh enough to retain, early enough to dodge the Month 5 deploy+interview crunch. Modular, 1-hour-chunkable; must not displace math or capstone-authoring depth.

**Setup note:** NotebookLM can't run SQL, so author against a real DB — reuse the existing Supabase instance or spin up local Postgres with a toy capstone-shaped table (`ts, price, load, gas`). Trivial; folds into the block, not its own action.

**Coding deliverable:** ~15–20 hand-authored queries of increasing complexity, culminating in 3–4 window-function queries that reproduce the capstone's lag features (lag-24h, lag-168h, rolling stats) in raw SQL. When you can write the LAG/LEAD queries cold, the block is done — do not keep adding query types.

### DL — Standalone CNN Mini-Project (Outside Capstone Scope)

**Goal:** Ship one artifact that proves end-to-end network build-and-train capability. The gap is a *missing artifact*, not missing knowledge — CNNs and backprop were taught (1-on-1, plus course grade 94), but there is no DL project, no trained model, no repo. Taught + graded = explained knowledge; recruiters buy *proven* doing. The value here is the shipped repo, not theory depth or accuracy.

| Sub-topic | Depth | Resource | Link |
|---|---|---|---|
| CNN architecture in PyTorch (2–3 conv blocks + pooling + 1–2 FC); forward pass, loss, optimizer, training loop | [AUTH] | PyTorch official "Training a Classifier" (CIFAR-10) tutorial; torchvision datasets docs | None by design — standalone. Theory stays at [REC] (already taught), not re-derived. |
| Train/val/test split, curve logging (loss + accuracy), confusion matrix, example predictions | [AUTH] | Same; matplotlib | None — the artifact's evidence layer |
| One regularization round — dropout **or** basic augmentation (not both, not a sweep) | [REC] | torchvision.transforms docs | None — demonstrates awareness, capped at one |
| HP search, transfer learning, deeper nets, SOTA tuning | OUT | — | Red line — explicitly excluded |

**Task:** CNN on **CIFAR-10** (**default, v2.3 CL5** — less tutorial-flavored, shows 3-channel image handling; ship-at-threshold ~70%+), with **Fashion-MNIST** as the de-scope fallback only if the 16-hour cap threatens. NOT time-series (loaders/windowing/normalization break the "mini"). NOT a tabular MLP (doesn't show DL-specific machinery, and invites the "why not LightGBM?" question the capstone already answers). PyTorch, CPU/MPS on the M3 — both datasets train fine without a GPU.

**Scope red line (pre-committed against completionism):**
- Ship at a sane threshold (~90%+ Fashion-MNIST, ~70%+ CIFAR-10 with a basic CNN) → stop.
- No HP sweep / grid / Optuna. No transfer learning / ResNet. No SOTA chase. No deployment, no app.
- **Hard stop:** first clean run clearing the threshold, plus curves + confusion matrix + clean README (seed set, deps pinned) + a "what I'd do with more time" paragraph (where stretch goals get *named*, not built) = DONE. Resist improving it.

**Placement & cap:** Month 1 → Month 2 seam, before the capstone M1 build ramps. **16-hour hard cap, 2–3 focused days.** Lands the artifact ~Month 2 for the first CV pass, and does double-duty as the palate-cleanser between the Months 0–1 math slog and the capstone grind.

**Routing note:** This is a **B-Claude build** (engineer → Claude Code → debug loop), not an L block — flagged standalone, *do not wire into the PJM capstone* (no-NN by design; inflating it would break the defended-boundary narrative). It appears in the syllabus only so the learning arc is complete on paper.

### Interview-Surface Blocks (v2.3, C4)

Four blocks closing funnel-specific gaps the program otherwise leaves open. They gate the interview funnel, not a capstone milestone, and all caps are hard — these blocks must never displace math or capstone-authoring depth.

**C4-1 — Classification & metrics.** The biggest gap in the program: everything taught is regression/forecasting, while most Israeli DS screens probe classification. Partly refresh (Technion ML 89), hence [AUTH-light].

| Sub-topic | Depth | Resource | Link |
|---|---|---|---|
| Logistic regression (decision boundary, regularization), ROC/PR curves and when each is the right plot, confusion-matrix trade-offs and threshold selection, class imbalance (resampling vs. class weights vs. threshold moving), bias-variance framing | [AUTH-light] | Stanford CS229 logistic-regression lecture; sklearn docs (`LogisticRegression`, `roc_curve`, `precision_recall_curve`, `class_weight`); one focused imbalanced-learning tutorial | Gates the interview funnel; rides on the tree/boosting intuition from Month 2 |

**Placement & cap:** early Month 3, parallel filler. **10–12h hard cap.** **Deliverable:** one notebook on a public imbalanced tabular dataset — logistic + LightGBM classifier, ROC/PR comparison, threshold tuning against a stated cost trade-off, calibration curve. Feeds take-home rehearsal #1 directly.

**C4-2 — A/B testing / experimentation.** [REC], **~4h**, rides the Month 1 hypothesis-testing block while power/sample-size machinery is fresh: power, sample size, peeking and sequential-testing pitfalls, randomization units. **Deliverable:** a one-page cheat sheet + three worked sample-size calculations.

**C4-3 — Timed take-home rehearsals ×2.** Generic tabular classification CSV, **4-hour box**, ship a notebook — the actual funnel format, rehearsed under the actual constraint. One in late Month 3 (consumes C4-1's output), one in Month 4. ~4h each + a 30-minute debrief naming what was cut under time pressure (the debrief is the learning artifact). Routed as B-Claude-free solo work — no AI assistance inside the box, by design.

**C4-4 — Mock interviews ×2–3.** ~1h each, late Month 4 → Month 5, before applications mature. Cover the capstone script (the §13 answers + the protected interview lines), classification fundamentals (C4-1), and a cold SQL question (SQL block). Routed as C-blocks: Claude chat as interviewer with a structured brief, or a human peer where available.

**Firewall note (mirrors the CNN/SQL reconciliation):** none of the four blocks enters the PJM capstone. They exist because the interview funnel probes surfaces the capstone deliberately does not cover.

## What This Syllabus Does NOT Teach

Each omission below is a deliberate scope decision tied to `capstone_V5_5.md` v5.5. Yarden can quote each in interviews when asked. The 2026-06-10 review explicitly considered and **rejected** four proposed additions to this syllabus (LO3, LO2, SC3-learn, SC5-learn — see the v2.3 delta), so the boundaries below are re-ratified, not merely inherited.

**Graph neural networks (CS224W, GraphSAGE, message passing).** *"The lean capstone doesn't use a graph anywhere. The PJM network topology is a real structural feature of the market, but Western Hub is the canonical low-basis liquid hub — congestion is structurally small there. Forecasting at a constrained nodal price (e.g., within Dominion) would make a stronger case for a graph model; at Western Hub, gradient-boosted trees on the engineered feature catalog cover the same signal more transparently."*

**NBEATSx and other deep time-series architectures.** *"The peer-reviewed evidence on PJM-COMED — Olivares 2023 — shows the classical EPF baseline within ~5% of deep-learning forecasts on hub-level day-ahead price. Spending the curriculum budget on NBEATSx mechanics would have demonstrated a tool I wouldn't use in the capstone over a tool I would."* *(v2.3: a Month-3 neural-baseline build block — SC3-learn — was proposed and rejected with capstone SC3; the no-neural-challenger boundary was re-ratified. The shipped CNN plus the published evidence carry the "can you do DL / why no NN?" pair.)*

> **Note (v2.2 — DL reconciliation):** A standalone CNN mini-project (see *Supplementary Interview-Readiness Blocks*) was added to close the general "no shipped DL artifact" gap — taught/graded DL knowledge with no proving artifact. This is consistent with the exclusions in this section, not in tension with them: the exclusions concern the **capstone's** modeling choices, and the capstone still uses **no neural challenger** for the reasons above. The CNN is image classification (Fashion-MNIST/CIFAR-10), deliberately *not* a deep time-series model, and is *not* wired into the PJM project. Interview framing stays clean: *"I can build and train networks — here's a shipped CNN — and I chose gradient boosting for the capstone on the merits, not for lack of the alternative."*

**EnbPI, SPCI, and sequential conformal prediction generally.** *"CQR gives finite-sample per-interval marginal coverage guarantees under exchangeability, which is the property an interview reader is most likely to recognize and probe. The sequential variants handle non-exchangeability more aggressively but add conceptual surface area that distracts from the project's core argument. The reliability diagram makes the exchangeability violation visible, and the limitations section names it."* *(v2.3: an EnbPI learning block — SC5-learn — was proposed and rejected with capstone SC5. Single pre-committed reopen condition, anchored in capstone v5.5 §13: post-Elliott empirical coverage diverging > 10 pp from nominal at M3 → EnbPI returns as remediation and the Month-4 block revives at [APPLIED-AUTH]. Below that threshold, this boundary stands.)*

**Causal inference, DAGs, do-calculus, counterfactual ATE.** *"The capstone makes no causal claim. It makes a predictive claim with calibrated uncertainty. Causal inference is a separate discipline with a separate evidentiary bar, and conflating it with forecasting is one of the more common interview red flags. I deliberately stayed on the predictive side of that line."* *(v2.3: a standalone causal-inference interview block — LO2 — was proposed and rejected: Months 3–4 filler slack is committed to C4 + SQL, and the published SEM bootstrap-mediation work already carries the interview answer without a new block. Backlog-only.)*

**Mixed-integer linear programming, BESS arbitrage, Approximate Dynamic Programming, Pyomo, GLPK.** *"The capstone deliberately stops at the forecast. Wrapping a forecast in a battery-storage optimization or a trading layer either requires real risk infrastructure or it becomes a toy. A toy decision layer would have undermined the project's credibility. The forecast and its honestly-communicated uncertainty are the artifact."*

**MLOps stack — partially in, partially out.** The original v2 paragraph treated the entire MLOps surface as out-of-scope. Under v5.5 the boundary is more precise:

- *Now in scope at recognition / applied-recognition level (recognition-level theory; applied-recognition for the artifacts Yarden actually ships):* **Docker multi-stage build patterns** (Month 5 [APPLIED-REC]), **DagsHub-hosted MLflow tracking + Model Registry** (Month 4 [REC]), **Hugging Face Spaces deployment mechanics** (Month 5 [APPLIED-REC]), and **(v2.3) the thin GitHub Actions CI** that runs the capstone's §9.4 invariant tests on push — wiring at [APPLIED-REC]-level effort inside the M2 build, not a curriculum topic. Interview-quotable rationale: *"The capstone's MLOps surface is portfolio-grade — containerized marimo on Hugging Face Spaces, backed by a public DagsHub-hosted MLflow Model Registry, with invariant tests running on every push. This demonstrates fluency with the standard ML portfolio-hosting stack and gives hiring managers a publicly inspectable experiment history. It is not enterprise MLOps infrastructure."*
- *Remains explicitly out of scope:* **DVC** (rejected as introducing solo-developer overhead disproportionate to portfolio-grade reproducibility; superseded by pinned deps + tagged commit + committed Parquet snapshot), **Evidently AI** (no drift monitoring), **FastAPI** (no API layer — marimo notebook IS the user-facing layer), **CI/CD regression matrices, scheduled retraining cron, automated rollback machinery, drift gates** (the thin invariant-test CI is not a regression matrix — capstone v5.5 §13 holds that line explicitly). Interview-quotable rationale: *"DVC, Evidently, FastAPI, CI/CD regression matrices, and drift monitoring are enterprise MLOps boilerplate. AI-generated code handles them correctly in any real role with a real codebase. Spending portfolio time on them would have demonstrated boilerplate over judgment. The investment instead went into calibration, diagnostics, and interactive uncertainty communication — the parts of a forecasting system that are hard to get right and that signal DS judgment."*

**The full breadth of MIT 18.065 (matrix methods).** *"Selectively useful — SVD, PCA, condition number as a multicollinearity diagnostic. The Eckart-Young proof, low-rank approximation theorems, and special-matrix theory weren't needed for the capstone and didn't earn their place against the math-baseline timeline."* *(v2.3: a proposal to upgrade Month-0 SVD/PCA to [AUTH] and restore Eckart–Young — LO3 — was reviewed and rejected: Month 0 is the binding constraint, the capstone's use of SVD/PCA is diagnostic and [REC] covers it, and the claimed "senior-DS interview surface" is not the junior/mid funnel being targeted. If the Month-0 consolidation verdict flags the SVD story as shaky, remediation fires through the existing checkpoint channel — not through a pre-emptive depth upgrade.)*

---

*End of Accelerated DS Syllabus — v2.3.*
