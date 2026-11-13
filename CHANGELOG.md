# Changelog

## v1.2.2 — 2026-11-13

Compound-command gate (Ultra Brain v1.8.2), registered character set (v1.8.3) and the 8th adversarial judgment.

- `BENCHMARK_V1.8.3.md`: measured figures of Ultra Brain v1.8.2 and v1.8.3 and Ultra-Brain-Legacy v1.68. Four CI runs per version, all success; one full-regression run per version, both 6,790 / 41 / 40 / 17 with 0 new regressions; eight soak samples (1,220,608 to 1,306,624 B) shown individually because the same binary varies by tens of thousands of bytes between runs.
- `EVALUATION_STAGE8_V183.md`: the builder's record (Claude Fable 5.2) and the unedited 8th adversarial judgment by Claude Opus 5.5. Verdicts: partly / partly (established inside the commander path) / not established. Of the ten 7th-judgment defects none is resolved, two are partly resolved, eight are not. New findings: variants of radar words pass, a connective can weaken the judgment, three entry points bypass the gate, the waiting evidence lists only eight clauses. The builder reproduced the claims and lists eight overclaims of its own release notes and R&D report.
- `evidence/06-v183-v168-benchmark.json`: the same figures in machine-readable form.
- `README.md`: repository table (v1.8.3, v1.68), new #6 section.
- Corrections recorded: 228,057 is the check count of the pre-implementation design prototype, not of the product; 1,232,896 B is 1.18 MiB and one sample of eight; the 7th judgment's Unicode-space finding concerns the R0 prover, not the command entrance; "ALL PASS" applies to the CI unit tests only; this repository has no CI of its own.
- Legacy v1.68 mounts the v1.8.2 headers; the v1.8.3 headers are not mounted (on hold by Owner decision).
- No source code of the private repository is published. Nothing here is a certification.

## v1.2.1 — 2026-11-13

6th-judgment fixes (Ultra Brain v1.8.1) and the 7th adversarial re-judgment.

- `BENCHMARK_V1.8.1.md`: measured figures of Ultra Brain v1.8.1, Ultra-Brain-Legacy v1.67 and Ultra-Brain-Labs v1.1.0. Both full-regression runs are recorded: the first ended 6,789 / 42 / 40 / 17 because an existing product test searches a JSON dump for the port string `9443` and a random SHA-256 digest contained it; the second ended 6,790 / 41 / 40 / 17 with 0 new regressions. The slower host-cost sample of v1.8.1 is recorded as measured.
- `EVALUATION_STAGE8_V181.md`: the builder's record (Claude Fable 5.2) and the unedited 7th adversarial judgment by Claude Opus 5.5. Verdicts are unchanged from the 6th: partly / partly / not established. Of the ten 6th-judgment defects one is resolved, eight partly, one not; new defects were found in v1.8.1. The builder reproduced the claims and lists six overclaims of the v1.8.1 release notes.
- `evidence/05-v181-v167-benchmark.json`: the same figures in machine-readable form.
- `README.md`: repository table (v1.8.1, v1.67), new #5 section.
- Corrections recorded: 1,286,144 B is 1.23 MiB; the 6th judgment ranked ten defects, not nine; "0 % hallucination closed loop" is not what the slot-bound rule gives.
- No source code of the private repository is published. Nothing here is a certification.

## v1.2.0 — 2026-11-13

Stage 8 Small Intelligence Commander: benchmark ledger and the 6th cross-appraisal.

- `BENCHMARK_V1.8.0.md`: measured figures of Ultra Brain v1.8.0, Ultra-Brain-Legacy v1.65 and Ultra-Brain-Labs v1.1.0, each with its CI run or measurement condition. Sections on latency, memory, determinism, regression, compilers, headquarters docking, a table that corrects the Owner directive's figures against the measurements, and the verified scope of the non-CI integrity mechanisms.
- `EVALUATION_STAGE8.md`: the builder's record (Claude Fable 5.2) and the unedited 6th adversarial judgment by Claude Opus 5.5, which answered the Owner's three questions with partly / partly / not established, plus the builder's reproduction of the judgment's counterexamples.
- `evidence/04-v180-v165-benchmark.json`: the same figures in machine-readable form.
- `README.md`: repository table, the #4 section with the benchmark table and the appraisal summary.
- Corrections recorded: 1,273,856 B is 1.21 MiB (not 1.27 MiB); no cloud-model latency ratio is published; DPAPI binds to the Windows user profile, not to a hardware key; SourceSeal was not verified for v1.8.0; no sandbox run exists for v1.8.0.
- No source code of the private repository is published. Nothing here is a certification.

## v1.1.0 — 2026-11-13

- `03_CORTEX_BENCHMARK_AND_CROSS_APPRAISAL.md`: C-Seed-1 Cortex benchmark receipts (Ultra Brain v1.7.90, Legacy v1.60) and the Fable 5.1 / Opus 5.5 cross-appraisal (5th judgment).

## Earlier

- `02_C-SEED-0_VS_CLAUDE.md` with four Opus 5.5 judgments, and the C-Seed-0 execution receipts and sandbox recordings (2026-11-11).
