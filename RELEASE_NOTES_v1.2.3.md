# Ultra-Brain-Showcase v1.2.3: Closed-Lexicon Gate, Legacy v1.69 and the 9th Adversarial Judgment

Date: 2026-11-14 (Asia/Seoul). **Nothing in this release is a certification by any accredited body**: it is a ledger of measured figures and three model-written records. No source code of the private repository is published.

Covered: Ultra Brain **v1.8.4** (code `549f049`, release `44469ae`) and Ultra-Brain-Legacy **v1.69** (`f375aac`). Ultra-Brain-Labs v1.1.0 is unchanged.

## Documents
- [BENCHMARK_V1.8.4.md](BENCHMARK_V1.8.4.md) — what the release changed and what was measured, every figure with its CI run or condition, and the 43 counterexamples of the 8th judgment one by one.
- [EVALUATION_STAGE8_V184.md](EVALUATION_STAGE8_V184.md) — the builder's record (Claude Fable 5.2), the unedited 9th adversarial judgment (Claude Opus 5.5) and the unedited cross verification (Claude Sonnet 5.5).
- [evidence/07-v184-v169-benchmark.json](evidence/07-v184-v169-benchmark.json) — the same figures, machine readable.

## Scorecard

| Item | Value | Source |
|---|---|---|
| Full regression, 6,888 cases, one run | **6,790 PASS / 41 FAIL / 40 ERROR / 17 SKIP**; 0 new regressions, 0 missing; designated core 191/191 | MAIN `549f049`, 2026-11-13 |
| CI | **five runs failed, then four passed**; judged real-process checks 11/11 PASS; unit tests 282/282 PASS (198 + 84) | MAIN CI runs 43441112272, 43441553065, 43441571676 (`549f049`), 43448400861 (`44469ae`) |
| Compilers | Clang 20.1.8 `-std=c11 -O2 -Wall -Wextra -pedantic -Werror` and MSVC `/utf-8 /W4 /WX`: 0 warnings; engine self-test 16/16, contract checks 2/2 | same runs |
| Runner images | engine 241,664 B (284,160 B in v1.8.3), contract runner 225,280 B; limit 307,200 B each | same runs |
| Resident daemon, 30 s soak in a 15 MiB Job Object, four samples | job peak **1,224,704 to 1,339,392 B (1.17 to 1.28 MiB)**; two samples above 1,310,720 B; engine CPU 0.0 s when idle | same runs |
| The 43 counterexamples of the 8th judgment, as the builder reproduced them | v1.8.3: 43 ran a cycle without the Owner. **v1.8.4: 0; all 43 wait for the Owner** | builder measurement on the source of both tags; confirmed separately by both reviewers |
| Determinism | P6 golden receipts SHA-256 `b53b511e…` unchanged; the eight cycle-path headers equal their v1.7.91 bytes | MAIN CI |
| Headquarters docking (Legacy v1.69) | runner 26/26, mesh 33/33; contract header blobs identical to MAIN **v1.8.4**; three runs, all success at the first attempt | Legacy CI runs 43452571685 (tag), 43452226365 (main), 43451026772 |

The 81 FAIL/ERROR cases are the historical Owner-exempt set; this is not raw ALL PASS. On the development PC Smart App Control blocked the builds, so every engine figure is a CI sample. The five failed CI runs had two causes: a marker in the runner driver that still held the previous version's corpus figures, and an expected exit code 1 from the gate that ended a workflow step.

## What v1.8.4 changed
- **Closed lexicon.** After its verb, every word of a clause must be a registered noun (68), a particle (15), a number or a version literal. A word the tables do not know waits for the Owner.
- **Risk matrix, Owner threshold 2, compound rule.** A clause that starts with an execute verb always waits. A command of two or more clauses passes only if every clause is a read.
- **Early exit.** A refused or waiting command starts no engine process (0 frames).
- **Entry points.** The dock command line and the ordinary send of the monitor console go through the same gate first.
- **Contract runner.** The self-tests of the two contract headers left the engine image.

