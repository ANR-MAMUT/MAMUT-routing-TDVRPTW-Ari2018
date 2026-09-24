# Changelog — Ari2018 TDVRPTW BKS

All notable changes to the curated `Ari2018` TDVRPTW best-known solutions (BKS) are recorded here. Objective: **Duration** (duration minimization — the depot departure time of each route is a decision variable). Costs are the authoritative output of the canonical checker (`mamut_routing_lib.td.check_td_solution`): exact IEEE-754 double arithmetic, no epsilon thresholds, routes in canonical order (sorted by first customer), total summed in that order — so any strict improvement is real. Reminder: this family is the VRP variant curated by Onyr (see README.md), so these BKS are not comparable with published TD-TSPTW results on the underlying raw files.

## 2026-09-24

**Re-priced under the `td-fold/2` checker contract (mamut-routing-lib 0.12.0); no route changed.** mamut-routing-lib 0.12.0 replaces the TD checker's route fold (checker contract `td-fold/1` -> `td-fold/2`): waiting and service at a vertex are now applied exactly to the accumulated arrival times instead of being composed through a ratio interpolation, the departure window restricts the first arc without interpolation, and travel on slope-one pieces is computed by addition. Under `td-fold/1` a ready time could be off by an ulp, and on stepwise travel-time functions such an ulp could land past a step and read its upper branch. All 160 BKS were re-priced with `mamut-routing bks reprice-td`: 92 files were rewritten, 31 of them because the cost moved (by at most 4.1e-16 relative, float rounding only), the other 61 only to refresh `route_durations` / `route_departure_times` by ulps. Each BKS whose cost moved records `metadata.repriced` (previous cost, checker, contract, date). 7 optimality-stamped BKS moved (by at most 9.1e-13): their `proven_optimum` now equals the re-priced cost, and a `note` records that the proof was obtained under `td-fold/1`, that `dual_bound` is the prover's value under that arithmetic, and that re-certification under `td-fold/2` is pending.

## 2026-08-06

**All 40 optimality certificates re-derived from scratch and re-stamped** under the four-solve agreement protocol (cold and warm starts crossed with the two labeling modes, an audited exact-pricing phase in every run, zero checker-infeasible priced columns) on Grid'5000, on the repaired kayros 1.5.1 build. The campaign was motivated by the 2026-08-05 withdrawal of one certificate in the Vu2020 TDVRPTW family and the subsequent finding that the certifying builds carried since-repaired pricing defects (documented in the kayros 1.5.1 release notes and CHANGELOG). Every re-issued stamp certifies bit-exactly the previously stored value; no solution data changed, and the stamps' provenance now cites this campaign and build. One instance (Ari-B2-pB-d98-w100, n=15) exceeded the campaign's standard memory watermark and was re-derived in a dedicated high-memory run (peak resident set near 16.5 GB), certifying bit-exactly like the rest.

## 2026-07-12

**8 further BKS stamped proven optimal, 2 stale stamps retracted.** The weekend top-up of the exact re-certification campaign (2026-07-11/12, Grid'5000; time limit 1800 s per run) certified Ari-B2-pB-d98-w100, Ari-C3-pA-d98-w50, Ari-C10-pB-d95-w50 (n=15); Ari-B10-pA-d90-w100 (n=20); Ari-B2-pB-d98-w100, Ari-B5-pB-d80-w100, Ari-C4-pB-d70-w100, Ari-C5-pB-d95-w100 (n=30). Protocol unchanged from the 2026-07-10/11 campaign: four independent exact solves (cold and warm starts crossed with the two labeling modes) agreeing on the value, an audited exact-pricing phase in every run, zero checker-infeasible priced columns, and canonical-checker re-validation at stamping time. Separately, the last 2 stamps surviving from the superseded 2026-07-07 campaign (Ari-B10-pA-d90-w50 n=15, Ari-C10-pA-d98-w50 n=20, pre-audit prover metadata) are retracted: the pricing-ladder audit voided their trust basis and the audited campaigns leave both instances open, so their values return to ordinary best-known status. This family now carries 40 certified TDVRPTW BKS, all from the audited protocol.

## 2026-07-11

**32 BKS stamped proven optimal** (`metadata.optimality`) under a re-certification of the whole family: the earlier certificates' producer carried a pricing-ladder termination defect (an exact-pricing phase could be skipped), so all stamps were regenerated from scratch under a stronger protocol. Each stamp certifies four independent exact solves (cold and warm starts x two labeling modes) agreeing on the value, an audited exact-pricing phase in every run, zero checker-infeasible priced columns, and canonical-checker re-validation at stamping time. Instances whose runs disagreed, timed out, or priced a checker-infeasible column carry NO stamp.

## 2026-07-08

59 of 160 BKS improved (mean -0.54%, largest single improvement -2.52%) by a 20,808-run anytime-strategy head-to-head campaign on Grid'5000: kayros 0.4.0.dev0 (TD-ILS, TD-ACO+LS, and an ACO-then-ILS budget split, all over the granular time-dependent local search), per-size time limits (120 s for n<=30, 300 s for n<=60, 600 s for n<=100), seeds {42, 123, 456}, single-threaded runs. Improve-only fold: for each instance the campaign-best solution was re-priced by the canonical checker before writing (checker cost authoritative); stored BKS marked proven optimal were left untouched.

## 2026-07-07

8 BKS improved by exact solves — **proven optimal**: kayros 0.3.0 lera branch-price-and-cut (HiGHS backend, warm-started from the previous BKS, TL 600 s), from the certification campaign over all families n≤50 (improvements −0.05% to −3.0% across n=15..40). Certificates: optimal under checker-exact route costs and standard LP/pricing tolerances, completeness modulo Lera epsilon dominance. The same campaign certified 30 of the other stored TDVRPTW BKS of this family optimal as stored.

Structured optimality metadata: all 38 BKS of this family proven optimal by that campaign now carry a machine-readable `metadata.optimality` object — prover, certificate wording, proven optimum, dual bound, wall time (schema: `OptimalityMetadata`, mamut-routing-lib ≥ 0.4.0).

## 2026-07-06 — local-search sweep

152 BKS improved by the first sweep of kayros 0.2.0.dev0 TD-ACO with time-dependent local search (tree-evaluated VND, every accepted move repriced by the checker-identical fold), 10 seeds per instance on Grid'5000.

## 2026-07-06 — initial seeding sweep

155 BKS added and 1 improved, reaching full 160/160 coverage, from the initial large-scale seeding sweep across all four TD families run on 2026-07-04 (kayros 0.0.1 TD-ACO, Grid'5000, 10 seeds per instance, 13 520 runs total).

## 2026-07-03

Family populated (160 curated instances + ATF sidecars) with 5 BKS re-validated and re-priced from Onyr's legacy heuristic store (2024–2026 TDVRPTW-benchmarks pipeline).
