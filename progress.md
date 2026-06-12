# Yarden's Triple-Track Progress Log

*Living document. Regenerated in full at the end of every session under the orchestrator-role.md regeneration contract. Last updated: **2026-06-12 (spike adjudicated → v6.1)**.*

---

## Current Position

### Track A: Learning (Syllabus)
- **Anchor: `syllabus_v3_0.md` v3.0.**
- **Phase:** I (Mathematical Foundations). **Month:** 0. **Week:** 1–2 (launched 2026-06-09).
- **L1 — DONE.** Strang L1–5 + Ch. 1–2; Gaussian-elimination solver shipped.
- **L2 — IN FLIGHT, PAUSED by owner (2026-06-12):** Yarden finishes L2 after the capstone checks close (only the R-2 rerun remains). **L3 — deferred** (parallel-question resolved: not now).
- **Pace note:** Month 0 is the binding constraint (5–6 weeks); the conversion + spike consumed ~3 days of week 1. Fine — but resume L2 promptly once the R-2 rerun lands (~2026-06-14).
- **Next pending checkpoint:** Month-0 **mid-month gate**, ~end of June — coding deliverable(s) runnable + NotebookLM consolidation verdict.

### Track B: Capstone Build
- **Anchor: `capstone_V6_1.md` v6.1 — AUTHORED 2026-06-12** (spike-adjudication release on v6.0; pending swaps — see Setup State). **Milestone: M0** (plan approved; spike done).
- **SPIKE EXECUTED 2026-06-12** — branch `claude/modest-bardeen-96zee0` @ `20fc1ff` on `hrsi56/PJM`; memo at `docs/spike-feed-status.md`; scope discipline held (sample pulls only). Results: **Q1/Q3/Q4/Q5/Q6 VERIFIED** (access clean; feature feeds natively PT15M across whole 2019+ archive, A44 date-conditional at 2025-10-01; boundary averaging exact to 1e-9; 2019 depth confirmed w/ ~25h A65/A01 head gap; ENTSO-E↔SMARD 96/96 hours within €0.01). **Q7 — gas reopen ADJUDICATED NOT MET** (orchestrator ruling 2026-06-12): SMARD API = generation volume only, no THE price; omission FINAL (narrow residual: optional 10-min browser check of SMARD's JS gas page before M1 — re-opens only if it reveals a CC-BY API-accessible daily THE price series; Yarden's call whether to spend the 10 min). **Q2 — R-2 PENDING-with-path:** publication-timing asymmetry observed (A65 present, A69 absent at 15:35 CEST D-1 → VRE initial publication post-gate); metadata = dead end; harness committed, snapshot #1 taken for delivery 2026-06-13. **Q8 — SFTP untested from sandbox** (port-22 egress blocked there); chunked API assessed sufficient; verify from M3 only if needed.
- **OPEN ACTION — R-2 rerun (~5 min, on/after 2026-06-14):** run the committed harness's snapshot + compare for delivery 2026-06-13 → final overwrite verdict; **carry the verdict to the next session opening.** This is the last item gating "checks done" and the L-track resume.
- **v6.1 delta (vs v6.0):** §0.3 gas reopen closed; §3/§4.0 feature-feed PT15M + chunk-boundary bin integrity made spec; §5.2/§11 R-2 sharpened (as-archived = expected operative path; overwrite question only); §9.3 entsoe-py pinned 0.8.0; §9.4 test 4 extended (full-bin clause); §12 routing note updated (spike DONE; M1 brief binding list extended: feature aggregation, head-gap, R-2 verdict, repo/branch-state verification); CP-1 annotated with pre-confirmations; §13 fuel-price entry rewritten.
- **Repo hygiene (B-Manual, ~2 min each, surfaced 2026-06-12):** (1) **rename repo `hrsi56/PJM`** → e.g. `delu-day-ahead-forecast` before anything public links it (GitHub auto-redirects); (2) **merge the spike branch to `main`** (or PR via Claude Code next repo session); (3) drop `capstone_V6_1.md` at repo root (highest-version rule makes it source of truth).
- **M1** lands in syllabus Month 2; CP-1 activates at M1; brief inherits the **v6.1 §12 binding list**.
- **Pending clerical (B-Manual):** DagsHub account — Months 0–2 (precedes M2). HF account + Spaces — start of Month 5.
- **Next pending checkpoint:** CP-1 (at M1). Spike pre-confirmed CP-1 item 1 in full; item 3 sample-level; item 5 pending the rerun; rest open by design.

