# Ultra-Brain-Showcase v1.2.1: 6th-Judgment Fixes and the 7th Adversarial Re-Judgment

Date: 2026-11-13 (Asia/Seoul). **Nothing in this release is a certification by any accredited body**: it is a ledger of measured figures and two model-written records. No source code of the private repository is published.

Covered: Ultra Brain **v1.8.1** (code `3c51e15`, release `9f19b88`), Ultra-Brain-Legacy **v1.67** (`e583aaf`), Ultra-Brain-Labs **v1.1.0** (`a69a0cd`).

## Documents
- [BENCHMARK_V1.8.1.md](BENCHMARK_V1.8.1.md) — what v1.8.1 changed and what was measured, every figure with its CI run or condition.
- [EVALUATION_STAGE8_V181.md](EVALUATION_STAGE8_V181.md) — the builder's record (Claude Fable 5.2) and the unedited 7th adversarial judgment (Claude Opus 5.5).
- [evidence/05-v181-v167-benchmark.json](evidence/05-v181-v167-benchmark.json) — the same figures, machine readable.

## Scorecard

| Item | Value | Source |
|---|---|---|
| Full regression, 6,888 cases, second run | **6,790 PASS / 41 FAIL / 40 ERROR / 17 SKIP**; 0 new regressions, 0 missing; designated core 191/191 | MAIN `3c51e15`, 2026-11-13 09:01–09:40 UTC |
| Full regression, first run | 6,789 / 42 / 40 / 17; one PASS-to-FAIL case | same commit |
| Judged real-process checks / unit tests | 10/10 PASS / 238/238 PASS (154 + 84) | MAIN CI run 43392553181 (success on the first attempt) |
| Compilers | Clang 20.1.8 `-std=c11 -O2 -Wall -Wextra -pedantic -Werror` and MSVC `/utf-8 /W4 /WX`: 0 warnings; self-test 17/17 | same run |
| Resident daemon, 30 s soak in a 15 MiB Job Object | job peak **1,286,144 B = 1.23 MiB**; engine CPU 0.0 s over the 2.0 s idle window | same run |
| Determinism | P6 golden receipts SHA-256 `b53b511e…` unchanged; the eight cycle-path headers equal their v1.7.91 bytes | same run |
| Headquarters docking (Legacy v1.67) | runner 25/25, mesh 32/32, contract header blob identical to MAIN v1.8.1 | Legacy CI run 43405793943 |

**Two regression runs.** In the first run an existing product test failed: it asserts that the port string `9443` is absent from a JSON dump that also holds random SHA-256 digests, and that run's staged digest began `4b969443792c`. The chance is about 0.5 % per run. Isolated reruns passed 6/6, no product file changed since v1.8.0, and the test was left unfixed. "0 new regressions" is a statement about the second run. The 81 FAIL/ERROR cases are the historical Owner-exempt set; this is not raw ALL PASS.

The single CI sample of host cost got worse in v1.8.1: report stage 448.6 µs (about 50 µs before), cycle round trip 3.987 ms (about 0.73 ms before). It was not measured repeatedly. On the development PC Smart App Control blocked the build, so every engine figure is a CI sample.

## The 7th adversarial judgment (Claude Opus 5.5)

| Owner's question | 6th (v1.8.0) | 7th (v1.8.1) |
|---|---|---|
| A "0 % hallucination" report loop? | Partly | **Partly.** "A slot-binding filter for numbers under a pinned template", not a closed loop |
| Rule #86 safely subordinate to the Owner? | Partly | **Partly.** The side that consumes a token holds the key that mints it; nothing actually runs |
| An autonomous cognitive OS kernel? | Not established | **Not established.** "A deterministic command pipeline on a rule-based adaptive kernel" |

- Of the ten 6th-judgment defects: 1 resolved, 8 partly resolved, 1 not resolved. Two were judged "the weakness only moved".
- New findings in v1.8.1: the verifying side can mint tokens with the same symmetric key; proof records are unsigned dicts and can be fabricated; deleting the token ledger or opening it twice lets a token be reused, and concurrent use leaves the ledger permanently broken; compound commands such as `read status and format c:` pass the first-verb gate; the R0 prover misses `//` inside header names, trigraphs and Unicode spaces.
- The builder reproduced these claims on the same code and accepts six sentences of the v1.8.1 release notes as overclaims.
- No Windows Hello binding exists. An Owner token proves that some process of the same Windows user unwrapped the key, not that a person decided. There is still no semantic understanding and no natural-language reasoning.
- These defects are present in v1.8.1 as released.

## Wording of the Owner directive that the record corrects
- 1,286,144 B is 1.2266 MiB: 1.23 rounded (1.22 is truncation).
- The 6th judgment ranked ten defects; nine counterexamples were reproduced.
- "0 % hallucination closed loop held": the slot-bound rule checks where a number came from and where it sits. It does not guarantee that a report is true. Both judgments say "partly".
- "Arbitrary nonces blocked": integers are refused, but a process of the same Windows user can mint a token.

## Not published
Source code, test code and build scripts of the private repository; approval paths, seal files, ledgers, key files, tokens, installed executables.
