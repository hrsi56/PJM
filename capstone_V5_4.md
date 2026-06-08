# Engineering Plan — PJM Western Hub Day-Ahead Price Forecasting Tool, v5.4 — Lean DS Capstone with Cloud-Backed Showcase

## 0. Ratified decisions (three; final)

Three decisions surfaced by expert review have been ratified by the project owner. They are final and binding on M1.

1. **Out-of-sample length vs. compute — RATIFIED: per-fold recalibration + ~450 test-days, mixed across regimes** (Section 5.1). Narrowing the training window to extend the test span is **rejected** — it would cost the pre-/post-Elliott regime span that the whole methodology depends on. The ~450-day, per-fold, mixed-regime design is the deliberate $0-compute balance.
2. **Regional gas basis — RATIFIED: TZ6 NNY + TETCO M3 + Henry Hub, with an automatic code-level fallback.** Henry Hub (`RNGWHHD`) is the guaranteed backstop; the two regional series are best-effort. The pipeline must implement a **graceful degradation**: if a regional series is missing for more than 48 hours, the pipeline switches that input exclusively to Henry Hub, **logs the substitution**, and continues — it does not crash (Section 3, Section 4 gas markers, R-3).
3. **Western Hub temperature zone — RATIFIED: load-weighted aggregate over the Western-Hub-adjacent zones in the catalog (AEP, APS, Dominion, PEPCO, PPL, MetEd)** (Section 4.1). A single station is **rejected** — it injects micro-climate noise irrelevant to the hub.

---

## 1. Purpose and Scope

This is a portfolio-grade probabilistic forecasting project built for an industry Data Scientist hiring audience. The tool produces, on demand, an hourly forecast of the PJM Day-Ahead Energy Market Locational Marginal Price (LMP) at the **Western Hub** trading hub for the **next operating day (24 hourly values, D+1)**, with calibrated 50% / 80% / 95% prediction intervals.

The target is the next operating day specifically because that is what the Day-Ahead market clears: PJM's DA bid/offer window closes at **11:00 EPT on D-1, with results posted by ~13:30** (PJM Manual 11). At that gate the prices for all 24 hours of D+1 are forecast, and every lagged-price and gas-marker feature is legitimately known. A multi-day-ahead target would force the model to consume values that do not yet exist at forecast time (a 24h lag for `D0+2` is a price that clears the night after the forecast is made) — look-ahead leakage in the one project whose centerpiece is a leakage audit. The 24h horizon keeps the feature catalog clean and the methodology airtight. (See Section 5.2.)

*Why PJM, for a non-energy interviewer:* the domain is a methodology vehicle, not a domain commitment — a real, noisy, public time-series with documented regime shifts and a published benchmark literature, chosen so the work is verifiable rather than a toy. The methodology transfers to any consequential time-series. The full interview answer is prepared in interview prep, not here.

The constraints, named up front:

- **Interactive marimo showcase, statically deployed.** A reactive marimo notebook (run in **server mode**, not WASM) with live UI sliders (cold-weather alert toggle, feature-value perturbation on the headline domain feature, quantile-level selector), containerized via a multi-stage Dockerfile and deployed to Hugging Face Spaces free tier. Data is a fixed weekly snapshot manually refreshed during the active application phase, not live API pulls during user sessions.
- **Cloud-backed experiment tracking via DagsHub-hosted MLflow.** Public read-only MLflow UI linked from CV and LinkedIn. No DVC. Data reproducibility achieved via a single committed Parquet snapshot plus pinned dependencies plus tagged commit — not via a data-versioning subsystem.
- **Single model.** A monolithic LightGBM quantile ensemble (nine native quantile heads). No model menagerie, no neural challenger, no blending, no shadow logging.
- **CQR-calibrated quantile intervals.** Conformalized Quantile Regression (Romano et al. 2019) on the four symmetric quantile pairs, with isotonic monotonicity enforced last. Per-interval finite-sample marginal coverage. No sequential conformal apparatus (EnbPI / SPCI) and no Giacomini-White gating.
- **Forecast + diagnostic report, not a trading product.** No LONG/SHORT/FLAT layer, no BESS optimization, no bid-reference logic, no utility/risk-aversion parameter, no auto-generated natural-language commentary.

The audience is a Data Science hiring manager. The artifact must demonstrate: domain understanding, defensible feature engineering, correct time-series validation, sensible model choice, explainability, cloud-backed experiment tracking via a publicly inspectable MLflow UI, honest uncertainty communication, and a polished interactive interface deployed to a recognizable ML hosting target. Total compute footprint: trains end-to-end in minutes on an Apple Silicon M3, no GPU required. Total hosting cost: $0 (Hugging Face Spaces free tier + DagsHub free tier).

**v5.2 → v5.3 delta.** All changes are research-adjudicated; none reopens a defended boundary; several further reduce risk. Grouped:

*Build-blockers resolved:*
1. **Temperature normal defined** as a train-only day-of-year × hour **Fourier climatology** (Section 4.1), removing the unsourced "30-yr normal." No external climatology vendor, no station→zone crosswalk.
2. **Walk-forward folds pinned** to a fixed **mixed-regime** 5-fold scheme (2 pre-Elliott, 3 post-Elliott; Fold 5 ends at most-recent data) so the regime-stratified table is fillable from eval folds (Section 5.1). Wording corrected to "≥3 post-Elliott, ≥2 pre-Elliott."
3. **UTC indexing + DST handling** mandated (Section 4.0 / 5.2): index on `datetime_beginning_utc`, compute all lags in UTC so 24h = 24 rows always, respect `RepeatedHourFlag`; the spring-forward / fall-back days carry 23 / 25 hour-ending values in local time.
4. **Load-forecast vintaging** fixed (Section 3 / 4): use the vintage known at the D0 11:00 gate from the historical all-vintages feed, never the latest revision.
5. **Target = verified DA LMP feed** (`rt_da_monthly_lmps` / Settlements Verified Hourly LMPs), not the preliminary snapshot (Section 3).

*Methodology:*
6. **Conformal = CQR** on the four symmetric pairs + median, not per-level quantile shifting (Section 6.2). Ordering-preserving by construction; isotonic-last becomes a cheap safety net. Coverage claim is per-interval marginal, not joint.
7. **CP-2 gate** changed from a hard ≥15% pinball threshold to a **one-sided Diebold-Mariano significance gate (p<0.05)** with 15% as a reporting target and a mandatory honest-fallback (Section 7.1 / CP-2).
8. **DM on mean pinball loss** added alongside DM on median MAE (Section 7.1), to test probabilistic skill, not just point accuracy.
9. **Leakage-vector audit** extended (Section 5.2): closed-left rolling windows, `cold_weather_alert` gate-timing, `post_elliott` time-collinearity.
10. **Calendar features** added: `is_day_after_holiday`, `is_bridge_day`, 3-level day-type (Section 4).