### Track C: Marketing — FROZEN (one-off floor authorized; consumes NO CV-iteration slot)
- Phase-trigger fires continuous activation **after capstone M2** (projected mid-Month 3). Active applications no later than start of Month 5.
- **Geography RATIFIED:** ≤3 office days/week TLV/Herzliya; Beer Sheva base.
- **One-off C2 floor (execute this month):** 1. LinkedIn floor (headline, About, Featured: World Psychiatry, solver, event app) — **PENDING**; recruiter-only open-to-work deferred to M2 trigger. 2. README polish on solver + event-app repos — **PENDING**. 3. Lab-Engineer framing fix — **CLOSED**; do NOT re-add "(transitional role)".
- **C8-research (Month 0–1):** tiered Beer-Sheva-local + ≤3-day-hybrid DS target list. Not yet issued.
- **C7 networking floor (from Month 1, ~1h/week):** former students, BGU alumni, one Tech7 / Gav-Yam Negev meetup per month.
- **Positioning theme (held):** career-changer from clinical research to industry DS — quantitative forecasting + honest uncertainty communication; World Psychiatry lead-analyst credibility + shipping experience; capstone with regime-aware methodology, walk-forward + embargo, KFT/LAG leakage audit, CQR-calibrated intervals, regime-stratified errors, cloud-backed reproducibility, deployed marimo showcase. "Why the German market, in Israel" carries the transfer answer.
- **Last published artifact:** None. **Pipeline:** empty. **Applications:** not started.
- **CV status:** over the bar to start applying. Three milestone-gated iteration slots remain (after M2, M3, M5).

---

## Setup State (one-time actions)

