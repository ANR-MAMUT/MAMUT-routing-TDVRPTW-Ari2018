# Ari2018 TDVRPTW benchmark family (Arigliano-generator TD travel times, curated VRP variant)

Satellite benchmark repository of [MAMUT-routing](https://github.com/ANR-MAMUT/MAMUT-routing) (ANR MAMUT, ANR-22-CE22-0016), mounted there as `benchmarks/TDVRPTW/Ari2018`. Self-contained: instances, canonical arrival-time-function (ATF) sidecars and best-known solutions ship together.

## What this family is — and what it is not

The raw data is the classic **TD-TSPTW** benchmark of the Arigliano et al. lineage: a single uncapacitated vehicle, no demands, no fleet size, no service times; per instance a symmetric distance matrix, integer time windows, and IGP time-dependent travel speeds (73 speed zones, 3 speed profiles shared by all arcs; congestion depth Delta ∈ {70, 80, 90, 95, 98}, traffic patterns A/B, TW-width parameter β·100 ∈ {0, 25, 50, 100}, three TW classes A/B/C, ten instances each; sizes 15/20/30/40 customers — a 4800-instance full factorial).

This canonical family is the **VRP variant curated by Onyr (2024–2026)**: demands, vehicle capacities, fleet sizes and gaussian service times were generated once by the legacy TDVRPTW-benchmarks pipeline and are pinned here verbatim (the generation was unseeded, so the values — recorded per instance in `metadata.legacy_attribute_source` — are the authoritative definition); raw Arigliano time windows kept verbatim. It is therefore **not** comparable with published TD-TSPTW results on the underlying raw files.

## Curated subset (deterministic)

The family ships **160 instances**, not the full 4800: one per structural cell `(n, Delta, pattern, width)`, chosen by a rule that is a pure function of the raw tree and the legacy BKS store — candidates holding a legacy best-known solution win first, remaining ties are decided by ascending SHA-256 of the canonical instance name (e.g. `sha256("Ari-C3-pA-d98-w50")`). Instance names `Ari-<Class><id>-p<Pattern>-d<Delta>-w<Width>` under `n=<customers>` carry all structural parameters. The full factorial raw trees remain available upstream for anyone needing the complement.

## Format

- `<Name>.vrp.json` — MAMUT shape: depot 0, mandatory display `coordinates` (deterministic stress-layout embedding of the symmetric raw distance matrix; quality in `metadata.coordinates_method`), `horizon` `[0, depot due date]`, `td` block with the sidecar's storage-independent sha256.
- `<Name>.atf.json.gz` — canonical ground truth: one arrival-time NDCPWLF per arc, exact IGP consolidation (breakpoints exactly where departure or arrival crosses a speed-zone boundary, each re-evaluated by the exact forward Ichoua loop; the last zone's speed extends beyond the horizon). All sidecars are gzipped in this satellite.
- `<Name>.bks.Duration.json` — best known solution under the `Duration` objective; costs are always the canonical checker's output.

Population pipeline: `populate_td_ari_vu` v1 (curation tooling maintained outside this repository; links will be published with the benchmark paper).

## Provenance and licensing

Underlying raw data: the Arigliano et al. TD-TSPTW benchmark generator distribution. MAMUT-authored curation artifacts under the MIT license; this notice does not relicense the underlying third-party benchmark definitions.
