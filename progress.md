# Yarden's Triple-Track Progress Log

*Living document. Updated at the end of every session. Yarden pastes the updated version into project knowledge after each session.*

---

## Current Position

### Track A: Learning (Syllabus)
- **Anchor:** `syllabus_v2_2.md` **v2.2**.
- **Phase:** I (Mathematical Foundations).
- **Month:** 0 (Linear Algebra Foundations + SVD/PCA Recognition Extension).
- **Week:** Not yet started.
- **Pace:** ~6.5 months accepted. Month 0 is realistically 5–6 weeks given the math baseline — 6 weeks is on-plan, not failure.
- **Next learning deliverable:** First L block — selective MIT 18.06 (Strang) Lectures 1–5 at [AUTH]; coding deliverable is a Gaussian-elimination row-reduction solver in native Python. Then Strang Ch. 1–2.

### Track B: Capstone Build
- **Anchor:** `capstone_V5_4.md` **v5.4**.
- **Milestone:** **M0 (Plan approved).**
- **Next:** M1 (data layer + feature catalog v1 with `gas_to_load_ratio`) lands in syllabus Month 2. CP-1 becomes active when M1 starts.
- **Last commit:** None.
- **Pending clerical (B-Manual, schedulable as filler):**
  - PJM Tools account + Data Miner 2 key — Months 0–1.
  - DagsHub account — Months 0–2 (must precede M2 MLflow logging).
  - HF account + initial Spaces setup — start of Month 5.

### Track C: Marketing Yarden — **FROZEN**
- **State:** Frozen by default. Activates only on explicit triggers (see `orchestrator-role.md` Track C rules). Phase-trigger fires continuous activation **after capstone M2** (projected mid-Month 3).
- **Positioning theme (held):** Career-changer from clinical research to industry DS — quantitative forecasting + honest uncertainty communication; published research credibility (World Psychiatry, lead analyst on SEM mediation) + shipping experience (400+ concurrent-user app, 187M-state search engine); capstone demonstrating regime-aware methodology, walk-forward + embargo, KFT/LAG leakage audit, split-conformal calibration, regime-stratified error analysis, cloud-backed reproducibility (public DagsHub MLflow UI + deployed marimo HF Spaces showcase).
- **Last published artifact:** None. **Networking pipeline:** Empty. **Application status:** Not started.
- **CV status:** Over the bar to start applying. One optional one-off framing fix pending — tighten the Laboratory Engineer entry to read as income-bridge / continuity (closes the post-Jan-2025 gap; "(transitional role)" already removed, do not re-add). Does NOT consume a milestone-gated CV iteration slot. Not yet requested for execution.

---

## Strategic Anchors

- **Target:** Industry Data Scientist role within ~6.5 months at NIS 35K. Capstone audience: DS hiring manager, not MLE.
- **Anchors (all mutually consistent):** `capstone_V5_4.md` v5.4, `syllabus_v2_2.md` v2.2, `orchestrator-role.md`. Executor role docs: `notebooklm-role.md`, `engineer-role.md`.
- **Depth labels:** [AUTH], [REC], [APPLIED-AUTH], [APPLIED-REC].
- **Budget:** $0 expected run rate; $65/month policy ceiling. v5.4 deployment is $0 (HF Spaces free + DagsHub free + GitHub Actions free).
- **Hardware:** Apple Silicon M3, 16 GB unified memory, CPU only.
- **Architecture:** Orchestrator → 3 tracks → execution teams. Orchestrator alone holds the strategic map. Each executor depends entirely on its brief + its role doc.
- **Language:** English replies (Hebrew input fine).

---

## Standing Scope Decisions (carry forward indefinitely — do not allow back in)

- **No DVC.** Reproducibility = pinned deps + tagged commit + committed Parquet snapshot.
- **No live API pulls during user sessions** on the deployed app. Weekly manual snapshot refresh (~5 min/week) during the application phase.
- **No paid HF Spaces tier in baseline budget.** CPU Upgrade ($22/month) on standby for CP-5 cold-start failures only — measure first, activate only if needed.
- **Single monolithic LightGBM** quantile ensemble. No neural challenger, no model menagerie.
- **Split conformal only.** No EnbPI / SPCI / Giacomini-White.
- **Forecast + diagnostic report, not a trading product.** No LONG/SHORT/FLAT, no BESS, no bid logic.
- **Marimo is mandated** as the showcase format. Reactive sliders are the centerpiece.
- **Walk-forward CV with 24h embargo** is non-negotiable.
- **Stranger-test gate happens on the deployed HF Spaces URL**, not locally.

---

## Session Log

### Baseline — 2026-05-31
- Project knowledge configured and made internally consistent. All four anchor/role docs aligned on canonical filenames (`capstone_V5_4.md` v5.4, `syllabus_v2_2.md` v2.2) and the four-label depth vocabulary.
- `orchestrator-role.md` carries a Default register: consultation is the default; full machinery deploys only on an execution signal or a progress request.
- Learning not yet started (Month 0 pending). Capstone at M0. Track C frozen.
- Progress log reset to this clean baseline.

---

## Blockers / Open Questions

- None.

---

## Notes for Future Sessions

- **First L block default:** Month 0 Week 1 — MIT 18.06 (Strang) Lectures 1–5 at [AUTH]; Gaussian elimination from scratch in native Python.
- **SQL authoring block** (v2.2 addition, [APPLIED-AUTH]): parallel filler Months 2–3, 20h cap; done when LAG/LEAD window-function queries can be written cold. Setup = toy `ts, price, load, gas` table on Supabase or local Postgres. Must complete before active applications (start of Month 5).
- **CNN mini-project** (v2.2 addition, [AUTH], B-Claude build): Month 1→2 seam, 16h cap, Fashion-MNIST/CIFAR-10, ship-at-threshold-and-stop. Outside capstone scope by design — ships a DL repo for the first CV pass.
- **`gas_to_load_ratio`** is the headline domain-insight feature; target SHAP top-5 at M3. Build-time fallbacks in capstone Section 4.1.
- **Best interview lines to protect:** KFT/LAG leakage audit (Section 5.2); the exchangeability paragraph (Section 6.2, must appear verbatim in the final artifact); Bayesian change-point validation of `post_elliott` (syllabus Month 1); regime-stratified error story (Section 8.3).
- **CP-5 cold-start:** 30s backstop is calibrated to HF free-tier reality; keep-alive cron eliminates it in practice for reviewers.
- **CV iteration cadence (Track C, when unfrozen):** three milestone-gated iterations max — after M2, after M3, after M5. The Lab Engineer framing fix is a separate one-off and does not consume a slot.
- **Routing reminder:** the orchestrator never writes Claude Code prompts — that belongs to Claude (chat) downstream of a B-Claude brief.
