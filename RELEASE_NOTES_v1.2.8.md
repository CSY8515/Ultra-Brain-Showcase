# Ultra-Brain-Showcase v1.2.8: The Final Part 1 Refactor

Date: 2026-11-28.

- This release is not a certification by any accredited body. It records measured figures and judgments written by AI
  models.
- Every judgment came from AI models of one company (Anthropic). It is not an independent audit by people or by an
  outside organisation.
- No source code of the private repositories is published.
- The Owner is the individual user who makes the final decisions for this project.

Terms used below:

- **Guard:** the main repository's own check layer, which judges each development act.
- **Green:** the guard's verdict level for an act with no risk signal. Such an act passes.
- **Slot:** a required field of an act, such as its target or its reason.
- **Seal:** a guard state that only the Owner may lift.
- **Native test directory:** the main repository's own unit tests of its native and guard layer. They are not among the
  6,888 tests of the full regression.
- **Designated core:** a fixed set of 191 tests inside the full regression that must all pass.

Covered:

- The private main repository Ultra-Brain, commit `1dca8a0`. Its base is commit `8262adb` from v1.2.7.
- The private specification repository Ultra-Brain-Legacy, commits `cae009c` (the design record, written before the main
  repository was changed) and `9048770` (the measurements).

The main repository's version label (1.13.0) did not change. Neither private repository got a tag or a release.

## Documents

- [evidence/12-2026-11-28-final-refactor.json](evidence/12-2026-11-28-final-refactor.json): the figures, machine readable.
- [SHA256SUMS_v1.2.8.txt](SHA256SUMS_v1.2.8.txt): the SHA-256 of the five files of this release (release notes,
  evidence JSON, README, CHANGELOG, VERSION). A list cannot hold its own hash.

## What was asked

The Owner asked for a last refactor of Part 1 (the guard layer that judges each development act) with three AI models
judging each step. The refactor was to:

- remove technical debt and unused code;
- make the guard's judgment faster (achieved for plain ASCII text only; Korean slot text is unchanged within noise);
- repair the gaps the refactor exposed;
- reduce the local disk use of the main repository without touching sources, evidence or the specification.

Two of the Owner's rules decided the outcome:

- A step that fails twice in a row is stopped and recorded, not tried a third time.
- No test, check or threshold may be weakened.

## How it was judged

Three judging models read every plan and every change separately: Claude Opus 5.5, Claude Sonnet 5.5 and Claude
Fable 5.2. The plans, the code and this release were written by Claude Opus 5.5 as technical lead. The Opus judgments
are therefore not independent of the author; Sonnet and Fable are different models. Blocking findings per round
(Opus / Sonnet / Fable):

| Round | Opus | Sonnet | Fable |
|---|---:|---:|---:|
| Design, round 1 | 4 | 6 | 4 |
| Design, round 2 (with a first implementation draft) | 6 | 3 | 6 |
| Design, round 3 (the narrowed decision) | 0 | 0 | 1 |
| Design, round 3b (that one finding) | - | - | 0 |
| Implementation, round 1 (the commit and the disk scripts) | 1 | 0 | 0 |
| Implementation, round 1b (that one finding) | 0 | - | - |

## What was stopped

The largest part of the draft was a rewrite of the reader that checks the guard's command log (which shell and git
commands an act ran).

- In round 1 the judges found gaps in the existing reader:
  - command spellings it does not see;
  - honest commands it reads as destruction, which would seal the guard until the Owner recovers it.
- In round 2, 14 of the 15 blocking findings were on the rewrite. Five of them were new defects made by the draft itself.
- The builder had also fixed one test expectation from the draft's own output, and so recorded a regression as the
  expected answer.

By the Owner's rule the rewrite was stopped and is **not** in this release. The existing reader is unchanged, and the gaps
the judges found in it stay open. They are recorded privately, by judge and with a command that reproduces each one, as
the starting point of the next stage. That stage starts with a frozen record of the old reader's answers, and every new
rule must keep them except for named exceptions.

## What changed in the main repository