*Factual corrections (Section 2):*
11. Energy price cap corrected to **$2,000/MWh cost-based offer cap (FERC Order No. 831) + stacked $850 reserve penalty factors ≈ $3,700/MWh** theoretical max; the "$4,100/MWh / 2024 Reserve Price Formation rulemaking" claim was false and is removed.
12. Data-center attribution **re-sourced** to the Monitoring Analytics (IMM) counterfactual for the correct auction year (~61%, 2027/2028 BRA Scenario 3); the capacity arc annotated ($329.17 cleared *at the FERC cap*; 2025/2026 RTO was $269.92).

*Deploy specifics (Section 9.2):* marimo **server mode** (not WASM); secrets via HF Spaces Secrets; registry-fetch-then-bundle-fallback confirmed; the GitHub Actions keep-alive cron **auto-disables after 60 days of repo inactivity** — flagged, not relied upon.

**v5.3 → v5.4 delta.** Section 0 ratified (three decisions final); plus three owner-directed strengthenings, none reopening a boundary: (1) **graceful gas-data fallback** — automatic, logged switch to Henry Hub when a regional series gaps >48h, no crash (Section 0 item 2, Section 4 gas markers, R-3); (2) **targeted invariant tests** — a small set of property tests on the invariants that must hold (CQR+isotonic monotonicity, closed-left rolling windows, UTC/DST lag alignment), added as Section 9.4 and wired into CP-1/CP-2, explicitly *not* a regression matrix (Section 13 reworded to keep that boundary while admitting invariant tests); (3) **dynamic OOD indicator** — the precomputed slider lookup carries a per-cell in/out-of-distribution flag, lighting a UI banner only when the user lands on an out-of-distribution combination, at zero live compute (Section 9.2).

## 2. PJM Western Hub: Market Context and Regime Shifts

PJM is a hybrid capacity + energy market. PJM's cost-based energy offer cap is **$2,000/MWh** (FERC Order No. 831); with stacked reserve penalty factors the theoretical maximum real-time energy price is about **$3,700/MWh**. (There was no 2024 "Reserve Price Formation" rulemaking; PJM's reserve price-formation reforms ran 2019–2022 and the penalty factors reverted to $850/$300. A "$4,100/MWh" figure exists only as a 2026 stakeholder proposal, not filed at FERC.) PJM DA prices are usually more stable and less bimodal than the most volatile US markets, with the dramatic exceptions being correlated generator failures during extreme weather.

**Western Hub** is the canonical liquid, low-basis PJM trading hub. Total LMP decomposes additively as

> `total_lmp_da = system_energy_price_da + congestion_price_da + marginal_loss_price_da`

(SMP + MCC + MLC). At Western Hub the marginal congestion component (MCC) is structurally small and rare, the marginal loss component (MLC) is small and smooth, and the bulk of total-LMP variance comes from SMP. This is the empirical foundation for the **monolithic target** decision in Section 6.

The model must absorb two structural non-stationarities. These are not historical curiosities — they are live drivers of current pricing behavior.

### 2.1 Winter Storm Elliott and the post-Elliott fuel-security risk premium

At the peak of Winter Storm Elliott (December 23–24, 2022), roughly 24% of PJM's generating capacity was unexpectedly offline — the precise figure is **45,950 MW (23.2%) on December 24** — with gas-fired units accounting for ~70% of unplanned outages (PJM *Winter Storm Elliott Event Analysis & Recommendation Report*, July 2023). Total Capacity Performance non-performance charges reached **~$1.8 billion**, about **45%** of the ~$4 billion in capacity revenue for that delivery year (and ~83% of the ~$2.2 billion earned by the ~750 penalized resources). The structural consequence is a persistent **fuel-security risk premium** that gas-fired plants now bid into the DA stack during cold-weather alerts. This appears in the price record as a regime shift starting on or around **2023-01-01**.

The model carries two explicit features encoding this regime: `post_elliott` (binary, true on or after 2023-01-01) and `cold_weather_alert` (issued by PJM ahead of forecast cold events). Both are first-class features in the catalog (Section 4) and the structural failure mode they hedge is the primary risk in the Risk Register (Section 11, R-1).

### 2.2 Hyperscaler data-center load growth

Cleared PJM RTO capacity prices moved from **$28.92/MW-day** in the 2024/2025 BRA to **$329.17/MW-day** in the 2026/2027 BRA — roughly an **11× increase**. Two precision points: the 2026/2027 figure **cleared at the FERC-approved price cap** (Docket ER25-1325, cap $329.17 / floor $177.24), not as a free-market clear; and the intermediate 2025/2026 BRA cleared at **$269.92/MW-day RTO** (with Dominion at $444.26 and BGE at $466.35). Monitoring Analytics (the Independent Market Monitor) estimated that forecast data-center load raised **2027/2028** BRA revenues by **~61%** (Scenario 3); for the 2025/2026 BRA the data-center counterfactual implies an even larger share (Scenario 88, ~+174%). PJM's long-term load forecast for the Dominion zone moved from ~5,700 MW (2022 forecast, target 2037) to >20,000 MW from data centers alone (2025 forecast, target 2037). This is a slow-moving regime shift in supply-tightness expectations that propagates into DA energy prices through scarcity intervals.

**Implication for the model.** The price distribution is not stationary. The system therefore mandates walk-forward CV that spans both regimes, explicit regime indicator features, and per-regime calibration checks — all covered below. The 2019–2025 training window (Section 3) spans both regimes while keeping the pre-Elliott era from dominating the data the forecast actually serves.

## 3. Data Sources

All data is free. The training window is **2019–2025 (~6 years)** — long enough for ample data and a mixed-regime fold scheme, recent enough to keep the pre-Elliott era from dominating, and small enough on a 16 GB machine. (Hub-level DA LMP is available back to ~2010–2011 via the archive endpoints; the window is a deliberate scope choice, not a data limit.)

| Source | Content | Exact identifiers / gotchas |
|---|---|---|
| **PJM Data Miner 2 via `gridstatus`** | DA LMP w/ SMP/MCC/MLC decomposition; 7-day load forecast; `da_temperature_sets` (DA temperature forecasts) | **Target = verified feed** `rt_da_monthly_lmps` (Settlements Verified Hourly LMPs), not preliminary `da_hrl_lmps`. Western Hub `pnode_id = 51217`. For dates older than ~731 days the whole archive downloads **before** location filtering (PJM limitation) — chunk by `frequency="365D"`; expect slow historical pulls. **`da_temperature_sets`**: DM2 feed `da_tempset`; gridstatus has **no named getter** — pull the DM2 API directly (API key + `da_tempset` feed); confirm archive reaches 2019 and the schema at M1. |
| **EIA API v2** | Natural-gas daily spot — Transco Z6 NNY, TETCO M3 (PJM-West markers), Henry Hub (national backstop) | **Henry Hub daily = `RNGWHHD`** (robust, history pre-2019). TZ6 NNY and TETCO M3 are EIA "Natural Gas Spot Prices" series — confirm exact `series_id`; **regional daily history may have post-2024 gaps** (some regional series now sourced via LSEG/NGI). Henry Hub guaranteed; regional best-effort (Owner item 2). |
| **PJM 7-day load forecast** | RTO total + Western-Hub-adjacent zones | **Vintaging matters.** Use the historical all-vintages feed `load_frcstd_hist` (`get_load_forecast_historical`) and select the **vintage known at the D0 11:00 gate**, never the latest revision (`get_load_forecast` is today-only/latest — do not use for training). The historical feed carries fewer regions — confirm Western/RTO present. |
| **NOAA NDFD** | Gridded forecast temperature/dewpoint/wind | **Secondary / droppable.** Historical *forecast* archive is incomplete, GRIB2-gridded, needs spatial extraction. Provides forecasts (KFT-legitimate); **realized actuals must never be substituted** (look-ahead leakage). Drop if its forecast archive is not clean; PJM `da_tempset` is primary. |

