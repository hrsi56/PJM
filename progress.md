# Yarden's Triple-Track Progress Log

*Living document. Updated at the end of every session. Yarden pastes the updated version into project knowledge after each session.*

---

## Current Position

### Track A: Learning (Syllabus)
- **Anchor:** **`syllabus_v2_3.md` v2.3** (ratified 2026-06-10; supersedes v2.2 — C5-min, F8, C4, CL5, Rehearse folded in; LO3 / LO2 / SC3-learn / SC5-learn rejected and recorded in-file so they cannot resurface).
- **Phase:** I (Mathematical Foundations).
- **Month:** 0 (Linear Algebra Foundations + SVD/PCA Recognition Extension).
- **Week:** **Week 1 — IN PROGRESS** (program launched 2026-06-09).
- **Active L block:** **L1** — Strang Lectures 1–5 + Ch. 1–2, [AUTH]; deliverable = native-Python Gaussian-elimination solver (partial pivoting + back-substitution + singular-matrix failure signal). Issued under v2.2 and **stands as issued** — the v2.3 C5-min change extends the later Month-0 chunks (span now runs to Lecture 17), not L1.
- **Next chunks after L1:** Lectures 6–17 — four fundamental subspaces → projections + least squares (L15–16) → Gram–Schmidt (L17); then eigenvalues/eigenvectors (L21–22; skim 18–20 determinants per the [REC] row). Strang Ch. 3–6.
- **Pace:** ~6.5 months accepted. Month 0 is 5–6 weeks on-plan. C4 interview-readiness additions (~24–28h, Months 1–4) ride as parallel filler.
- **Next pending checkpoint:** Month-0 **mid-month gate**, ~2.5–3 weeks from launch — coding deliverable(s) runnable + NotebookLM consolidation verdict. The L block closing the gate gets the checkpoint flag.

### Track B: Capstone Build
- **Anchor:** **`capstone_V5_5.md` v5.5** (ratified 2026-06-10; supersedes v5.4).
- **Milestone:** **M0 (Plan approved at v5.5).**
- **Next B block: Month-0 data-layer de-risking spike** (B-Claude, **6–8h hard cap — not an early M1**). Confirms: (a) `da_tempset` archive reaches 2019 + schema workable via direct DM2 API; (b) EIA regional gas series IDs (Transco Z6 NNY, TETCO M3) exist + post-2024 gap profile tolerable, Henry Hub `RNGWHHD` backstop confirmed; (c) `load_frcstd_hist` carries Western/RTO; (d) one 30-day verified-LMP pull reconciles total_lmp = SMP+MCC+MLC; **(e) R-4 probe — PJM Cold Weather Advisory/Alert records 2019→present with issuance timestamps** (degraded fallback pre-authorized, v5.5 §4/§11). Output: feed-status memo + **repo skeleton + first commit** (this is when the capstone repo gets created).
- **M1** (data layer + feature catalog v1 — **`temperature_deviation_from_normal` primary, `scarcity_stress` secondary**, train-only temperature-aggregate weights, R-4 outcome applied) lands in syllabus Month 2; CP-1 activates when M1 starts. The M1 brief inherits the **v5.5 §12 binding list** — do not restate the spec.
- **Last commit:** None (the spike delivers the first).
- **Pending clerical (B-Manual):**
  - **PJM Tools account + Data Miner 2 key — NOW, before the spike** (the spike needs the key).
  - DagsHub account — Months 0–2 (must precede M2 MLflow logging).
  - HF account + initial Spaces setup — start of Month 5.

### Track C: Marketing Yarden — **FROZEN (one-off floor authorized)**
- **State:** Frozen by default; phase-trigger fires continuous activation **after capstone M2** (projected mid-Month 3). Active applications no later than start of Month 5.
- **Geography anchor RATIFIED (owner decision): maximum 3 office days/week in TLV/Herzliya; Beer Sheva base.**
- **One-off unfreeze (C2, execute this month; consumes NO CV-iteration slot):**
  1. LinkedIn floor — headline, About, Featured projects (World Psychiatry, solver, event app). **Recruiter-only open-to-work DEFERRED to the M2 phase-trigger.**
  2. README polish on the solver + event-app repos (linked from the live CV today).
  3. Lab-Engineer framing fix — **verified on the live CV ("Income-bridge; managing instrumentation and protocols across 7 concurrent lab courses") and CLOSED.** Do NOT re-add "(transitional role)".
