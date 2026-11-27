# Ultra-Brain-Showcase v1.2.7: The Guard's Owner Acts Bound to a Windows Hello Key

Date: 2026-11-27.

- This release is not a certification by any accredited body. It records measured figures and judgments written by AI
  models.
- Every attack and every judgment came from AI models of one company (Anthropic). It is not an independent audit by
  people or by an outside organisation.
- No source code of the private repositories is published.

Covered:

- The private main repository Ultra-Brain, commits `5291823` and `8262adb` (final). Their base is commit `b41c685` from
  v1.2.6.
- The private specification repository Ultra-Brain-Legacy, commit `82b948b`. It holds the full measured ledger.

The main repository's version label (1.13.0) did not change. Neither private repository got a tag or a release.

## Documents

- [evidence/11-2026-11-27-owner-route.json](evidence/11-2026-11-27-owner-route.json): the figures, machine readable.
- [SHA256SUMS_v1.2.7.txt](SHA256SUMS_v1.2.7.txt): the SHA-256 of the five files of this release (release notes,
  evidence JSON, README, CHANGELOG, VERSION). A list cannot hold its own hash.

## What was decided, step by step

v1.2.6 left one item for the Owner: a keyed witness (a signature that the same user cannot forge) for the guard's own store. It was meant to stop a consistent
rewrite by the same Windows user. Two safety checks were made before any code was written, and each one was reported
to the Owner.

1. **First check.** The Owner first chose signed checkpoints at acts that already ask for Windows Hello. The check found
   that the guard had no such act: its Owner route was unbound, so the product guard could not even be created. Built
   as asked, the option would make zero checkpoints. It was stopped and reported.
2. **Second check.** The Owner then chose to bind the guard's Owner route to Windows Hello. The check found that this
   still does not close the three same-user rewrites (a forged tail, a rollback, an emptied quarantine), because the
   list of signatures is itself a file the same user can write. Closing them needs a counter that only moves forward and
   that a standard user cannot reach. What binding does give:
   - the product guard becomes usable;
   - an Owner act line cannot be added without a valid Windows Hello signature over its position (the limits
     below still apply).
3. **The Owner's answer:** proceed anyway.

## What was built

- **One gesture per Owner act.** Each Owner act asks the Owner for one Windows Hello gesture. The Owner acts are: create
  the guard, lift a seal, lift a lock, resolve a judgment hold, switch the adversarial audit off, and recover.
- **What the gesture signs.** It signs the chain position the act follows: the repository, the act, the line number and
  the hash. The key is created in the Windows Hello container and is not exported by the guard. The public key is
  read from that
  container every time, never from the guard's own files.
- **What the guard refuses.** A chain is refused when an Owner line has no valid signature, or when the chain does not
  start with a signed creation or recovery.
- **When the gesture is asked.** Before the guard takes its write lock, and only when the act would otherwise proceed.
- **Windows calls.** Three Windows system functions, each bound by its address. The code uses two Windows interface
  classes and no others.

Terms: an Owner act is an act only the Owner may make; a seal and a lock are guard states that only the Owner may lift;
a judgment hold stops one change until it is resolved; the quarantine is a guard file whose entries keep the store
refused until the Owner recovers it; a tail is the lines written after a given point of the ledger.

## Scorecard

| Item | Value |
|---|---|
| Live check by the Owner (two Windows Hello gestures) | key created (success), RSA 2048; test signature signed and verified |
| Blocking findings (Opus / Sonnet / Fable) | round 1: 3 / 1 / 2; rounds 2, 3 and 4: 0 / 0 / 0 |
| Full regression, 6,888 tests, both commits | PASS 6,792 / FAIL 39 / ERROR 40 / SKIP 17; designated core 191/191; 0 new regressions; every test's status identical to the v1.2.6 run |
| Native test directory | 1,208 run, OK (20 skipped); v1.2.6 had 1,182 |
| Combined cases, 2,264 | identical to v1.2.6 case by case; 0 crash, 0 lost receipt, 0 lock left behind |
| Joint chain of v1.2.5, 235 rows | identical to v1.2.6 (misses 49, false positives 4) |
| Guard judgment (100,000 calls) | median 0.0157 ms (budget 0.4 ms) |
| Release gates | leak scan 0, private cross-match 0, wording audit 0, SHA-256 list recomputed: 5 files, 100 % match |

- The 39 FAIL and 40 ERROR results (79 tests) are inside the set of 81 tests that the Owner exempted in earlier releases. Not
  every test passed.
- One benchmark item was not run: the command round trip of the native engine. Windows Smart App Control blocked this
  build's engine image. A second image of the same source is not tried again, by the project's own rule. This item does
  not involve the guard.

## What the three judging models found (round 1)

| Finding | Fix |
|---|---|
| The guard checked a copy of its ledger read a moment after loading it; a file swapped in between was accepted (Opus) | the chain is re-derived from the bytes read and must end at the loaded head |
| One malformed signature line raised an error instead of being refused (Opus, Fable) | every such line is skipped; any other failure makes the store refused |
| A chain with no Owner line at all read as clean (Opus, Sonnet, Fable) | every chain must start with a signed creation or recovery |
| The stated limits were incomplete (Opus, Sonnet; Fable as a non-blocking note) | the limits below were added |