- **v6.1 swaps — ACTION-REQUIRED:** replace `capstone_V6_0.md` with **`capstone_V6_1.md`** in (1) project knowledge, (2) NotebookLM, (3) **repo root** (replace or add — engineer doc's highest-version rule applies).
- **Repo rename — ACTION-REQUIRED (2 min):** `hrsi56/PJM` → DE-LU-appropriate name, before any public linking.
- **Spike branch merge — ACTION-REQUIRED:** merge `claude/modest-bardeen-96zee0` → `main` (manually or via Claude Code PR next repo session).
- **R-2 rerun — SCHEDULED (~2026-06-14):** snapshot + compare for delivery 2026-06-13 via the committed harness (~5 min); carry verdict forward.
- ENTSO-E token — DONE. Capstone repo + CLAUDE.md + capstone at root — DONE (spike ran against them). PK/NotebookLM v6.0/v3.0 swaps — DONE 2026-06-11.

---

## Strategic Anchors

- **Target:** Industry Data Scientist role within ~6.5 months at NIS 35K. Capstone audience: DS hiring manager, not MLE.
- **Geography:** ≤3 office days/week TLV/Herzliya (ratified 2026-06-10); Beer Sheva base.
- **Anchor docs:** **`capstone_V6_1.md` v6.1** (authored 2026-06-12; v6.0 ratified 2026-06-11) + **`syllabus_v3_0.md` v3.0**, `orchestrator-role.md` / `notebooklm-role.md` / `engineer-role.md` (refreshed 2026-06-11; engineer doc = repo `CLAUDE.md`). Decision records: `change-register-decisions.md`, `de-lu-conversion-memo`, `docs/spike-feed-status.md` (in-repo).
- **Depth labels:** [AUTH], [REC], [APPLIED-AUTH], [APPLIED-REC].
- **Budget:** $0 expected run rate; $65/month policy ceiling. Deployment $0.
- **Hardware:** Apple Silicon M3, 16 GB unified memory, CPU only.
- **Language:** English replies (Hebrew input fine).

---

## Standing Scope Decisions (carry forward indefinitely — do not allow back in)

- **Market = DE-LU via ENTSO-E + SMARD (ratified 2026-06-11).** PJM dead — do not revisit.
- **No gas/fuel-price layer — reopen ADJUDICATED NOT MET 2026-06-12, omission FINAL.** SMARD API exposes gas generation volume only, no THE price. Narrow residual: optional pre-M1 browser check of SMARD's JS gas page; re-opens only if it reveals a CC-BY, API-accessible daily THE **price** series (v6.1 §0.3, §13).
- **Hourly target end-to-end across the 15-min MTU transition** (2025-10-01); quarter-hours averaged to hourly (spike-validated). **Feature feeds natively PT15M from 2019 — uniform hourly aggregation at ingest; chunk-boundary bin integrity mandatory** (v6.1 §4.0). Quarter-hourly target = named future work.
- **No external weather.** Train-only Fourier climatology on the residual-load forecast series (v6.1 §0.5, §4.1).
- **No DVC.** Pinned deps + tagged commit + committed snapshot — **CC BY 4.0 with attribution** (v6.1 §9.3). `entsoe-py` pinned 0.8.0.
- **No live API pulls during user sessions.** Weekly manual snapshot refresh during the application phase.
- **No paid HF Spaces tier in baseline budget.** CPU Upgrade on standby for CP-5 cold-start only.
- **Single monolithic LightGBM** quantile ensemble. **No neural challenger** (SC3 rejected; shipped CNN + published evidence carry the question).
- **CQR (split conformal) only. No EnbPI / SPCI / Giacomini-White** — single pre-committed reopen (v6.1 §13): any regime stratum's coverage diverging >10 pp at M3 → EnbPI as remediation, comparison only.
- **LO3 REJECTED** — Month-0 SVD/PCA stays [REC]. **LO2 REJECTED** — no causal block.
- **Forecast + diagnostic report, not a trading product.**
- **Marimo is mandated.** Three sliders (Dunkelflaute toggle, headline perturbation, quantile selector) from the precomputed lookup with the per-cell OOD flag.
- **Walk-forward CV with 24h embargo, three-regime pinned fold scheme** (v6.1 §5.1: crisis-peak F3 Jul–Sep 2022; negative-price F4 May–Jul 2025; F5 current tail, fully post-15-min-transition).
- **Stranger-test gate happens on the deployed HF Spaces URL.**
- **Thin GitHub Actions CI on the §9.4 invariant tests is IN** (lands at M2; four mandatory tests incl. the extended 15-min aggregation-integrity test) — no regression matrix.
- **CP-2 secondary gate pre-committed:** Fold-5 DM p ∈ [0.05, 0.15] + pooled-across-folds DM significant → M2 passes, both numbers reported. **Comparator = similar-day naïve (Lago convention).** Fixed before results.
- **Headline-feature rule:** `residual_load_deviation_from_normal` primary; `scarcity_stress = relu(load_z)·relu(−vre_z)` secondary; SHAP at M3 decides. `gas_to_load_ratio` stays dead.

---

## Session Log (newest first)

- **2026-06-12 (adjudication) — Spike report received; v6.1 authored.** Q1/Q3/Q4/Q5/Q6 verified; gas reopen ruled NOT MET (closed, narrow browser-check residual); R-2 sharpened (VRE initial publication observed post-gate; overwrite question pending the ~06-14 rerun); chunk-boundary artifact + PT15M-everywhere + metadata-dead-end folded into spec; entsoe-py pinned 0.8.0; CP-1 item 1 pre-confirmed, item 3 sample-level. Repo hygiene surfaced (rename `PJM`; merge spike branch; v6.1 to repo root). L-track pause registered (L2 finishes after the rerun; L3 deferred — parallel question closed by owner).
- **2026-06-12 — Spike brief issued.** Token arrived early; repo + Claude Code + gh CLI ready; eight-question brief delivered.
- **2026-06-11 (audit) — Post-conversion full-set audit PASSED** (two spike-scope additions; one cosmetic M1 note).
- **2026-06-11 (roles) — Three role docs refreshed.**
- **2026-06-11 (syllabus) — `syllabus_v3_0.md` authored.**
- **2026-06-11 (respec) — `capstone_V6_0.md` authored + ratified** (PJM→DE-LU).
- **2026-06-11 — Conversion memo; PJM confirmed geo-blocked; ENTSO-E open; token email sent; NotebookLM swap #1; L2 started.**
- **2026-06-10 — Orchestrator amendments; L1 done → L2 issued; change-register adjudication.**
- **2026-06-09 — Program launch; L1 issued.** **2026-05-31 — Baseline.**

---

## Blockers / Open Questions

- **R-2 final verdict** — pending the ~2026-06-14 snapshot/compare rerun (5 min, harness committed). Gates the "checks done → L2 resumes" moment and the M1-brief R-2 line. Not a blocker for anything else.
- **Gas browser check (optional, Yarden's call):** 10 min on `smard.de/en/energy-data-compact/gas` — only worth it for epistemic closure; the boundary stands either way.
- None other outstanding.

---

## Notes for Future Sessions

- **Next session opening:** Yarden carries the **R-2 rerun verdict** (archive overwrite yes/no). If yes (values change post-publication) → the as-archived documentation stands with the overwrite noted; if no (stable) → R-2 narrows to "post-gate initial publication, stable thereafter" — either way M1 proceeds with the documented treatment.
- **L2 resume** right after the rerun; then L3 (OCW L14–17, verify index per C5-min — OLS normal-equation deliverable); then eigen L21–22 [AUTH]; determinants skim [REC]; SVD/PCA + condition number [REC] close Month 0 (collinearity example: load×residual-load).
- **M1 brief (syllabus Month 2):** inherits the **v6.1 §12 binding list** (incl. feature aggregation + chunk-boundary integrity, A65/A01 head gap, R-2 verdict + as-archived documentation, repo/branch-state verification).
- **M1 cosmetic:** verify Dec-2024 / Jan-2025 Dunkelflaute peaks against pulled data; tighten the §2.2 narrative sentence.
- **SQL authoring block** [APPLIED-AUTH]: parallel filler Months 2–3, 20h cap; toy table `ts, price, load, wind, solar`; done when LAG/LEAD window queries write cold.
- **CNN mini-project** [AUTH], B-Claude: Month 1→2 seam, 16h cap, CIFAR-10 default, ship-at-threshold. Not wired into the capstone.
- **C4 placements:** A/B testing [REC, ~4h] rides Month 1; classification & metrics [AUTH-light, 10–12h] early Month 3; timed take-homes late Month 3 + Month 4; mocks ×2–3 late Month 4 → Month 5.
- **Best interview lines to protect:** KFT/LAG leakage audit @ 12:00 CET gate (now w/ the observed 15:35 asymmetry as evidence); exchangeability paragraph (verbatim); negative-Q / isotonic-last; change-point validation of the crisis flags; regime-stratified story w/ crisis-peak F3 + negative-price F4; R-2 vintage honesty line ("as-archived, same convention as the published benchmarks — and I measured the publication timing myself"); gas-probe closure line ("I checked SMARD's API — volumes, not prices"); two-sided bounded tail; model-staleness; "why the German market, in Israel" + "why hourly" + "why marimo" + "why thin CI".
- **CP-5 cold-start:** 30s backstop; Actions cron auto-disables after 60 days of repo inactivity — README disclaimer is the real backstop.
- **CV iteration cadence:** three milestone-gated slots (after M2, M3, M5).
- **Track C flips:** recruiter-only open-to-work at the M2 phase-trigger; applications no later than start of Month 5.
- **Routing reminder:** the orchestrator never writes Claude Code prompts; M briefs inherit the v6.1 §12 binding list rather than restating the spec.