## The 9th adversarial judgment (Claude Opus 5.5) and the cross verification (Claude Sonnet 5.5)

| Owner's question | Claude Opus 5.5 | Claude Sonnet 5.5 |
|---|---|---|
| Are the 43 counterexamples of the 8th judgment completely blocked by the closed lexicon and clause decomposition? | **Partly.** The 43 strings do not pass through the gated paths. For the system as a whole it does not hold | **Partly.** "Completely" holds for the 43 only |
| Is Owner sovereignty completely established by D1, D3 and D4? | **Partly.** The defects of the approval mechanism are unchanged | **Not established.** Some state writes pass without the Owner |
| Can it be recognised as a defect-free deterministic security control kernel with zero counterexamples? | **Not established.** Only "deterministic" holds | **Not established.** Counterexamples were found |

- Of the thirteen 8th-judgment defects (Opus): 1 resolved (the engine was loaded before accept), 5 partly resolved, 6 not resolved; the over-waiting item changed character. Of the ten 7th-judgment defects: 8 not resolved.
- What the reviewers could not break: the 43 inputs and their variants (64 by Opus, 47 by Sonnet), any input holding an unregistered word, the clause bounds and the window, weakening by a connective, 1,440 generated variants of dangerous seeds.
- New findings:
  - **Commands made of registered words only pass without the Owner.** Write verbs on nouns the builder classed "public": `write receipt`, `update golden headers`, `store digest`, `set engine`. 312 of the 2,380 verb-noun commands.
  - **Reads of protected nouns pass without the Owner**: `read secrets`, `show password`, `read owner key`. 120 of 2,380.
  - The version literal accepts any dotted number: `set engine 10.0.0.1` passes.
  - The monitor console's probe button skips the dialog that lists the clauses and shows a 39-character preview.
  - The raw session functions, the daemon's data channel and the commander's internal cycle function are outside the gate; the test that pins the entry points catches none of five variants.
  - The registered word pair `for maturity` forms the root `format`, so a formal command waits at risk 3.
- The builder reproduced these claims on the same code and accepts nine sentences of its release notes and specification as overclaims or errors. One of them, "Korean commands always wait", had already been flagged by the 8th judgment and was repeated.
- These defects are present in v1.8.4 as released. A command that passes performs no action: the engine cycle does nothing outside itself. The reviewers point out that an executor, once built, would turn these passes into actions.

## Wording of the Owner directive that the record corrects
- "The 43 counterexamples pointed out by the 8th judgment": the judgment ran 41 inputs. 43 is the builder's count, with two limits it had disclosed itself.
- "Unregistered verb/object attacks": the 43 are of six kinds; 13 of them were clauses of registered verbs.
- "D1: Owner confirmation forced for execution and state writes at risk 2": true for execute verbs. A write passes when its noun is classed public.
- "D3: all 32 clauses visible": 32 is the size of the window. The waiting evidence lists every clause, beyond 32 too.
- "D4: dock and control tower entry points unified behind the commander's gate": the gate is on the dock command line and on the ordinary send of the monitor console. Raw paths remain, the console's approval is a dialog and not an Owner token, and "control tower" names a different component in the code.
- "Completely", "zero counterexamples", "defect-free", "recognised": the builder does not claim them and both reviewers answer the third question "not established".
- "Four-model line-up": Claude Fable 5.1 could not be run in this environment. It is recorded as **NOT_RUN**; no other model's output is presented as its view. Three models are recorded: Fable 5.2 (builder), Opus 5.5 and Sonnet 5.5, all products of the same company.
- "1.25 MiB" (headquarters invariant 84): exceeded in two of the four v1.8.4 samples. The 15 MiB limit holds.

## Not published
Source code, test code and build scripts of the private repository; approval paths, seal files, ledgers, key files, tokens, installed executables.
