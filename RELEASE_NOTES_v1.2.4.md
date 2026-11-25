# Ultra-Brain-Showcase v1.2.4: Measured Scorecard, Adversarial Review by Three AI Models and a 1,732-Case Stress Test

Date: 2026-11-25; times are UTC unless stated. Nothing in this release is a certification by any accredited body: it is a ledger of
measured figures and of judgments written by AI models. Every attack and every judgment was made by AI models from one
company (Anthropic); this is not an independent audit by people or by an outside organisation. No source code of the
private repositories is published.

Covered: the private main repository Ultra-Brain, commit `083af88` as measured and commit `2d08614` with the four
defects fixed; the private specification repository Ultra-Brain-Legacy, commit `bd138ec`, where the full measured ledger
is archived. The main repository's version label (1.13.0) did not change, and no tag or release was made in either
private repository.

## Documents

- [REVIEW_SCORECARD_2026-11-25.md](REVIEW_SCORECARD_2026-11-25.md): the scorecard in Korean and English, with a table of
  terms.
- [evidence/08-2026-11-25-measured-review.json](evidence/08-2026-11-25-measured-review.json): the figures, machine readable,
  with the SHA-256 of every measurement command.
- [SHA256SUMS_v1.2.4.txt](SHA256SUMS_v1.2.4.txt): the SHA-256 of the six files of this release (scorecard, release notes,
  evidence JSON, README, CHANGELOG, VERSION); a list cannot hold its own hash.

## Scorecard

| Item | Value |
|---|---|
| Identity check (Windows Hello), run once by the Owner | verified = true |
| Full regression, 6,888 tests, measured commit | PASS 6,792 / FAIL 39 / ERROR 40 / SKIP 17; 0 new regressions, 0 missing, designated core 191/191; every test's status identical to the baseline run; 35 min 14 s |
| Full regression, 6,888 tests, final commit | PASS 6,792 / FAIL 39 / ERROR 40 / SKIP 17; 0 new regressions, 0 missing, designated core 191/191; every test's status identical to the measured commit's run; 35 min 4 s |
| Command round trip, 100 samples | median 0.205 ms, p99 0.902 ms |
| C-only pipe meter, 5 runs of 136 samples | upper median 16.5 – 26.3 µs; all within the 38 µs latency budget |
| Attack by three AI models | Opus 23 attacks: 0 breached / 8 partial / 15 blocked. Sonnet: not run (BLOCKED). Fable 12 attacks: 3 breached / 4 partial / 5 blocked |
| Defects found and fixed | 4; the design was judged in three rounds until each judging model reported 0 blocking findings |
| Stress test, 1,732 combined cases | 0 crash, 0 lost acknowledged receipt, 0 lock left behind; no memory or handle growth proportional to cycles; 17 false integrity alarms, fixed (0 on re-run) |
| Release gates | leak scan 0, private cross-match 0 (specification repository: checked-out files only), wording audit 0 (no must-fix finding after five readings), SHA-256 list recomputed: 6 files, 100 % match |

The 39 FAIL and 40 ERROR results are inside the exempt set of 81 tests that the Owner exempted in earlier releases; not
every test passed. No remote CI run was started.