Opus's third finding and Sonnet's one finding each covered two of these causes, so the rows do not add up to the six
findings (Opus 3, Sonnet 1, Fable 2).

## Ultra-Brain's own contribution, kept apart

- **The guard kernel audited the builder's change.**
  - It placed all seven changed files in the guarded part of the repository and found no hard fault.
  - It flagged the new test file as not plain: code ran at import, and one test was skipped. The builder changed the
    file, and the kernel's final audit no longer flags it.
- **The project's own register of changed files** caught the two new files before they were registered.
- **The project's regression runner, test harnesses and source inventory check** found no regression.
- **The three AI models** found six blocking findings from four causes.
- **The Owner** made three decisions and two live gestures.
- **The builder (Claude Opus 5.5)** did the two safety checks and the implementation, repairs, measurements and
  records. The judging models ran as separate sessions.

## Limits, stated

- **The three same-user rewrites stay open:**
  - a forged tail after the last Owner line;
  - a rollback to any earlier signed point, including the creation point and signatures that outlived a recovery;
  - an emptied quarantine.
- **What a signature does not cover:**
  - It covers an Owner line's kind and position, not the line's own content.
  - The Windows Hello dialog does not show what is being signed. A gesture given to a prompt the Owner did not start
    signs whatever that requester asked for, so the Owner gestures only for acts the Owner started.
  - A signature made for an act the guard then refused remains valid at its position (it can be reused there).
- **Not run:**
  - whether Cancel closes the prompt;
  - whether deleting the key asks for a gesture;
  - production data, remote CI, long-duration operation.
- No remote CI run was started.

---

# Ultra-Brain-Showcase v1.2.7 (한국어 요약)

v1.2.6에서 남긴 "열쇠 있는 증인" 항목에 대해 소유자가 결정했고, 그 결과 가드의 소유자 행위를 Windows Hello 열쇠에 묶었습니다.

- 비공개 본 저장소의 커밋은 `5291823`과 `8262adb`(최종)이고, 측정 원장은 비공개 사양 저장소 커밋 `82b948b`에 있습니다.

**결정 경과**

- 코드를 쓰기 전에 두 번 점검했고, 두 번 모두 소유자에게 보고했습니다.
  1. 첫 선택(이미 Hello를 받는 행위에서만 체크포인트)
     - 가드에는 그런 행위가 없었습니다.
     - 그대로 구현하면 체크포인트가 0개이므로 멈췄습니다.
  2. 두 번째 선택(소유자 경로를 Hello에 바인딩)
     - 서명 목록도 같은 사용자가 쓸 수 있는 파일입니다.
     - 그래서 같은 사용자의 일관 재기록 3가지는 닫히지 않습니다.
- 소유자는 "그래도 진행"을 택했습니다.

**만든 것**

- 소유자 행위 6가지마다 Windows Hello 제스처 1회로 Hello 열쇠가 서명합니다.
- 서명 없는 소유자 줄, 또는 서명된 생성·복구로 시작하지 않는 사슬은 거부합니다.

**판정 결과**

- 판정 차단 사항(Opus / Sonnet / Fable)은 1회차 3 / 1 / 2였고, 2~4회차는 모두 0 / 0 / 0이었습니다.

**실측**

- 소유자 직접 실측: 열쇠 생성 성공(RSA 2048), 시험 서명 검증 통과.
- 전수 회귀 6,888건: 통과 6,792 / 실패 39 / 오류 40 / 건너뜀 17, 핵심 191/191, 신규 회귀 0.
  - 실패 39건과 오류 40건(79건)은 소유자가 앞선 릴리즈에서 면제한 81건 안에 있습니다.
- 자체 시험 디렉터리: 1,208건 실행, 이상 없음(건너뜀 20).
- 고밀도 조합 2,264건과 합동 연쇄: v1.2.6과 같습니다.
- 가드 판정 중앙값 0.0157 ms.
- 엔진 왕복 벤치 1건은 Windows 스마트 앱 컨트롤이 이번 빌드 이미지를 차단해 실행하지 않았습니다.

**남은 것**

- 같은 사용자의 재기록 3가지가 남습니다: 마지막 소유자 줄 이후의 위조, 서명된 지점으로의 되감기, 격리 비우기.
- 서명이 다루지 않는 것:
  - 소유자 줄 내용 자체는 서명되지 않습니다.
  - Hello 대화 상자는 서명 대상을 보여 주지 않습니다. 그래서 소유자가 직접 시작한 행위에만 제스처해야 합니다.
  - 가드가 결국 거부한 행위를 위해 만든 서명도 그 위치에서는 유효하게 남습니다.
- 실행하지 않은 것: 취소가 프롬프트를 닫는지, 열쇠 삭제에 제스처가 필요한지, 실제 운영 데이터, 장시간 운영.
- 원격 CI 실행은 0건입니다. 인증이 아니고, 모든 시험이 통과한 상태도 아닙니다.
