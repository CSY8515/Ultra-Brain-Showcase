# Ultra-Brain-Showcase v1.2.5: New Attack Variables, a Joint Chain Attack by Three AI Models and 2,264 Combined Cases

Date: 2026-11-26. Nothing in this release is a certification by any accredited body: it is a ledger of measured figures
and of judgments written by AI models. Every attack and every judgment was made by AI models from one company
(Anthropic); this is not an independent audit by people or by an outside organisation. No source code of the private
repositories is published.

Covered: the private main repository Ultra-Brain, commit `553d82b` as measured and commit `9aea59e` with the defects
fixed (code commits `08da2c5`, `cb0cefa` and `9aea59e`); the private specification repository Ultra-Brain-Legacy, commit
`315782e`, where the full measured ledger is archived. The main repository's version label (1.13.0) did not change, and no
tag or release was made in either private repository.

## Documents

- [REVIEW_SCORECARD_2026-11-26.md](REVIEW_SCORECARD_2026-11-26.md): the scorecard in Korean and English, with a table of
  terms.
- [evidence/09-2026-11-26-joint-chain.json](evidence/09-2026-11-26-joint-chain.json): the figures, machine readable.
- [SHA256SUMS_v1.2.5.txt](SHA256SUMS_v1.2.5.txt): the SHA-256 of the six files of this release (scorecard, release notes,
  evidence JSON, README, CHANGELOG, VERSION); a list cannot hold its own hash.

## Scorecard

| Item | Value |
|---|---|
| Identity check (Windows Hello), run once by the Owner | verified = true |
| Attack variables (design and execution) | Opus 19, Sonnet 116, Fable 14, the builder 9 |
| Joint chain, four steps | baseline commit: all four steps passed; final commit: stops at the first step (fail-closed) |
| Combined cases, 2,264 (0 duplicate ids) | misses 590 / false positives 5 → misses 54 / false positives 3 (the remaining misses are recorded static-analysis limits); 0 crash, 0 lost receipt, 0 lock left behind |
| Blocking findings (Opus / Sonnet / Fable) | 0 / 0 / 0 after 8 design rounds; 0 / 0 / 0 after 6 implementation rounds |
| Full regression, 6,888 tests, final commit | PASS 6,792 / FAIL 39 / ERROR 40 / SKIP 17; 0 new regressions, 0 missing, designated core 191/191; every test's status identical to the baseline run |
| Native test directory, final commit | 1,167 run, OK (20 skipped) |
| Guard judgment, final commit (100,000 calls) | median 0.016 ms, p99 0.055 ms (budget 0.4 ms); baseline median 0.0042 ms |
| Release gates | leak scan 0, private cross-match 0, wording audit 0, SHA-256 list recomputed: 6 files, 100 % match |

The 39 FAIL and 40 ERROR results are inside the exempt set of 81 tests that the Owner exempted in earlier releases; not
every test passed. The first full-regression run on the final commit was PASS 6,791 / FAIL 39 / ERROR 41 / SKIP 17
with designated core 190/191 and 1 new regression: a connection readiness test waited more than 10 s for a child
process to exit. That test does not use the guard code and passed 5 of 5 runs alone; both runs are kept in the ledger.
Two items that need the Owner's decision were not fixed. No remote CI run was started.