- **A faster Green judgment for plain ASCII text.**
  - The check that the target, the reason and the other slots are really filled now answers plain ASCII text directly.
  - Before, it looked up every character in Unicode tables. Text with other characters, Korean included, takes the
    unchanged path.
  - For plain text values the answers are the same as before on every text tested:
    - all 2,113,665 ASCII texts of length 0 to 3;
    - 21,218,430 texts of length 4 to 5 built from the character classes;
    - 300,000 mixed and 19,208 decorated texts.
  - A new test compares the new code with a frozen copy of the old functions.
- **A gap closed.** The same check now accepts only plain text values. Before, a crafted text object could pass an empty
  target and reason, a right-to-left override or a NUL character as filled.
- **Unused code removed.** Three internal definitions that nothing read were removed. One of them looked like the
  destroy rule in force but was never read.
- **Kept on purpose.** Public names that nothing uses were kept, because the repository's own rule is to register
  unused public names, never to delete them. The Owner decides their fate.

## Scorecard

| Item | Value |
|---|---|
| Guard judgment, Green, ASCII slot text (100,000 calls, the same benchmark as v1.2.7) | median 0.0059 ms, p99 0.0154 ms (budget 0.4 ms); v1.2.7: 0.0157 ms |
| Slot texts alone, ASCII / Korean (a separate benchmark of the slot check; 100,000 calls, median of three rounds) | 0.0061 ms / 0.0219 ms; before: 0.0158 ms / 0.0221 ms (Korean: unchanged within noise) |
| Full regression, 6,888 tests, run 1 | PASS 6,791 / FAIL 39 / ERROR 41 / SKIP 17. The one extra error is a known race in the test harness's own process-kill check, hit by an agent-review bridge subprocess test. That test passed 3 times out of 3 alone. |
| Full regression, run 2 (same commit) | PASS 6,792 / FAIL 39 / ERROR 40 / SKIP 17; 191 of 191 tests of the designated core; 0 new regressions; every test's status identical to v1.2.7 |
| Native test directory | 1,212 run, OK (23 skipped); v1.2.7: 1,208 run (20 skipped) |
| Combined cases, 2,264 | identical to v1.2.7 case by case |
| Joint chain, 235 rows | the same verdicts as v1.2.7 (7 rows differ only in timing, commit ids and lock-message text) |
| Release gates | leak scan 0, private cross-match 0, wording audit 0, SHA-256 list recomputed: 5 files, 100 % match |

- The 39 FAIL and 40 ERROR results (79 tests) are inside the set of 81 tests that the Owner exempted in earlier releases.
  Not every test passed.
- Skipped native tests: 21 of them were listed with their reasons:
  - 19 because Windows Smart App Control blocks this build's images;
  - 1 because the account cannot create a symbolic link;
  - 1 because no specification checkout was named.

  None is in a changed file. The three skips more than v1.2.7 were not tied to named tests. This is consistent with
  Smart App Control blocking per image, but it is not verified.
- One benchmark item was not run: the command round trip of the native engine. Smart App Control blocked this build's
  engine image. A second image of the same source is not tried again, by the project's own rule.

## Disk

| Step | Result |
|---|---|
| Repository history | A byte copy was taken first and compared file by file. The history was then repacked with every expiry set to "never". Objects 7,708 before and 7,708 after, 0 lost. References, reference logs, work trees and settings identical (only the file that packs the references changed; the references are the same). The integrity check is clean before and after. Size 78.1 MB to 70.2 MB. |
| Transparent NTFS compression of an allow-list | 28,086 files: test-run databases in test residue folders and JavaScript library files of the user interface. Every file was hashed before and after. 2.60 GB on disk became 0.44 GB. Content and timestamps are unchanged; only the compression flag was set. No folder attribute was set. No other file was compressed. Eight folders could not be listed (their permissions do not allow it); this is reported. |
| Main repository on disk | 7.93 GB to 5.97 GB (24.7 % less; GB = 10^9 bytes: 7,933,132,800 to 5,972,267,008 bytes). Over the same hours the logical size grew by 199 MB (the test runs and the history copy). |