**Weather is PJM-native by default.** PJM's `da_temperature_sets` are the temperature *forecasts* PJM used at the DA gate — KFT by construction, zone-aligned, archived on Data Miner 2. They are the primary weather source and the basis of the headline feature (Section 4.1); no external climatology vendor is required because the seasonal normal is fit in-pipeline.

`gridstatus` handles Data Miner 2 pagination across the historical archive. Secondary PJM feeds present in v4 (marginal-value shadow prices, virtual transactions, solar forecast, reserve market results, generation outage feeds) are dropped from this version.

## 4. Feature Catalog

Approximately 25 features, grouped by source. Every feature is classified as either **known-at-forecast-time (KFT)** — publicly available before the DA bid gate — or **lagged-only (LAG)**. This distinction is the foundation of the leakage audit in Section 5.2. Because the target is the next operating day (D+1, Section 1), every LAG feature below is legitimately available at forecast time: the most recent settled DA price is for day `D0`, and a 24h lag of a `D0+1` target lands on `D0`.

### 4.0 Time indexing and DST (mandatory)

The entire pipeline is indexed on **UTC** (`datetime_beginning_utc`, exposed by the Data Miner feeds and sorted on by gridstatus). All lags are computed in UTC so that 24h = 24 rows, 168h = 168 rows, always. PJM uses **hour-ending (HE) Eastern Prevailing Time** for display; the spring-forward day has **23 HE values** (HE03 skipped) and the fall-back day has **25** (a repeated HE02 — respect gridstatus's `RepeatedHourFlag`). Hours are never dropped or duplicated ad hoc. The user-facing "24-value D+1 vector" therefore renders as 23 or 25 values on the two DST-transition days; the artifact publishes UTC-derived HE with an explicit DST note. This corrects an integer-lag corruption that otherwise hits ~2 shoulder-season days per year.

**Calendar (KFT)** — hour-of-day, day-of-week, month, NERC holiday flag, **`is_day_after_holiday`**, **`is_bridge_day`**, a **3-level day-type {weekday, Saturday, Sunday/holiday}** (mirroring the Sat/Sun/Mon vs Tue–Fri asymmetry standard in EPF naïve baselines), daylight-saving transition flag, summer-peak / winter-peak season flags.

**Load forecast (KFT)** — PJM load forecast (bid-gate vintage, Section 3) for RTO total and Western Hub-adjacent zones (AEP, APS, Dominion, PEPCO, PPL, MetEd). Same-hour-tomorrow value and rolling 24h forecast level.

**Weather forecast (KFT)** — DA temperature sets from PJM `da_temperature_sets` (primary); NOAA NDFD where used (secondary / droppable, Section 3).

**Lagged price (LAG)** — lagged total DA LMP at Western Hub at lag-24h, lag-48h, lag-168h; lagged SMP, MCC, MLC at the same lags as input features (not separate targets).

**Rolling price statistics (LAG)** — 7-day and 30-day rolling mean and standard deviation of DA LMP; 168h rolling 5/50/95 quantiles. **All rolling windows use a strict `t−1` right edge and are closed-left**, so the current target is never inside its own feature window (leakage guard, Section 5.2).

**Gas markers (LAG)** — TZ6 NNY, TETCO M3, and Henry Hub daily spot closes. Daily close is **broadcast across all 24 hours** of D+1 (standard treatment; public intraday hourly gas does not exist). Use the close at **lag `t−1` relative to the operating day** (the D0 close, known at the 11:00 D-1 gate); exclude any same-day-as-operating-day gas value as look-ahead. **Graceful fallback (ratified):** if a regional series (TZ6 NNY or TETCO M3) is missing for >48h, the pipeline substitutes Henry Hub (`RNGWHHD`) for that input, logs the substitution, and continues — it never crashes on a regional gap (R-3).

**Regime indicators (KFT — known a day ahead)** — `post_elliott` binary; `cold_weather_alert` flag, using only the alert state **known at the D0 11:00 gate** (confirm the PJM Cold Weather Advisory/Alert for the target day is issued before the gate; if issued after, it is look-ahead — Section 5.2).

### 4.1 Domain-insight feature: `temperature_deviation_from_normal` (primary), `scarcity_stress` (secondary)

The **primary headline feature** is the deviation of forecast temperature from a seasonal normal:

> `temperature_deviation_from_normal(d,h) = da_temperature_sets_forecast(d,h) − normal(DOY(d), h)`

where `normal(·)` is a **train-only seasonal climatology** — *not* an external 30-year product. It is fit by low-order Fourier/harmonic regression:

> `normal(d,h) = β0 + Σ_{k=1..3}[a_k sin(2πk·DOY/365.25) + b_k cos(2πk·DOY/365.25)] + Σ_h γ_h·1[hour=h] + (k≤2 annual-harmonic × hour-of-day interaction)`

i.e. 3 annual harmonics, 24 hour-ending dummies, and a low-order annual×diurnal interaction (~30–40 coefficients), estimated by OLS. **Fitting rules:** fit **on training rows only**, **re-fit per fold** (the climatology never sees validation/test data; refit cost is negligible). **Default fit-series: the historical `da_temperature_sets` forecast series itself** — this keeps the feature self-consistent (forecast minus the forecast's own seasonal-diurnal mean), needs no realized-temperature source and no station→zone crosswalk, and centers out systematic forecast bias by construction. *(Optional refinement at M1: if a clean realized PJM zonal temperature series is readily available, fit the normal on realized actuals for a more physical baseline; not required.)* The fitted coefficient vector is persisted **inside the `mlflow.pyfunc` artifact** (it is part of the feature pipeline); at inference the deviation is computed with no external call. The temperature zone defining "Western Hub" is Owner item 3 (default: load-weighted aggregate over the adjacent zones above). Rationale: standard EPF practice (Lago et al. 2021; Jędrzejewski/Marcjasz/Weron 2021) builds seasonal structure from the training series, not an external climatology.

A **secondary candidate** encodes the joint-high scarcity condition as a true composite:

> `scarcity_stress = relu(gas_z) · relu(load_z)`

where `gas_z` and `load_z` are gas spot and forecast load standardized on **train-set statistics only** (mean/std computed on training data, applied at inference — a fit-time statistic that must not leak). The product fires only when *both* exceed their means — expensive gas during high demand, the actual scarcity corner. Its interview defense (why `relu`, why a product) must be rehearsed if selected.

**`gas_to_load_ratio` is deliberately dropped as the headline.** A ratio `gas / load` is maximized by high gas and *low* load — the opposite of the joint-high scarcity condition; it would read as inverse-load (or just gas) in the dependence plot. A ratio cannot encode an AND condition; a product can.

**Selection rule.** Both `temperature_deviation_from_normal` and `scarcity_stress` enter the catalog from M1. The headline is confirmed empirically at M3 from SHAP: whichever surfaces in the SHAP top-5 with a clean, mechanism-consistent dependence plot becomes the headline the artifact and the marimo slider feature. Default expectation is `temperature_deviation_from_normal`; `scarcity_stress` is promoted only if SHAP earns it. A third fallback, `forecast_uncertainty_proxy` (rolling std of historical load-forecast errors by hour-of-day and season), remains available.

## 5. Methodology: Walk-Forward CV and Data Leakage Audit

The two methodology choices below are the top-three sources of overstated results in the EPF literature (Lago et al. 2021) and the top-two questions a DS interviewer asks about a time-series project. Both are spelled out explicitly so they live in the final report.

### 5.1 Walk-forward expanding-window CV with 24h embargo — pinned mixed-regime scheme

Validation uses **expanding-window walk-forward CV with five folds**, each with a **24-hour embargo gap** and a **90-day eval block**, anchored so the eval blocks span **both regimes** (≥2 pre-Elliott, ≥3 post-Elliott) rather than concentrating in the recent tail. Concentrating all five blocks post-Elliott would make the regime-stratified table (Section 8.3) un-fillable from eval folds and would violate the EPF principle of testing across regimes (Lago et al. 2021, whose benchmark uses a long, multi-regime out-of-sample span).

**Pinned scheme** (training window 2019-01-01 → 2025-12-31; each fold trains on all data up to its eval start minus the 24h embargo; expanding — each training set is a superset of the prior):

| Fold | Eval block | Regime |
|---|---|---|
| 1 | 2021-07-01 → 2021-09-28 | pre-Elliott |
| 2 | 2022-04-01 → 2022-06-29 | pre-Elliott |
| 3 | 2023-07-01 → 2023-09-28 | post-Elliott |
| 4 | 2024-07-01 → 2024-09-28 | post-Elliott |
| 5 | 2025-10-01 → 2025-12-29 | post-Elliott; ends at most-recent data |

The regime-stratified error table is then populated from eval folds for both regimes. Finer pre-Elliott stress rows (e.g., a specific cold snap such as Elliott itself) are sourced from **training-period backtesting** — re-predicting in-window days with a model trained only on data prior to those days — which is standard and not an out-of-sample claim, so it does not contaminate the headline metrics.

**Recalibration cadence.** At each fold boundary, re-fit the LightGBM heads, the CQR thresholds, **and** the climatology; within a 90-day block, predict D+1 sequentially without peeking. Per-day recalibration (epftoolbox gold standard) is out of scope under the $0 compute ceiling; **per-fold recalibration is the stated lean substitute** (Owner item 1). Fold 5 (Oct–Dec 2025) supplies a winter-stress row; Folds 2 and 5 add spring/late-fall to balance the summer-heavy Folds 1/3/4.

**Why the 24h embargo suffices given 48h/168h lags.** The embargo's only job is to break the autocorrelation adjacency between the **last training target** and the **first validation feature**. The D+1 target uses lags of the *known historical price*; a 168h-lagged feature of a validation row reaches back into the training period — but using a known, already-published historical price as an input feature is not leakage; it is exactly what the production model has at the bid gate. A 24h embargo (one full target horizon) removes the only true adjacency. Document this reasoning verbatim for the interviewer.

### 5.2 Data leakage audit

Every feature in Section 4 is classified KFT or LAG with explicit justification. The D+1 target makes the LAG features legitimate: no LAG feature consumes a value dated on or after the target day.

- **KFT features** are confirmed published before the D0 11:00 gate: PJM load forecast (bid-gate vintage from `load_frcstd_hist`, never the latest revision), `da_temperature_sets` (forecasts, not actuals), NOAA NDFD where used (forecasts), calendar features (trivially known), `post_elliott` (static date threshold), `cold_weather_alert` (alert state known at the gate).
- **LAG features** use `t-1` or earlier relative to the forecast origin `D0`. Rolling statistics use a strict `t−1` right edge, closed-left.
- **Three audited vectors:** (i) rolling-quantile features computed closed-left so the target is never in its own window; (ii) `cold_weather_alert` issuance confirmed before the D0 gate (otherwise look-ahead); (iii) `post_elliott` leaks nothing as a constant but is perfectly collinear with time inside any all-post-Elliott fold — a further reason for the mixed-regime scheme (Section 5.1).
- **Weather-source guard.** A smoke test asserts weather features are forecasts (KFT), not realized actuals (R-2).
- **Target transform.** Do **not** apply log/Box-Cox to the target — DA LMP can be negative. Pinball loss, the CQR score, the additive conformal shift, and monotone rearrangement are all location-equivariant and behave correctly with negative targets and zero-crossing intervals (Section 6). If variance stabilization is ever wanted, use `asinh` (defined on ℝ); with LightGBM quantile heads no transform is needed.

This audit is the single best preparation against the standard interview question *"How did you make sure your features weren't leaking?"*

## 6. Model: LightGBM Quantile Ensemble with CQR Calibration

### 6.1 Single monolithic LightGBM, packaged as one model

The target is the hourly total DA LMP at PJM Western Hub for the next operating day. The model is a **single LightGBM regressor family** predicting the nine native quantiles `{p2.5, p5, p10, p25, p50, p75, p90, p95, p97.5}` — one head per level, each fit independently with `objective='quantile'`.

**The champion is one artifact, not nine.** The nine quantile boosters, the **fitted climatology coefficients**, the **CQR thresholds** (Section 6.2), the final isotonic step, and the feature pipeline are wrapped in a single custom `mlflow.pyfunc.PythonModel` exposing one `predict` that returns the nine calibrated, monotone quantiles. This is what is logged, registered, tagged `Production`, and loaded — one model, one Production tag, one `models:/pjm_forecast/Production` load path.

### 6.2 CQR calibration, and the post-processing order

LightGBM's native quantile heads have no finite-sample coverage guarantee, and independently fit heads can cross. Both are handled in order:

1. **Raw LightGBM** produces nine quantile predictions per row (may cross).
2. **CQR (Romano, Patterson & Candès 2019)** on each symmetric pair. For each interval `(lo, hi) ∈ {(p2.5,p97.5),(p5,p95),(p10,p90),(p25,p75)}` at miscoverage `α ∈ {0.05, 0.10, 0.20, 0.50}`, compute on the calibration set the conformity score `E_i = max{ q̂_lo(x_i) − y_i , y_i − q̂_hi(x_i) }`, take the threshold `Q_(lo,hi)` as the `⌈(n_cal+1)(1−α)⌉`-th smallest `E_i`, and form `[q̂_lo − Q, q̂_hi + Q]`. The median (p50) is left un-shifted for the point forecast.
3. **Isotonic regression (last)** across the nine conformalized endpoints per row, as the final ordering guard.

**Calibration set:** the most-recent slice of each fold's training partition, held out and disjoint from both proper-training and eval.

**Why CQR, and why isotonic is still last.** CQR shifts the lower quantile down and the upper quantile up by the same nonnegative `Q`, so it **cannot re-cross** a non-crossing pair — the isotonic step becomes a cheap safety net rather than a load-bearing correction (per-level shifting, by contrast, moves each head independently and *can* re-introduce crossings *after* conformal, perturbing the coverage conformal just guaranteed). CQR is also the most recognizable split-conformal construction, which is the interview-defensible choice. Monotone rearrangement (Chernozhukov, Fernández-Val & Galichon 2010) weakly improves estimation error; the reliability diagram (Section 8.4) verifies the final intervals hold coverage. **Coverage claim:** per-interval **marginal** coverage ≥ 1−α, finite-sample, distribution-free under exchangeability (Angelopoulos & Bates). Do **not** claim joint/simultaneous coverage across the three intervals — that is a different, smaller guarantee; state this caveat.

Stated in the final report verbatim, for interview defense:

> *CQR provides finite-sample marginal coverage guarantees under exchangeability. The walk-forward CV mildly violates exchangeability — the post-Elliott regime is not exchangeable with the pre-Elliott regime — so empirical coverage may diverge from nominal on regime-shift folds. This is documented in the reliability diagram (Section 8.4).*

That paragraph is the heart of the project's uncertainty story.

### 6.3 Why monolithic, not decomposed

PJM publishes LMP as SMP + MCC + MLC. Forecasting each component separately is intuitively attractive but rejected: (1) at Western Hub the congestion component is structurally small and rare; the bulk of total-LMP variance comes from SMP, which a well-featured boosted-tree model already captures; (2) forecasting a smooth ~$0.30/MWh MLC series independently and adding it to a ~$40/MWh SMP forecast adds reconciliation complexity for negligible win; (3) standard PJM-focused academic work forecasts total LMP at a hub directly. A future version forecasting a constrained nodal price would have a stronger case for separate MCC modeling. The decomposition is used as **input features** (lagged SMP, MCC, MLC, rolling MCC frequency-of-nonzero), not as separate output targets.

## 7. Benchmarks and Statistical Testing

LightGBM is compared against:

- **Seasonal-naïve 24h** — forecast = price 24 hours ago. Day-over-day persistence baseline (the stronger baseline at a liquid hub).
- **Seasonal-naïve 168h** — forecast = price 168 hours ago. Week-over-week seasonality baseline.
- **Ridge regression** with the same engineered feature catalog. Classical baseline; checks whether the boosted-tree complexity earns its place.

Metrics: **MAE** on the median forecast, **mean pinball loss** across the nine quantiles, **empirical coverage** at the 50% / 80% / 95% nominal levels.

### 7.1 Diebold-Mariano tests (point and probabilistic)

Two DM tests are reported, both against seasonal-naïve-24h:

**(a) Point accuracy — DM on median absolute error.** Loss differential on the daily median forecast.

**(b) Probabilistic skill — DM on the mean pinball-loss differential.** Per hour-ending `h` and day `t`, per quantile `τ`, pinball loss `L_τ(t,h) = max(τ·(y−q̂_τ), (τ−1)·(y−q̂_τ))`; day-level model loss `L_model(t) = (1/24)Σ_h (1/9)Σ_τ L_τ(t,h)`; differential `d_t = L_model(t) − L_naive(t)`. One-sided DM `= mean(d)/sqrt(LRV(d)/N)`, **LRV via Newey–West** with lag truncation `⌊N^{1/3}⌋`; reject (model better) if DM < −1.645. Use the **multivariate, pooled-across-hours** form as the headline (matching Lago et al.'s daily-vector framing, avoiding 24 multiple-testing problems); report per-hour DM only as a diagnostic. Averaging pinball across the nine levels is a discrete approximation to CRPS (the standard probabilistic proper score); explicit CRPS is optional.

The probabilistic DM (b) is the one aligned with the project's thesis — it tests whether the *intervals* beat persistence, not just the median.

## 8. Diagnostics and Explainability

These diagnostics are the heart of the portfolio artifact.

### 8.1 SHAP on the median model

- Global SHAP summary plot for the `p50` head on the validation tail; top-10 ranking; two dependence plots: `cold_weather_alert × hour-of-day`, and **headline domain feature × `cold_weather_alert`** (`temperature_deviation_from_normal` by default, or `scarcity_stress` if SHAP selects it at M3).
- **Scoping (state explicitly):** SHAP on the p50 head explains the **central tendency**, not interval *width*. Interval width is driven by the inter-quantile spread and the CQR shift `Q`. If asked "what drives your uncertainty," the answer is the spread + `Q`, not the p50 SHAP. SHAP-at-M3 also performs the headline-feature selection (Section 4.1).

### 8.2 Permutation importance

Permutation importance on the validation tail via `sklearn.inspection.permutation_importance`. SHAP attributes predictions in expectation; permutation importance measures degradation under shuffling. Reporting both signals maturity and surfaces any SHAP-vs-permutation rank disagreement.

### 8.3 Regime-stratified error analysis

A table of MAE and mean pinball loss on subsets: all validation; `cold_weather_alert` days; non-alert days; post-Elliott (≥2023-01-01); pre-Elliott; weekday; weekend. **`n_obs` is reported prominently for every row, and small subsets (notably `cold_weather_alert` days, which may be only tens of hours) carry bootstrap confidence intervals** (the Month-1 bootstrap) and are read qualitatively, not as precise point estimates. Both regimes are populated from the mixed-regime eval folds (Section 5.1); finer pre-Elliott stress rows come from training-period backtesting. If MAE on alert days is materially worse, the report says so and ties it to the exchangeability paragraph (Section 6.2).

### 8.4 Reliability diagram

Empirical vs. nominal coverage at 50% / 80% / 95% on the validation tail, plotted for **both the raw LightGBM heads and the final CQR-adjusted, isotonic-enforced intervals**. This doubles as the empirical check that isotonic-last did not degrade coverage. **Bounded-target tail caveat:** the DA energy price is bounded near $3,700/MWh; on the rare days realized prices press the cap, the upper conformity residuals are truncated and the p97.5 interval can under-cover conditionally. At Western Hub this is negligible (prices almost never approach the cap); it is documented as a known tail caveat rather than engineered around (a cap-aware tail would reopen scope). Marginal coverage is preserved on average; the concern is conditional coverage on cap-pressing days, which the stratified table surfaces.

## 9. Engineering Infrastructure

### 9.1 Cloud-Backed Experiment Tracking via DagsHub-Hosted MLflow

MLflow runs against a remote tracking URI pointing to a public DagsHub repository. Runs log hyperparameters, per-fold metrics (MAE, pinball loss across all nine quantiles, both DM statistics vs. seasonal-naïve), and artifacts (SHAP summary, permutation tables, reliability diagrams, the regime-stratified table). The champion — the single `mlflow.pyfunc` (Section 6.1) wrapping the nine heads, climatology coefficients, CQR thresholds, and isotonic step — is registered and tagged (`Staging`, `Production`), decoupling the model from the app code (subject to the bundled fallback in Section 9.2). The DagsHub repo is publicly viewable; README, CV, and LinkedIn link the MLflow UI so hiring managers inspect the full experiment history (≥5 tuning runs with clear progression) without cloning. The public experiment history is the registry's primary marketing value, inspected independently of the deployed demo. **Data versioning is intentionally lightweight** — a single committed Parquet snapshot at a tagged commit; DVC was evaluated and rejected (solo-developer overhead disproportionate to portfolio reproducibility); pinned deps + tagged commit + committed snapshot give equivalent reproducibility with less operational risk.

### 9.2 Containerized Marimo Showcase on Hugging Face Spaces

The presentation layer is a reactive marimo notebook **run in server mode** (`marimo run` inside the container — **not** WASM/Pyodide, which cannot reliably serve the bundled Parquet lookup or the LightGBM artifact), containerized via a multi-stage Dockerfile (base layer with locked dependencies; application layer with notebook code, the data snapshot, the precomputed slider lookup, and the bundled champion model) and deployed to Hugging Face Spaces free tier (Docker SDK). HF free `cpu-basic` = **16 GB RAM, 2 vCPU, 50 GB non-persistent disk**.

On container startup the app:

1. **Loads the champion — registry-first, bundle-fallback.** It tries to fetch the champion from the DagsHub-hosted MLflow Registry via the production URI (`models:/pjm_forecast/Production`), reading the DagsHub/MLflow token from an **HF Spaces Secret** (Space settings → Variables and secrets; never baked into the image). On *any* failure (network, auth, rate-limit, registry outage), it falls back to the champion **bundled in the image**. The demo never hard-depends on a live registry fetch.
2. Loads the bundled weekly data snapshot (`data/snapshot.parquet`, refreshed manually each Monday during the application phase). No live calls to PJM/EIA/NOAA during user sessions.
3. Renders the next-day (24-hour, UTC-derived HE) forecast with a quantile fan chart, applying the model's calibrated, monotone output (the CQR-then-isotonic pipeline is internal to the `pyfunc`).

**Interactive sliders:** `cold_weather_alert` toggle; **headline-feature perturbation** (`temperature_deviation_from_normal`, or `scarcity_stress` if selected at M3); quantile-level selector (50/80/95, annotating the reliability diagram's empirical coverage for that level).

**Slider updates are served from a precomputed lookup, not live compute.** The slider inputs are low-dimensional and mostly discrete; at build time the forecast curve and SHAP attributions are precomputed over the discretized grid (`cold_weather_alert` binary × headline-feature grid (~20–50 points) × 3 quantile levels) and stored as a Parquet in the image. On interaction the app indexes into the lookup — instant, zero live compute, guaranteed on the free tier. This removes the single most uncertain dependency (live TreeSHAP on a shared CPU). The M4 spike validates that the grid covers the interaction space and indexing is instant; it no longer measures live-compute latency. **The slider is a *ceteris-paribus sensitivity probe*, with a dynamic out-of-distribution indicator.** Perturbing temperature while load/gas stay fixed produces physically OOD inputs, so it shows model sensitivity, not a realistic joint forecast. Beyond a static UI label, the precomputed lookup carries a **per-cell in/out-of-distribution flag** computed at build time (a grid cell is flagged OOD when its slider combination is sufficiently far from any real training observation — e.g., by Mahalanobis or nearest-neighbour distance in the perturbed features); when the user lands on a flagged cell the UI lights a small banner ("out-of-distribution combination — model is extrapolating"). This is zero live compute (the flag is read from the same lookup as the forecast and SHAP) and demonstrates that the limitation is not merely known but engineered around. CPU Upgrade ($0.03/hr ≈ $22/month) is reserved for cold-start mitigation only, not SHAP.

**Sleep and keep-alive.** HF free tier sleeps after 48h of inactivity (~20–30s cold-start on the next visit; the visit itself restarts it). A GitHub Actions cron *can* ping the Space every 12h — **but scheduled workflows auto-disable after 60 days of repo inactivity** (GitHub docs), so the cron will silently die in a quiet stretch. Do not rely on it to prevent sleep: either commit to the repo at least every 60 days, or accept the sleep and keep the README "~30s first-load on cold start" disclaimer as the defensible backstop. A standalone CLI inference script (`predict_next_week.py`, retained for cold-start regeneration and local dev; produces the next-day forecast) is not the user-facing artifact.

### 9.3 Reproducibility

Fixed seeds (NumPy, scikit-learn, LightGBM); `pyproject.toml` pinned via `uv`; a single Parquet snapshot committed at a tagged commit (reviewers run `make train` after checking out the tag, no extra fetch); the champion `pyfunc` **bundled in the image** (reproducible even if the registry is unreachable); every contributing MLflow run logged to DagsHub with a permalink; compute footprint stated (*"trains in ~X minutes on Apple M3, no GPU, $0 hosting"*).

### 9.4 Targeted invariant tests

A small set of **property tests** asserts the invariants that must hold — engineering signal for a hiring manager who is an engineer, not only a data scientist. These are deliberately **not** a regression matrix (Section 13); they are focused assertions on properties that, if violated, silently corrupt results. Three are mandatory, each guarding a specific failure mode the design exposed:

1. **Monotonicity after CQR + isotonic.** Generate quantile predictions (synthetic and on a validation slice) and assert the nine post-pipeline quantiles are non-decreasing per row — zero crossing violations. Guards the post-processing-order bug (Section 6.2).
2. **Rolling-window leakage.** Assert every rolling/lag feature is closed-left with a strict `t−1` right edge — construct a row whose target value would change a rolling feature and assert the feature does not move. Guards the §5.2 leakage vector.
3. **UTC/DST lag alignment.** On the spring-forward (23-hour) and fall-back (25-hour) days, assert that a 24h/168h lag in UTC retrieves the correct row (24/168 rows back) and that no hour is dropped or duplicated. Guards the §4.0 integer-lag corruption.

Optional fourth: assert the gas-fallback path (Section 4) substitutes Henry Hub and logs when a regional series is nulled. These tests run in seconds and live in the repo as the proof that correctness was engineered, not assumed.



The artifact is the **deployed marimo showcase on Hugging Face Spaces** (Section 9.2). marimo is mandated because the reactive sliders are the centerpiece — Quarto and Jupyter cannot match the feature-perturbation review experience.

Reading order: (1) data story; (2) compressed regime context; (3) feature catalog with the headline domain-insight feature highlighted; (4) CV methodology including the 24h embargo, the D+1 target rationale, and the KFT/LAG leakage audit; (5) results table — LightGBM vs. baselines, with both DM tests; (6) SHAP summary + top-10 + two dependence plots; (7) permutation importance; (8) regime-stratified error table (n_obs + bootstrap CIs on thin subsets); (9) reliability diagram — raw and final CQR-adjusted, all three levels; (10) representative next-day (24-hour, UTC-derived HE, with a DST note) forecast with a quantile fan chart and the three interactive sliders; (11) honest limitations (exchangeability under regime shift, regime-stratified gaps, bounded-target tail, any coverage divergence); (12) reproducibility statement — tagged commit, DagsHub MLflow permalinks, deployed HF Spaces URL.

### 10.1 Stranger-test gate

Passes when one technical and one non-technical reviewer can each, **on the deployed HF Spaces URL**, articulate the data, method, and result in under 20 minutes. Reviewer recruitment is a Track C-Manual block before M5.

## 11. Risk Register

### R-1 — Post-Elliott regime fragility
**Description.** Pre-2023 and post-2023 LMP distributions are structurally distinct; training spans both (2019–2025) but the forecast serves the post-Elliott regime. **Mitigations:** mixed-regime walk-forward CV (Section 5.1); `post_elliott` and `cold_weather_alert` as first-class features; per-regime error analysis (Section 8.3); if coverage diverges on the post-Elliott folds (reliability diagram), the report names it as a regime-shift failure rather than glossing it. R-1 is the project's best interview narrative.

### R-2 — Data acquisition (weather), and the KFT-contamination trap
**Description.** Historical *forecast* weather is the most likely timeline-slip and a leakage trap: NDFD's historical-forecast archive is incomplete/GRIB2/large, and the tempting shortcut (realized actuals) silently breaks KFT. **Mitigations:** PJM `da_temperature_sets` primary (KFT, zone-aligned, on Data Miner 2); the in-pipeline climatology removes any external-normal dependency; NDFD secondary/droppable, never back-filled with actuals; a leakage smoke test asserts forecasts-not-actuals; the 2019–2025 window keeps surface small.

### R-3 — Data vintage, indexing, and availability
**Description.** Three concrete data risks distinct from R-2: (i) **load-forecast vintaging** — using the latest revision instead of the bid-gate-known vintage is look-ahead; (ii) **DA LMP vintage** — training on preliminary rather than the verified feed; (iii) **DST integer-lag corruption** on the two transition days; plus **regional gas-series gaps** (TZ6 NNY / TETCO M3 post-2024). **Mitigations:** bid-gate vintage from `load_frcstd_hist` (Section 3/4); verified `rt_da_monthly_lmps` target (Section 3); UTC indexing + `RepeatedHourFlag` (Section 4.0); Henry Hub as guaranteed gas backstop with an **automatic, logged fallback** (regional gap >48h → switch that input to Henry Hub, no crash) (Section 0 item 2 / Section 4). The §4.0/§5.2 invariant tests (Section 9.4) assert the indexing and leakage guards actually hold.

## 12. Milestones and Checkpoints

**Build-routing note (orchestrator-facing).** No milestone is briefed inside this document. Each is briefed by the **orchestrator** at the session its work begins, as a Type B-Claude block to the engineering lead, **pointing the engineer to this document** for architecture and acceptance criteria — never restating them. Briefs are one-off and never pre-written here. **The next brief due is the M1 brief** (data layer + feature catalog v1), generated when M1 is scheduled (syllabus Month 2, per `syllabus_v2_2.md`). It must carry the v5.4 decisions binding M1: **24h D+1 target** (§1); **UTC indexing + DST handling** (§4.0); **verified DA LMP target feed** and **bid-gate load-forecast vintage** (§3); **PJM-native weather + 2019–2025 window** (§3); the **train-only Fourier climatology** headline feature with `scarcity_stress` secondary (§4.1); the **pinned mixed-regime fold scheme** (§5.1); the **ratified §0 decisions** (per-fold/450-day, gas fallback, load-weighted temperature zone); and the **invariant tests** (§9.4). Until that session, no M1 brief exists.

### M0 — Plan approved
This document (v5.4).

### M1 — Data layer
Bulk pull from PJM Data Miner 2 (verified DA LMP + decomposition, bid-gate-vintage load forecast, `da_temperature_sets`) + EIA gas, 2019–2025, UTC-indexed. NOAA NDFD only if its forecast archive proves clean (R-2). Feature catalog v1 including both domain-insight candidates, the train-only climatology, and the regime indicators. KFT/LAG classification documented with the weather-source guard and the three leakage-vector audits.

**CP-1**
- [ ] All primary sources return non-empty results for at least the most recent 30 days; archive depths confirmed (`da_tempset` to 2019; EIA regional series checked, Henry Hub guaranteed).
- [ ] Historical bulk pull covers **2019–2025**, UTC-indexed, DST handled (23/25-hour days correct), no nulls in `total_lmp_da`, SMP, MCC, MLC for Western Hub (`pnode_id 51217`), from the **verified** feed.
- [ ] `total_lmp_da` reconciles to within $0.01/MWh of SMP + MCC + MLC on > 99.99% of hourly rows.
- [ ] Load forecast uses the **bid-gate-known vintage** (not latest revision); Western/RTO present in the historical feed.
- [ ] `post_elliott` flips at 2023-01-01; `cold_weather_alert` decodes from gate-known alert state with no false positives in a spot-check.
- [ ] Weather features are forecasts (KFT), not realized actuals (R-2 smoke test).
- [ ] Train-only climatology fit (per-fold), coefficients persisted in the pyfunc; `temperature_deviation_from_normal` computable at inference with no external call.
- [ ] Gas-fallback path verified: nulling a regional series for >48h triggers a logged switch to Henry Hub without a crash (Section 4 / R-3).
- [ ] Invariant tests pass (Section 9.4): closed-left rolling windows; UTC/DST lag alignment on the 23-hour and 25-hour days.
- [ ] Data leakage audit drafted; every feature KFT/LAG with justification; rolling windows closed-left.

### M2 — Baselines and LightGBM
Seasonal-naïve (24h, 168h) and Ridge baselines trained. LightGBM quantile ensemble trained end-to-end with the mixed-regime walk-forward CV (24h embargo, per-fold recalibration) and MLflow logging to DagsHub. Packaged as the single `pyfunc` from the start.

**CP-2 (revised).** The LightGBM quantile ensemble passes M2 if, on the most-recent fold (Fold 5 eval), a **one-sided Diebold–Mariano test on the daily loss differential rejects equal predictive accuracy versus seasonal-naïve-24h at p < 0.05** in favor of the model (HAC/Newey–West LRV). The **target** improvement is ≥15% mean pinball loss — a reporting target, **not** a pass/fail threshold. **Honest fallback:** if DM is significant but pinball improvement is below 15%, M2 still passes; the report states the true percentage and uses the regime-stratified table to show **where** the gains concentrate, rather than suppressing or inflating the headline. Additional gates:
- [ ] LightGBM trains in under 5 minutes wall-clock on M3.
- [ ] Zero quantile-crossing violations on the validation tail after the CQR-then-isotonic pipeline; the monotonicity invariant test (Section 9.4) passes.
- [ ] Both DM tests (median MAE and mean pinball) reported with statistic, p-value, and effect size.
- [ ] MLflow on DagsHub logs ≥ 5 experiments showing clear hyperparameter progression, publicly viewable.

### M3 — Calibration, explainability, regime diagnostics
CQR layer fit on the held-out calibration slice, isotonic enforced last (Section 6.2). SHAP and permutation importance on the median model; **headline domain feature selected from SHAP** (Section 4.1). Regime-stratified error table (n_obs + bootstrap CIs on thin subsets). Reliability diagram for raw and final intervals.

**CP-3**
- [ ] CQR empirical coverage at the 80% nominal level lands within ± 5 percentage points on at least 4 of 5 folds.
- [ ] SHAP top-10 includes both `cold_weather_alert` and the selected headline domain feature.
- [ ] Permutation importance top-10 overlaps the SHAP top-10 on ≥ 6 features.
- [ ] Regime-stratified table renders cleanly; the post-Elliott vs pre-Elliott difference documented and explained; n_obs + bootstrap CIs on thin subsets.
- [ ] Reliability diagram compares raw and final (CQR-adjusted, isotonic-enforced) coverage at 50/80/95, confirming isotonic-last did not degrade coverage. Coverage claimed is **per-interval marginal**, not joint (documented).

### M4 — Marimo showcase runs locally; champion registered; slider lookup precomputed
Marimo (server mode) runs end-to-end **locally**, loading the registered Production model (registry-first, bundle-fallback) and applying calibrated quantiles on the snapshot. The precomputed slider lookup is generated and validated by the M4 spike. CLI inference runs inside the container in under 5 minutes.

**CP-4**
- [ ] Champion `pyfunc` registered to the DagsHub MLflow Registry with the `Production` tag.
- [ ] Local marimo render from cold start completes in under 10 seconds.
- [ ] All three sliders update from the precomputed lookup without errors and without live compute.
- [ ] The precomputed grid covers the interaction space; indexing is instant (M4 spike validated).
- [ ] CLI inference smoke test asserts all upstream pulls returned recent (≤48h) rows in fresh-data mode.

### M5 — Deployed showcase on HF Spaces
Marimo (server mode) containerized via multi-stage Dockerfile (bundling snapshot, slider lookup, champion) and deployed to HF Spaces free tier (Docker SDK), with the DagsHub token as an HF Secret. Final artifact passes the stranger-test gate **on the deployed URL**. CV and LinkedIn updated with the live Spaces URL and the public MLflow URL.

**CP-5**
- [ ] The deployed app loads within 30 seconds of cold start (warm load under 5 seconds).
- [ ] The Production model loads on startup — fetched from the registry if available, else the bundled fallback; the demo renders either way.
- [ ] The `cold_weather_alert` toggle and headline-feature perturbation slider update the forecast and SHAP from the precomputed lookup on the deployed instance.
- [ ] A non-technical reviewer reaches a correct directional interpretation within 20 minutes unaided on the live URL.
- [ ] The honest limitations section names the exchangeability violation, the regime-stratified gaps, the bounded-target tail caveat, and any coverage divergence.
- [ ] Reproducibility statement present and accurate (seeds, pinned versions, snapshot at tagged release, DagsHub permalinks, compute footprint).

The 30-second cold-start threshold is calibrated to HF free-tier reality (multi-stage layer hydration + registry-or-bundle load + Parquet snapshot load). Sleep behavior is accepted per Section 9.2; the README disclaimer is the backstop.

## 13. What This Project Is NOT

Each omission is a deliberate scope decision, defended and interview-quotable. None was reopened by v5.3.

**No multi-day-ahead forecast.** *"The target is the next operating day, because that's what the DA market clears and it's the horizon at which every lag and gas feature is legitimately known at forecast time. A 7-day-ahead forecast made at the gate would have to consume prices that haven't cleared yet — a look-ahead leak. I'd rather ship a clean 24-hour forecast than a leaky seven-day one."*

**No formal causal claim.** *"The model makes a predictive claim with calibrated uncertainty, not a causal one. The perturbation slider is a ceteris-paribus sensitivity probe — it shows how the model responds to a feature holding others fixed, which can be physically out-of-distribution. SHAP attributes predictions, it doesn't establish causation. I deliberately stayed on the predictive side of that line."*

**No enterprise production pipeline or live trading layer.** *"Containerized marimo on Hugging Face Spaces backed by a DagsHub MLflow registry is portfolio-grade hosting, not production infrastructure — no CI/CD matrices, no scheduled retraining, no live per-visitor API ingestion, no trading gates, no drift monitoring. I invested in interactive risk communication and transparent experiment tracking, which showcase DS judgment, rather than enterprise MLOps boilerplate. DVC was rejected as collaboration overhead the project doesn't need; live per-visitor pulls as brittleness that would break the demo."*

**No neural challenger — no NBEATSx, TFT, or transformers.** *"Olivares 2023 shows the classical EPF baseline within ~5% of deep learning on hub-level day-ahead price; the marginal accuracy doesn't earn the complexity for a portfolio. LightGBM is the published-evidence-backed default. I'd rather demonstrate judgment than run a model horse-race."*

**No LEAR / `epftoolbox` baseline.** *"LEAR is the canonical EPF baseline but a specialized library that obscures the DS skills the project demonstrates. Ridge on the same engineered catalog plus seasonal-naïve at 24h and 168h covers the same transparent-classical-baseline role."*

**No sequential conformal (EnbPI / SPCI) and no Giacomini-White.** *"CQR gives finite-sample marginal coverage under exchangeability — the property an interview reader is most likely to recognize and probe. EnbPI/SPCI handle non-exchangeability more aggressively but add conceptual surface that distracts from the core argument. The reliability diagram makes the exchangeability violation visible, and the limitations section names it."*

**No trading layer, BESS arbitrage, or bid logic.** *"The deliverable is a forecast and a diagnostic report. Wrapping a forecast in a trading layer either needs real risk infrastructure or it's a toy — and a toy trading layer would undermine credibility. The forecast and its honestly-communicated uncertainty are the artifact."*

**No CLI/API test matrix, no pytest CI — but targeted invariant tests.** *"I wrote a few focused property tests on the invariants that silently corrupt results if violated — quantile monotonicity after the CQR-plus-isotonic pipeline, closed-left rolling windows, and UTC/DST lag alignment. That's the right testing level for a single-developer portfolio: assert the properties that must hold, not a regression matrix on a script that runs in five minutes, which would be engineering theater. The invariant tests show software-engineering judgment; a sprawling CI matrix would show the opposite."*

---

*End of Engineering Plan — PJM Western Hub Day-Ahead Price Forecasting Tool, v5.4 — Lean DS Capstone with Cloud-Backed Showcase.*
