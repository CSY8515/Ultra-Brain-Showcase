# Ultra-Brain-Showcase v1.2.0: Small Intelligence Dual AI Benchmark & Certification

Date: 2026-11-13 (Asia/Seoul). The title is the Owner's release title. **Nothing in this release is a certification by any accredited body**: it is a ledger of measured figures and two model-written appraisals. No source code of the private repository is published.

Covered: Ultra Brain **v1.8.0** (code `da6debe`, release `f5313a1`), Ultra-Brain-Legacy **v1.65** (`bcd8e1a`), Ultra-Brain-Labs **v1.1.0** (`a69a0cd`).

## Documents
- [BENCHMARK_V1.8.0.md](BENCHMARK_V1.8.0.md) — the benchmark ledger, every figure with its CI run or measurement condition.
- [EVALUATION_STAGE8.md](EVALUATION_STAGE8.md) — the builder's record (Claude Fable 5.2) and the unedited 6th adversarial judgment (Claude Opus 5.5).
- [evidence/04-v180-v165-benchmark.json](evidence/04-v180-v165-benchmark.json) — the same figures, machine readable.

## Scorecard

| Item | Value | Source |
|---|---|---|
| Full regression, 6,888 cases, single run | **6,790 PASS / 41 FAIL / 40 ERROR / 17 SKIP**; 0 new regressions, 0 missing, 0 source changes during the run; designated core 191/191 | MAIN `da6debe`, 2026-11-13 06:46–07:23 UTC |
| Judged real-process checks / unit tests | 10/10 PASS / 220/220 PASS (136 + 84) | MAIN CI run 43381020952 |
| Compilers | Clang 20.1.8 `-std=c11 -O2 -Wall -Wextra -pedantic -Werror` and MSVC `/utf-8 /W4 /WX`: 0 warnings; self-test 17/17 | same run |
| Resident daemon, 30 s soak in a 15 MiB Job Object | job peak **1,273,856 B = 1.21 MiB**; engine CPU 0.0 s over the 2.0 s idle window | same run |
| Determinism | P6 golden receipts SHA-256 `b53b511e…` unchanged; 28 frames per cycle, 55 per cortex query; the eight cycle-path headers equal their v1.7.91 bytes | same run |
| Commander over the real engine | `read status` reported (28 frames, 0 differences); `delete system now` waits for an Owner nonce, 0 cycles | same run |
| Headquarters docking (Legacy v1.65) | runner 25/25, mesh 32/32, contract header blob identical to MAIN | Legacy CI run 43387715941 |
| Labs pipe IPC, 1,000 round trips | average 0.027–0.051 ms across three CI runs and two compilers; first round trip 8–15 ms | Labs CI runs 43373628526, 43374572593, 43374577834 |
| Engine warm round trip | median 0.244 ms is the 2026-11-11 pre-cortex build; **not measured for v1.8.0** | evidence/02 |

The 81 FAIL/ERROR cases are the historical Owner-exempt set; this is not raw ALL PASS. On the development PC Smart App Control blocked the v1.8.0 build, so every engine figure is a CI sample.

## The 6th adversarial judgment (Claude Opus 5.5), in one table

| Owner's question | Verdict |
|---|---|
| Does the `si_report` fill rule close a "0 % hallucination" report loop? | **Partly.** It is a "number-token provenance filter": it does not bind a value to its slot, does not check verdict words, and sees ASCII digits only |
| Is Rule #86 self-refactoring subordinate to the Owner's Rule #78 decision line, and safe? | **Partly.** Order and checks are enforced, but a caller can create the decision and the nonce; there is no actuator and no refactoring was performed |
| Category leap to an "ultra-light autonomous cognitive OS kernel with a self-tidying commander"? | **Not established.** The accurate name is "a deterministic command pipeline on top of a rule-based adaptive kernel" |

The builder reproduced the judgment's nine counterexamples on the same code; all were real. One sentence of the v1.8.0 release notes ("the only way a report can still be wrong is a wrong template") is recorded as an overclaim. These defects are present in v1.8.0 as released.

## Figures of the Owner directive that the measurements corrected
- 1,273,856 B is 1.21 MiB (1.27 is decimal MB).
- "4,000× a cloud LLM": no cloud model latency was measured; the ratio is not published.
- Labs IPC average: 0.027–0.051 ms over three runs (the directive's 0.031–0.051 covers two).
- "DPAPI HMAC hardware seal": HMAC-SHA256 under a key protected by Windows DPAPI for the current user profile. No hardware binding was verified, and HMAC is a signature, not encryption.
- "SourceSeal, 1,619 files, 0 errors": SourceSeal was neither renewed nor verified for v1.8.0. 1,619 is a v1.7.87 count. What v1.8.0 verified is 1,896 tracked files unchanged during the regression run.
- "Offline sandbox run": NOT_RUN for v1.8.0.
- "CI notarization": the runs are execution records on GitHub-hosted runners, not a notarization.

## Not published
Source code, test code and build scripts of the private repository; approval paths, seal files, ledgers, key files, installed executables.