- Nothing was deleted except two folders of Python byte-code caches.
- Earlier records, release evidence, protected data, the build environment and the installers were left as they were.
- Sensitive-looking files were not compressed. The builder's own early size measurement did read them. It recorded
  each file's size and SHA-256 digest; no content was printed or sent. That broke the project's rule against reading
  such files, and deleting that record is left to the Owner (decision 3 below).
- The directive named 6.75 GB. The measured figure at the start was 7.93 GB on disk.

## Left to the Owner

Five decisions:

1. the unused public names;
2. whether to delete the test residue folders, which were compressed only;
3. the builder's early size-and-hash record, which holds digests of sensitive-looking files and should not have been
   made;
4. the history copy;
5. whether and how to resume the command log reader rewrite.

## Not run

- the full YAML parse of the CI workflow (no parser on this machine; read by hand, no defect found);
- remote CI (no run was started);
- production data;
- long-duration operation.

## 한국어 요약

- **결정**
  - 소유자가 Part 1 마지막 리팩토링을 지시했습니다. 3개 AI 모델이 매 단계를 따로 판정했습니다.
  - 명령 기록 판독기의 재작성은 두 판 연속 차단 사항이 나와, 소유자의 "같은 항목 2회 연속 실패 시 중단" 규칙에 따라 멈췄습니다.
  - 두 번째 판의 차단 사항 15건 중 14건이 이 재작성에 관한 것이었고, 그중 5건은 초안이 새로 만든 결함이었습니다.
  - 기존 판독기는 바뀌지 않았고, 판정관이 찾은 틈은 열린 채로 비공개 기록에 남았습니다.
- **반영**
  - Green 판정의 ASCII 빠른 경로를 넣었습니다. 바뀌기 전과 답이 같음을 2,113,665개 전수와 21,218,430개 문자 부류 조합으로 확인했습니다.
  - 일반 문자열이 아닌 값을 거절해, 빈 대상·사유가 채워진 것으로 통과하던 틈을 닫았습니다.
  - 아무도 읽지 않던 내부 정의 3개를 지웠습니다.
  - 쓰이지 않는 공개 이름은 저장소 규칙에 따라 지우지 않았습니다.
- **판정 차단 사항(Opus / Sonnet / Fable)**: 설계 1회차 4 / 6 / 4, 2회차 6 / 3 / 6, 3회차 0 / 0 / 1 → 0, 구현 1회차 1 / 0 / 0 → 0.
  - 계획·코드·이 릴리즈는 Claude Opus 5.5(기술 총괄)가 썼으므로 Opus의 판정은 작성자와 독립적이지 않습니다. Sonnet과 Fable은 다른 모델입니다.
- **측정**
  - 가드 판정 중앙값(ASCII 슬롯 기준): 0.0157 ms → 0.0059 ms. 한국어 슬롯은 0.0221 → 0.0219 ms로 사실상 같습니다(빠른 경로는 ASCII만).
  - 전수 회귀 2회차: 통과 6,792 / 실패 39 / 오류 40 / 건너뜀 17, 신규 회귀 0. 1회차의 오류 1건은 하니스 자체 가드의 알려진 경합이었습니다.
  - 자체 시험 디렉터리(가드·네이티브 층 단위 시험, 전수 회귀에 포함되지 않음): 1,212건 실행, 이상 없음(건너뜀 23).
  - 고밀도 2,264건과 합동 연쇄: v1.2.7과 같았습니다.
- **디스크**: 7.93 GB → 5.97 GB(24.7 % 감소).
  - 저장소 이력: 바이트 사본을 먼저 만들고 다시 묶었습니다. 객체 0개 손실입니다.
  - 압축: 허용 목록의 시험 DB·라이브러리 파일만, 전후 해시를 대조해 투명 압축했습니다.
  - 기록은 지우지 않았습니다.
  - 빌더의 초기 크기 측정이 민감해 보이는 파일의 크기와 SHA-256을 기록했습니다(내용은 출력·전송하지 않음). 규칙 위반이며 그 기록의 삭제는 소유자 결정입니다.
- **소유자 결정 5건이 남았습니다.** 원격 CI 실행은 0건입니다.