- **C8-research (Month 0–1):** C-Research block — tiered Beer-Sheva-local + ≤3-day-hybrid DS target list (who hires, stack, salary bands, office-day policy).
- **C7 networking floor (from Month 1, ~1h/week):** former students (30 taught 1-on-1), BGU alumni, one Tech7 / Gav-Yam Negev meetup per month.
- **Positioning theme (held):** career-changer from clinical research to industry DS — quantitative forecasting + honest uncertainty communication; World Psychiatry lead-analyst credibility + shipping experience (400+ concurrent-user app, 187M-state search engine); capstone with regime-aware methodology, walk-forward + embargo, KFT/LAG leakage audit, **CQR-calibrated intervals**, regime-stratified error analysis, cloud-backed reproducibility, deployed marimo showcase.
- **Last published artifact:** None. **Pipeline:** Empty (floor starts Month 1). **Applications:** Not started.
- **CV status:** Over the bar to start applying. Framing fix CLOSED. Three milestone-gated iteration slots remain (after M2, M3, M5).

---

## Setup State (one-time actions, pre-next-paste)

- **ACTION REQUIRED — NotebookLM source swap:** the notebook currently holds `syllabus_v2_2.md` + `capstone_V5_4.md`. **Replace with `syllabus_v2_3.md` + `capstone_V5_5.md`** (keep `notebooklm-role.md`). Do this before pasting the next L block; **L1 already in flight is unaffected** (its content didn't change).
- **Project knowledge swap:** `capstone_V5_4.md` → `capstone_V5_5.md`; `syllabus_v2_2.md` → `syllabus_v2_3.md`; this file replaces the prior `progress.md`; `change-register-decisions.md` retained as the decision record.
- Capstone repo: created by the Month-0 spike (skeleton + first commit are spike outputs).

---

## Strategic Anchors

- **Target:** Industry Data Scientist role within ~6.5 months at NIS 35K. Capstone audience: DS hiring manager, not MLE.
- **Geography: ≤3 office days/week TLV/Herzliya** (ratified 2026-06-10).
- **Anchors (all mutually consistent as of 2026-06-10):** **`capstone_V5_5.md` v5.5**, **`syllabus_v2_3.md` v2.3**, `orchestrator-role.md`. Executor role docs: `notebooklm-role.md`, `engineer-role.md`. **Decision record:** `change-register-decisions.md` — 23 accepted (2 modified), 6 rejected.
- **Depth labels:** [AUTH], [REC], [APPLIED-AUTH], [APPLIED-REC].
- **Budget:** $0 expected run rate; $65/month policy ceiling. Deployment $0 (HF Spaces free + DagsHub free + GitHub Actions free — now also running the thin CI).
- **Hardware:** Apple Silicon M3, 16 GB unified memory, CPU only.
- **Language:** English replies (Hebrew input fine).

---

## Standing Scope Decisions (carry forward indefinitely — do not allow back in)

- **No DVC.** Reproducibility = pinned deps + tagged commit + committed Parquet snapshot — **plus the M1 redistribution-terms check; if PJM terms restrict, fallback = engineered matrix + regeneration code** (v5.5 §9.3).
- **No live API pulls during user sessions.** Weekly manual snapshot refresh (~5 min/week) during the application phase.
- **No paid HF Spaces tier in baseline budget.** CPU Upgrade on standby for CP-5 cold-start failures only.
- **Single monolithic LightGBM** quantile ensemble. **No neural challenger — SC3 reviewed and REJECTED 2026-06-10** (Olivares 2023 + the shipped CNN cover the question). SC3-learn fell with it.
- **CQR (split conformal) only. No EnbPI / SPCI / Giacomini-White — SC5 reviewed and REJECTED**, with the **single pre-committed reopen condition** (v5.5 §13): post-Elliott empirical coverage diverging >10 pp from nominal at M3 → EnbPI returns as *remediation*, comparison only (SC5-learn revives at [APPLIED-AUTH] only then).
- **LO3 REJECTED** — Month 0 SVD/PCA stays [REC]; no Eckart–Young. **LO2 REJECTED** — no causal block; the SEM background carries the answer. Both recorded inside `syllabus_v2_3.md` ("Does NOT Teach").
- **Forecast + diagnostic report, not a trading product.**
- **Marimo is mandated.** Three sliders served from the precomputed lookup with the per-cell OOD flag.
- **Walk-forward CV with 24h embargo** is non-negotiable — v5.5 fold scheme: **winter Fold 4 (Jan–Mar 2025), Fold 5 sliding to the pull date.**
- **Stranger-test gate happens on the deployed HF Spaces URL.**
- **Thin GitHub Actions CI on the §9.4 invariant tests is IN** (lands at M2) — still no regression matrix, no scheduled retraining, no drift gates.
- **CP-2 secondary gate pre-committed (F5):** Fold-5 DM p ∈ [0.05, 0.15] + pooled-across-folds DM significant → M2 passes, both numbers reported. **Fixed before results — do not relitigate after seeing them.**

---

## Session Log

### Baseline — 2026-05-31
- Project knowledge configured and made internally consistent across all anchor/role docs. Capstone at M0; Track C frozen.

### 2026-06-09 — Program launch
- Readiness confirmed: anchors aligned, track positions clean. Flagged NotebookLM notebook setup as the one pre-paste item.
- Track A Month 0 started. **Issued L1** (Strang Lectures 1–5 + Ch. 1–2, [AUTH], Gaussian-elimination solver deliverable).
- Track-A-only session by design (B dormant until the Month-0 spike; C frozen).

### 2026-06-10 — Change-register adjudication; v5.5 + v2.3 ratified
- Reviewed `change-anchoring-register.md`: **23 accepted (2 modified), 6 rejected** (SC3, SC3-learn, SC5, SC5-learn, LO3, LO2). The register's "time is not a consideration" premise was struck. Decision record: `change-register-decisions.md`.
- **`capstone_V5_5.md` ratified.** Key deltas: winter Fold 4 (Jan–Mar 2025); window 2019-01-01 → pull date with Fold 5 sliding; CP-2 secondary DM gate; R-4 + pre-authorized fallback; calibration set pinned (60–90 days); train-only temperature weights; negative-Q correction; staleness disclosure; redistribution check; `predict_next_day.py`; thin CI; single EnbPI reopen condition.
- **`syllabus_v2_3.md` ratified.** C5-min (Lectures 1–17), F8 (CQR + Romano 2019 + negative-Q at Month 4), C4 (four interview-surface blocks, fully authored in the Supplementary section), CL5 (CIFAR-10 default), Rehearse (Month 5 transfer answers). Alignment pass also caught **three stale v2.2 artifacts the register missed**: the four-slider list with a horizon selector (→ three sliders, headline-feature perturbation), the `gas_to_load_ratio` SHAP dependence pair, and the PJM-account timing (→ Month 0, pre-spike).
- **progress.md regenerated** (this file). Correction owned: the 2026-06-10 interim version had **dropped the 2026-06-09 launch state** (Week 1 in progress, L1 active, launch log entry) — restored here. F1/F9/office-days/C2/C3/C7/C8 all reflected.

---

## Blockers / Open Questions

- None. (The NotebookLM source swap is a one-time pre-paste action, not a blocker; R-4 and the redistribution question are pre-mitigated risks with scheduled probes.)

---

## Notes for Future Sessions

- **L1 is in flight** — next L block picks up at Lecture 6 (four fundamental subspaces). Month-0 span runs to Lecture 17; eigen at L21–22; determinants 18–20 skim (all in v2.3).
- **Month-0 spike (C3)** is the first B-Claude block — schedule alongside Week 1–2 L blocks. **PJM key (B-Manual) must land first.**
- **SQL authoring block** [APPLIED-AUTH]: parallel filler Months 2–3, 20h cap; done when LAG/LEAD queries can be written cold. **C4 placements** (all detailed in v2.3 Supplementary): A/B [REC] rides Month 1; classification [AUTH-light, 12h cap] early Month 3; take-homes late Month 3 + Month 4; mocks late Month 4 → Month 5.
- **CNN mini-project** [AUTH], B-Claude: Month 1→2 seam, 16h cap, **CIFAR-10 default** (Fashion-MNIST only as de-scope fallback), ship-at-threshold.
- **Headline feature: `temperature_deviation_from_normal` (primary); `scarcity_stress` (secondary, promoted only if SHAP earns it at M3).** `gas_to_load_ratio` is dead — do not resurrect (v5.5 §4.1).
- **Best interview lines to protect:** KFT/LAG leakage audit (§5.2); exchangeability paragraph (§6.2, verbatim); **negative-Q / isotonic-last point (§6.2)**; Bayesian change-point validation of `post_elliott` (Month 1); regime-stratified error story with the **true-OOS winter Fold 4** (§5.1, §8.3); **model-staleness honesty line (§9.3)**; "why PJM, in Israel" + "why marimo" + "why a thin CI, no regression matrix" (Month 5).
- **CP-5 cold-start:** 30s backstop calibrated to HF free tier; **the Actions keep-alive cron auto-disables after 60 days of repo inactivity — the README "~30s first-load" disclaimer is the real backstop; commit at least every 60 days or accept the sleep** (v5.5 §9.2).
- **CV iteration cadence:** three milestone-gated slots (after M2, M3, M5). Lab-Engineer fix CLOSED; the C2 LinkedIn floor consumed no slot.
- **Track C flips:** recruiter-only open-to-work at the M2 phase-trigger; applications no later than start of Month 5.
- **Routing reminder:** the orchestrator never writes Claude Code prompts; the M1 brief inherits the v5.5 §12 binding list rather than restating the spec.
