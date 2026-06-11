# Yarden's Triple-Track Progress Log

*Living document. Regenerated in full at the end of every session under the orchestrator-role.md regeneration contract. Last updated: **2026-06-11 (syllabus v3.0 alignment)**.*

---

## Current Position

### Track A: Learning (Syllabus)
- **Anchor: `syllabus_v3_0.md` v3.0 — AUTHORED 2026-06-11** (this session; supersedes the planned "v2.4 alignment pass" — version number per Yarden's call). Market-conversion alignment release for capstone v6.0: **zero pace impact; math arc, depth labels, month structure, time budgets, and SQL/CNN/C4 supplementary blocks untouched.** Touched: Month-0 collinearity example (load×residual-load) + PCA toy dataset; Month-1 change-point block (DE-LU series, two boundaries: 2021-09→2022-02 onset + late-2022 normalization) + domain reading (Olivares out; Trebbien 2023 + Hilger 2024 in; Lago stays); Month-2 M1 interleave (ENTSO-E/SMARD, residual-load catalog, Dunkelflaute, R-2) + ARIMA framing + §5.4→§5.1 fix; Month-3 comparator → **similar-day naïve (Lago convention)** + v6.0 fold refs; Month-4 EnbPI reopen wording + SHAP plot relabels; Month-5 sliders, CC-BY attribution row, interview-prep additions (why-German-market / why-hourly / why-no-gas / negative-prices-no-log); Does-NOT-Teach GNN+NBEATSx rewrites; SQL toy table (`ts, price, load, wind, solar`).
- **Phase:** I (Mathematical Foundations). **Month:** 0. **Week:** 1 (launched 2026-06-09).
- **L1 — DONE.** Strang L1–5 + Ch. 1–2; Gaussian-elimination solver shipped.
- **L2 — ACTIVE / in flight.** Strang L6–10 + Ch. 3, [AUTH], four fundamental subspaces; deliverable `four_subspaces(A)`. (L2 content is version-agnostic — unaffected by the v3.0 swap.)
- **Next: L3 ∥ Month-0 spike, after L2.** L3 — orthogonality → projections → least squares → Gram–Schmidt (OCW L14–17; verify index at briefing per C5-min); lands the OLS normal-equation deliverable. Then eigen L21–22 [AUTH]; determinants skim [REC]; SVD/PCA + condition number [REC] close Month 0.
- **Next pending checkpoint:** Month-0 **mid-month gate**, ~end of June — coding deliverable(s) runnable + NotebookLM consolidation verdict.

### Track B: Capstone Build
- **Anchor: `capstone_V6_0.md` v6.0 — RATIFIED 2026-06-11.** German **DE-LU day-ahead** via **ENTSO-E Transparency (primary) + SMARD (secondary)**; v5.5 (PJM) superseded, retained as decision record. **Milestone: M0** (plan approved).
- **Conversion core (one line each):** §0 five ratified decisions (per-fold/~450d carried; DE-LU switch; **gas omitted** w/ SMARD-THE reopen; **hourly across the 15-min MTU transition** via quarter-hour averaging; **weather dropped** — in-pipeline climatology on residual-load forecast). §6–§10 verbatim from v5.5 (LightGBM 9-head + CQR + isotonic-last; DM + pre-committed CP-2 secondary gate; SHAP/permutation/stratified/reliability; DagsHub/marimo/HF/OOD lookup; invariant tests + thin CI; stranger test). Risks: R-1 three-regime / **R-2 VRE-forecast vintage (top empirical unknown)** / R-3 platform migration+quality / R-4 15-min boundary.
- **Next B block: Month-0 ENTSO-E/SMARD de-risking spike** (B-Claude, **6–8h hard cap — not an early M1**): (a) **R-2 vintage probe #1** (as-published vs. post-gate-revision archiving; SMARD cross-probe); (b) archive depths to 2019-01-01 (A44/A65-A01/A69, DE-LU `10Y1001A1001A82H`); (c) 15-min boundary pull across 2025-10-01 + hourly-averaging validation (entsoe-py ≥0.7.5); (d) ENTSO-E↔SMARD price reconciliation sample; (e) SMARD **THE gas** CC-BY probe (gas reopen); (f) API limits/migration stability + SFTP test; (g) token works from Israel end-to-end. Output: feed-status memo + **repo skeleton + first commit** (repo gets `capstone_V6_0.md`; engineer `CLAUDE.md` filename reference updated at repo creation).
- **Gated on:** ENTSO-E API token (B-Manual, in flight — email sent 2026-06-11; grant expected ~06-16/17). SMARD keyless.
- **M1** lands in syllabus Month 2; CP-1 activates when M1 starts. The M1 brief inherits the **v6.0 §12 binding list** — do not restate the spec.
- **Last commit:** None (the spike delivers the first).
- **Pending clerical (B-Manual):** ENTSO-E token — IN FLIGHT. DagsHub account — Months 0–2 (precedes M2). HF account + Spaces — start of Month 5.
- **Next pending checkpoint:** CP-1 (at M1). Before that, the spike's feed-status memo is the de-facto gate on M1 readiness.

### Track C: Marketing — FROZEN (one-off floor authorized; consumes NO CV-iteration slot)
- Phase-trigger fires continuous activation **after capstone M2** (projected mid-Month 3). Active applications no later than start of Month 5.
- **Geography RATIFIED:** ≤3 office days/week TLV/Herzliya; Beer Sheva base.
- **One-off C2 floor (execute this month):** 1. LinkedIn floor (headline, About, Featured: World Psychiatry, solver, event app) — **PENDING**; recruiter-only open-to-work deferred to M2 trigger. 2. README polish on solver + event-app repos — **PENDING**. 3. Lab-Engineer framing fix — **CLOSED**; do NOT re-add "(transitional role)".
- **C8-research (Month 0–1):** tiered Beer-Sheva-local + ≤3-day-hybrid DS target list. Not yet issued.
- **C7 networking floor (from Month 1, ~1h/week):** former students, BGU alumni, one Tech7 / Gav-Yam Negev meetup per month.
- **Positioning theme (held; survives the pivot):** career-changer from clinical research to industry DS — quantitative forecasting + honest uncertainty communication; World Psychiatry lead-analyst credibility + shipping experience; capstone with regime-aware methodology, walk-forward + embargo, KFT/LAG leakage audit, CQR-calibrated intervals, regime-stratified errors, cloud-backed reproducibility, deployed marimo showcase. "Why the German market, in Israel" carries the transfer answer.
- **Last published artifact:** None. **Pipeline:** empty. **Applications:** not started.
- **CV status:** over the bar to start applying. Three milestone-gated iteration slots remain (after M2, M3, M5).

---

## Setup State (one-time actions)
- **ENTSO-E API token — IN FLIGHT** (email sent 2026-06-11; ~3-working-day grant; then generate in account settings; store for repo env, never commit).
- ~~PJM Tools account + DM2 key~~ — **CANCELLED 2026-06-11.**

---

## Strategic Anchors

- **Target:** Industry Data Scientist role within ~6.5 months at NIS 35K. Capstone audience: DS hiring manager, not MLE.
- **Geography:** ≤3 office days/week TLV/Herzliya (ratified 2026-06-10); Beer Sheva base.
- **Anchor docs:** **`capstone_V6_0.md` v6.0** + **`syllabus_v3_0.md` v3.0** (both ratified/authored 2026-06-11), `orchestrator-role.md` (amended 2026-06-10). Executor role docs: `notebooklm-role.md`, `engineer-role.md` (repo `CLAUDE.md` — capstone-filename reference updates at repo creation). Decision records: `change-register-decisions.md`, `de-lu-conversion-memo` (2026-06-11). *Note: the orchestrator-role doc hardcodes stale versions in places — these anchors win.*
- **Depth labels:** [AUTH], [REC], [APPLIED-AUTH], [APPLIED-REC].
- **Budget:** $0 expected run rate; $65/month policy ceiling. Deployment $0.
- **Hardware:** Apple Silicon M3, 16 GB unified memory, CPU only.
- **Language:** English replies (Hebrew input fine).

---

## Standing Scope Decisions (carry forward indefinitely — do not allow back in)

- **Market = DE-LU via ENTSO-E + SMARD (ratified 2026-06-11).** PJM dead — do not revisit.
- **No gas/fuel-price layer.** Residual load carries the merit-order signal. **Single reopen:** spike confirms SMARD exposes a CC-BY THE day-ahead series → gas returns as a LAG feature only, never headline (v6.0 §0.3, §13).
- **Hourly target end-to-end across the 15-min MTU transition** (2025-10-01); quarter-hours averaged to hourly. Quarter-hourly target = named future work (v6.0 §0.4, §13).
- **No external weather.** Train-only Fourier climatology on the residual-load forecast series (v6.0 §0.5, §4.1).
- **No DVC.** Pinned deps + tagged commit + committed snapshot — **CC BY 4.0 with attribution** (redistribution RESOLVED; v6.0 §9.3).
- **No live API pulls during user sessions.** Weekly manual snapshot refresh during the application phase.
- **No paid HF Spaces tier in baseline budget.** CPU Upgrade on standby for CP-5 cold-start only.
- **Single monolithic LightGBM** quantile ensemble. **No neural challenger** (SC3 rejected; shipped CNN + published evidence carry the question).
- **CQR (split conformal) only. No EnbPI / SPCI / Giacomini-White** — single pre-committed reopen (v6.0 §13): any regime stratum's coverage diverging >10 pp at M3 → EnbPI as remediation, comparison only.
- **LO3 REJECTED** — Month-0 SVD/PCA stays [REC]. **LO2 REJECTED** — no causal block.
- **Forecast + diagnostic report, not a trading product.**
- **Marimo is mandated.** Three sliders (Dunkelflaute toggle, headline perturbation, quantile selector) from the precomputed lookup with the per-cell OOD flag.
- **Walk-forward CV with 24h embargo, three-regime pinned fold scheme** (v6.0 §5.1: crisis-peak F3 Jul–Sep 2022; negative-price F4 May–Jul 2025; F5 current tail, fully post-15-min-transition).
- **Stranger-test gate happens on the deployed HF Spaces URL.**
- **Thin GitHub Actions CI on the §9.4 invariant tests is IN** (lands at M2; four mandatory tests incl. the 15-min boundary) — no regression matrix.
- **CP-2 secondary gate pre-committed:** Fold-5 DM p ∈ [0.05, 0.15] + pooled-across-folds DM significant → M2 passes, both numbers reported. **Comparator = similar-day naïve (Lago convention).** Fixed before results.
- **Headline-feature rule:** `residual_load_deviation_from_normal` primary; `scarcity_stress = relu(load_z)·relu(−vre_z)` secondary; SHAP at M3 decides. `gas_to_load_ratio` stays dead.

---

## Session Log (newest first)

- **2026-06-11 (syllabus alignment) — `syllabus_v3_0.md` authored.** Full row-by-row audit of v2.3 against capstone v6.0; all conversion touch-points updated (list in Track A above); math arc/pace/structure verified untouched; rejected-item records (LO3/LO2/SC3-learn/SC5-learn) carried forward. Supersedes the planned "v2.4" item; v3.0 numbering per Yarden. Setup-state swaps consolidated (PK + NotebookLM now each need both files).
- **2026-06-11 (v6.0 respec) — `capstone_V6_0.md` authored + ratified.** Full PJM→DE-LU conversion; §6–§10 verbatim, §0–§5/§11–§12 rewritten; standing decisions amended (gas; hourly; weather; attribution; folds; comparator). Spike redefined w/ R-2 vintage probe #1.
- **2026-06-11 — DE-LU conversion research memo delivered.**
- **2026-06-11 — PJM confirmed geo-blocked; ENTSO-E open from Israel; token email sent; PJM key cancelled.**
- **2026-06-11 — NotebookLM swap #1 confirmed; L2 started; L3 ∥ spike plan set.**
- **2026-06-10 — Orchestrator amendments; L1 done → L2 issued; change-register adjudication (v5.5 + v2.3 ratified).**
- **2026-06-09 — Program launch; L1 issued.** **2026-05-31 — Baseline.**

---

## Blockers / Open Questions

- **ENTSO-E token grant** (~3 working days from 2026-06-11; expected ~06-16/17) — gates the spike's API path. SFTP + SMARD paths and all planning work unblocked.
- None other outstanding.

---

## Notes for Future Sessions

- **Spike brief (issue when token lands):** scope per Track B above; repo skeleton + first commit; place `capstone_V6_0.md` in repo; update engineer `CLAUDE.md` capstone-filename reference.
- **L3 pickup (after L2 + NotebookLM swap #2):** OCW L14–17 (verify index per C5-min) — OLS normal-equation deliverable. Then eigen L21–22 [AUTH]; determinants skim [REC]; SVD/PCA + condition number [REC] close Month 0 (collinearity example now load×residual-load per v3.0).
- **SQL authoring block** [APPLIED-AUTH]: parallel filler Months 2–3, 20h cap; toy table `ts, price, load, wind, solar`; done when LAG/LEAD window queries write cold.
- **CNN mini-project** [AUTH], B-Claude: Month 1→2 seam, 16h cap, CIFAR-10 default, ship-at-threshold. Not wired into the capstone.
- **C4 placements:** A/B testing [REC, ~4h] rides Month 1; classification & metrics [AUTH-light, 10–12h] early Month 3; timed take-homes late Month 3 + Month 4; mocks ×2–3 late Month 4 → Month 5.
- **Best interview lines to protect (v6.0/v3.0):** KFT/LAG leakage audit @ 12:00 CET gate; exchangeability paragraph (verbatim); negative-Q / isotonic-last; change-point validation of the crisis flags (two boundaries, Month 1); regime-stratified story w/ true-OOS crisis-peak F3 + negative-price F4; R-2 vintage honesty line ("as-archived, same convention as the published benchmarks"); two-sided bounded tail w/ live −500 floor (+ −600 from 2026-05-28 note); model-staleness line; "why the German market, in Israel" + "why hourly" + "why no gas" + negative-prices/no-log + "why marimo" + "why thin CI".
- **CP-5 cold-start:** 30s backstop; Actions cron auto-disables after 60 days of repo inactivity — README disclaimer is the real backstop.
- **CV iteration cadence:** three milestone-gated slots (after M2, M3, M5).
- **Track C flips:** recruiter-only open-to-work at the M2 phase-trigger; applications no later than start of Month 5.
- **Routing reminder:** the orchestrator never writes Claude Code prompts; the M1 brief inherits the v6.0 §12 binding list rather than restating the spec.
