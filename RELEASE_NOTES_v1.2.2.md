# Ultra-Brain-Showcase v1.2.2: Compound-Command Gate, Registered Character Set and the 8th Adversarial Judgment

Date: 2026-11-13 (Asia/Seoul). **Nothing in this release is a certification by any accredited body**: it is a ledger of measured figures and two model-written records. No source code of the private repository is published.

Covered: Ultra Brain **v1.8.2** (code `b1b58fe`, release `142e1e1`) and **v1.8.3** (code `2e90677`, release `eed98d5`), Ultra-Brain-Legacy **v1.68** (`5f74841`). Ultra-Brain-Labs v1.1.0 is unchanged.

## Documents
- [BENCHMARK_V1.8.3.md](BENCHMARK_V1.8.3.md) — what the two releases changed and what was measured, every figure with its CI run or condition.
- [EVALUATION_STAGE8_V183.md](EVALUATION_STAGE8_V183.md) — the builder's record (Claude Fable 5.2) and the unedited 8th adversarial judgment (Claude Opus 5.5).
- [evidence/06-v183-v168-benchmark.json](evidence/06-v183-v168-benchmark.json) — the same figures, machine readable.

## Scorecard

| Item | Value | Source |
|---|---|---|
| Full regression, 6,888 cases, one run per version | both **6,790 PASS / 41 FAIL / 40 ERROR / 17 SKIP**; 0 new regressions, 0 missing; designated core 191/191 | MAIN `b1b58fe` and `2e90677`, 2026-11-13 |
| Judged real-process checks / unit tests | v1.8.2: 11/11 PASS / 262/262 PASS (178 + 84). v1.8.3: 11/11 PASS / 265/265 PASS (181 + 84) | MAIN CI, four runs per version, all success |
| Compilers | Clang 20.1.8 `-std=c11 -O2 -Wall -Wextra -pedantic -Werror` and MSVC `/utf-8 /W4 /WX`: 0 warnings; self-test 18/18 | same runs |
| Runner image | 278,016 B (v1.8.2), 284,160 B (v1.8.3); limit 307,200 B | same runs |
| Resident daemon, 30 s soak in a 15 MiB Job Object, eight samples | job peak **1,220,608 to 1,306,624 B (1.16 to 1.25 MiB)**; engine CPU 0.0 s over the 2.0 s idle window in all eight | same runs |
| Determinism | P6 golden receipts SHA-256 `b53b511e…` unchanged; the eight cycle-path headers equal their v1.7.91 bytes | same runs |
| Headquarters docking (Legacy v1.68) | runner 26/26, mesh 33/33; contract header blobs identical to MAIN **v1.8.2** | Legacy CI runs 43423157347 (tag), 43423001643 (main) |

The 81 FAIL/ERROR cases are the historical Owner-exempt set; this is not raw ALL PASS. On the development PC Smart App Control blocked the builds, so every engine figure is a CI sample. The same binary's soak peak varies by tens of thousands of bytes between runs, so the eight samples are listed one by one in the benchmark document.

## What the two releases changed
- **v1.8.2, compound-command gate.** A command is split into clauses, every token of every clause is judged, and a radar (30 tokens, 9 patterns, a bare drive designator) runs over the whole text. The three commands the 7th judgment gave (`read status and format c:`, `check and wipe disk`, `show everything then erase disk`) now wait for the Owner.
- **v1.8.3, registered character set (Legacy Rule #97).** A command holding anything other than Hangul syllables, printable ASCII, tab or line ends, or bytes that are not UTF-8, is refused whole at accept. Characters are neither stripped nor folded. An Owner token offered with such a command is not consumed.

## The 8th adversarial judgment (Claude Opus 5.5)

| Owner's question | Verdict |
|---|---|
| Is the compound-command disguise completely blocked? | **Partly.** The 7th judgment's three counterexamples and the "listed word behind a first clause" kind are closed. "Completely" does not hold |
| Is the fullwidth / Unicode-space bypass completely blocked? | **Partly; established inside the commander path.** The premise differs from the 7th judgment's text |
| Reliability and "cognitive kernel" status of the kernel with the meta sector | **Not established.** "A deterministic command pipeline on a rule-based adaptive kernel, with a syntactic filter in front of it" |

- Of the ten 7th-judgment defects: 0 resolved, 2 partly resolved, 8 not resolved. The two releases aimed at one of them.
- New findings: variants of radar words pass without the Owner (`reformat`, `shutdown_now`, `power_off`, `re-boot`); a connective can weaken the judgment (`read status run payload` waits, `read status and run payload` passes); the control tower view, the resident daemon and the dock command line do not go through these rules; the waiting evidence lists only eight clauses and shows the first verb's risk; a refused command still loads the engine and exchanges the greeting and closing frames; many harmless inputs wait (`read status?`, `please read status`, `show ledger -v`).
- Every attack of the judgment on the registered character set failed inside the commander path.
- The builder reproduced the claims on the same code and accepts eight sentences of its own release notes and R&D report as overclaims.
- These defects are present in v1.8.3 as released. There is still no semantic understanding and no natural-language reasoning.

## Wording of the Owner directive that the record corrects
- "Unit 265/265 ALL PASS": 265 is the CI unit-test count. The full regression has 81 historical exemptions.
- "0 new defects": 0 new regressions against the baseline.
- 1,232,896 B is 1.1758 MiB: 1.18 rounded. It is one of eight samples; the largest is 1,306,624 B.
- "228,057 verified" for v1.8.2: that is the number of property checks run on the design prototype before implementation, not on the product.
- "The fullwidth and Unicode-space bypass the 7th judgment pointed out": the 7th judgment's Unicode-space finding concerns the R0 prover, which v1.8.3 does not change and which still fails. Fullwidth and zero-width characters at the command entrance were not on its defect list.
- "Confirm CI, then publish": this repository has no CI. The CI that was confirmed is MAIN's and Legacy's.
- Legacy v1.68 mounts the v1.8.2 headers. The v1.8.3 headers are not mounted there; the re-mount is on hold by Owner decision.

## Not published
Source code, test code and build scripts of the private repository; approval paths, seal files, ledgers, key files, tokens, installed executables.
