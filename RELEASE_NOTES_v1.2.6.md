# Ultra-Brain-Showcase v1.2.6: The Owner's Decision on the Two Open Items of v1.2.5

Date: 2026-11-27. This release is not a certification by any accredited body. It records measured figures and
judgments written by AI models. Every attack and every judgment came from AI models of one company (Anthropic), so this is
not an independent audit by people or by an outside organisation. No source code of the private repositories is
published.

Covered:

- The private main repository Ultra-Brain, commit `b41c685`. Its base is commit `9aea59e`, the final commit of v1.2.5.
- The private specification repository Ultra-Brain-Legacy, commit `387cc2f`. It holds the full measured ledger.

The main repository's version label (1.13.0) did not change. Neither private repository got a tag or a release.

## Documents

- [evidence/10-2026-11-27-owner-decision.json](evidence/10-2026-11-27-owner-decision.json): the figures, machine
  readable.
- [SHA256SUMS_v1.2.6.txt](SHA256SUMS_v1.2.6.txt): the SHA-256 of the five files of this release (release notes,
  evidence JSON, README, CHANGELOG, VERSION). A list cannot hold its own hash.

## What the Owner decided, and the safety check made before any code

v1.2.5 left two items for the Owner. The Owner took both, with one condition each. Both conditions were checked
before any code was written.

| Item | The Owner's condition | Result |
|---|---|---|
| Reading NTFS alternate data streams (hidden bytes beside a file or a folder) | the new operating-system call must be a dedicated, minimum-privilege, read-only interface that opens no new way to start a program or to reach a path outside the repository | **Passed, in a narrower form than first offered** |
| A keyed witness for the guard's own store (against a consistent rewrite by the same Windows user) | report before building if it needs the Owner's Windows Hello again for every use instead of signing automatically inside the guard kernel | **Stopped before implementation** |

- **Alternate data streams.**
  - The first offer was one allowed line that kept a handle to the whole Windows system library. Through that handle,
    functions that start programs or load libraries can be reached.
  - What was built instead is three allowed lines. Each binds exactly one function: list the first stream, list the
    next stream, close the listing.
  - In the first implementation round, all three judging models showed that the bound functions still held the
    library object. Each function is now rebuilt from its bare address, and a test walks every object reachable from it
    and finds no library object among them.
- **Keyed witness.**
  - A key protected by Windows for the current user was opened by a second process of the same user in 2.25 ms with no
    prompt. A key the guard kernel can use automatically can therefore also be used by the same user to sign a forgery.
  - A key that does stop it is bound to Windows Hello, and every signature needs the Owner's gesture. That would mean
    one gesture on every write of the guard.
  - The item was stopped and reported. Its misses remain.
  - The Owner can choose one of three options:
    - Hello on every write;
    - signed checkpoints only at acts that already ask for Hello, with what happens after the last checkpoint stated;
    - a witness held by another account.

## Scorecard

| Item | Value |
|---|---|
| Hidden-stream attacks of v1.2.5 (on a file, on a folder) | missed → held for judgment on both (no penalty is applied; the actor can remove the stream and the hold lifts) |
| What is read | every tracked, untracked and ignored code file; every folder under the root, empty ones included; the files directly in the repository's own git folder. Windows' own download marks under their exact names are a minor anomaly (the lowest warning level); any other new or changed stream is held. |
| Blocking findings (Opus / Sonnet / Fable) | implementation round 1: 2 / 2 / 4; round 2: 0 / 0 / 1; round 3: 0 / 0 / 0 |
| Full regression, 6,888 tests | PASS 6,792 / FAIL 39 / ERROR 40 / SKIP 17; designated core 191/191; 0 new regressions; every test's status identical to the v1.2.5 final run |
| Native test directory | 1,182 run, OK (20 skipped); v1.2.5 had 1,167 |
| Combined cases, 2,264 | identical to v1.2.5 case by case: misses 54, false positives 3, 0 crash, 0 lost receipt, 0 lock left behind |
| Joint chain of v1.2.5, 235 rows (a separate set from the 2,264 cases) | only the two hidden-stream rows changed (missed → held); misses 51 → 49, false positives 4 → 4 |
| Guard judgment (100,000 calls) | median 0.0153 ms (budget 0.4 ms); v1.2.5 0.016 ms |
| One full guard measurement over a clean copy of the main repository | median 2.36 s, up from 1.59 s, about 1.5 times (the cost of reading the streams); 0 integrity alarms on the clean copy |
| Release gates | leak scan 0, private cross-match 0, wording audit 0, SHA-256 list recomputed: 5 files, 100 % match |

Terms: the guard kernel is the main repository's own check layer that judges each development act; the designated
core is the 191 tests the Owner marked as must-pass; the joint chain is the combined attack of
the three models in v1.2.5; held for judgment means the act is not accepted, no penalty is applied, and the actor may
undo the cause.

The 39 FAIL and 40 ERROR results are inside the set of 81 tests that the Owner exempted in earlier releases. Not every
test passed.

The first full-regression run on this commit was PASS 6,791 / FAIL 39 / ERROR 41 / SKIP 17, with designated core
191/191 and 1 new regression. That run is kept in the ledger, and the run reported above is the second one on the same
commit. The one test that errored waits on a connection worker. It does not use the guard code, and it passed 5 of 5
runs alone.

## What the three judging models found

| Round | Finding | Fix |
|---|---|---|
| 1 | The bound functions still held the system library object (all three) | each function rebuilt from its address; a test finds no library object reachable from them |
| 1 | Names compared after Unicode case folding, which Windows does not do: a payload could hide behind a look-alike name of a download mark (all three) | names kept exactly as Windows returns them; a mark only under its exact name |
| 1 | Folders without a code file, and the git folder, were not read (Fable) | every folder under the root is read |
| 1 | A name ending in a dot or a space was read as its trimmed twin (Fable) | every item read by its extended-length spelling |
| 2 | Control characters in a stream name reached the reason text (Fable) | every character outside visible ASCII is escaped |

## Ultra-Brain's own contribution, kept apart

- The guard kernel audited the builder's change. All five changed files were in the guarded part of the repository,
  and the guard flagged the two changed test files as changes to existing tests. It found no blocking fault.
- The project's own test, which lists every line allowed to start a process or load a library, caught a new test of
  the builder's that spelled those allowed lines as text.
- The project's regression runner, its test harnesses and its source inventory check judged the change: no regression.
- The three AI models found eight blocking findings from four causes in round 1 and one more in round 2.
- The builder (Claude Opus 5.5) wrote the pre-check, measured the key, and did the implementation, repairs, measurements
  and records. The judging models, Claude Opus 5.5 included, ran as separate sessions from the builder.

## Limits, stated

- The keyed witness is not built, and its misses remain until the Owner chooses.
- Stream reading leaves the following out:
  - files inside the git folder's subfolders;
  - ignored non-code files;
  - items below a skipped tool folder;
  - NTFS extended attributes;
  - a stream added and removed within one act;
  - a folder swapped for a link between the check and the read.
- A well-formed download mark can carry up to 4 KiB of text for one minor anomaly.
- No remote CI run was started.

---

# Ultra-Brain-Showcase v1.2.6 (한국어 요약)

v1.2.5에서 소유자 결정으로 남겨 둔 두 항목을 처리했습니다. 비공개 본 저장소의 커밋은 `b41c685`이고, 측정 원장은 비공개
사양 저장소 커밋 `387cc2f`에 있습니다.

- **대체 데이터 스트림 검사: 구현했습니다.**
  - 소유자의 조건은 새 운영체제 호출이 프로그램 실행이나 저장소 밖 경로로 이어지지 않는 전용 최소 권한 읽기 전용 호출이어야
    한다는 것이었고, 이를 통과했습니다.
  - 처음 제안한 형태보다 좁혔습니다. 허용 줄 3개가 각각 함수 하나만 묶고, 함수는 주소로 다시 묶었습니다.
    시험은 도달 가능한 객체 중에서 라이브러리 객체를 찾지 못합니다.
  - v1.2.5의 숨긴 스트림 공격 2건은 미탐에서 판단 보류로 바뀌었습니다.
- **열쇠 있는 증인: 구현 전에 멈추고 보고했습니다.**
  - 같은 사용자의 다른 프로세스가 사용자 범위 열쇠를 프롬프트 없이 2.25 ms에 열었습니다. 자동 서명 열쇠로는 같은 사용자의
    위조를 막지 못합니다.
  - 막을 수 있는 열쇠는 Windows Hello에 묶인 열쇠이고, 그러면 가드가 쓸 때마다 소유자의 손동작이 필요합니다.
  - 소유자의 선택지는 다음 셋입니다.
    - 쓰기마다 Hello
    - 이미 Hello를 받는 행위에서만 서명 체크포인트(마지막 체크포인트 이후의 상태를 명시)
    - 다른 계정의 증인
- **판정 차단 사항(Opus / Sonnet / Fable):** 구현 1회차 2 / 2 / 4, 2회차 0 / 0 / 1, 3회차 0 / 0 / 0.
- **전수 회귀 6,888건:** 통과 6,792 / 실패 39 / 오류 40 / 건너뜀 17, 핵심 191/191, 신규 회귀 0.
  - 같은 커밋의 첫 실행은 통과 6,791 / 오류 41이었습니다. 연결 작업자 시험 1건이 오류였고, 가드 코드를 쓰지 않는 시험이며
    단독 5회 모두 통과했습니다. 두 실행 모두 원장에 보관했습니다.
- **나머지 실측:**
  - 자체 시험 디렉터리: 1,182건 실행, 이상 없음(건너뜀 20).
  - 고밀도 조합 2,264건: v1.2.5와 케이스 단위로 같습니다.
  - 합동 연쇄: 미탐 51에서 49로 줄었고, 오탐은 4 그대로입니다.
  - 가드 판정 중앙값: 0.0153 ms.
  - 본 저장소 1회 측정: 1.59 s에서 2.36 s로 늘었습니다.
- 실패 39건과 오류 40건은 소유자가 앞선 릴리즈에서 면제한 81건 안에 있습니다.
- 스트림 검사가 읽지 않는 것: git 폴더의 하위 폴더 안 파일, 무시된 비코드 파일, 건너뛰는 도구 폴더 아래 항목, NTFS 확장 특성,
  한 행위 안에서 추가했다가 지운 스트림, 검사와 판독 사이에 링크로 바뀐 폴더. 형식이 맞는 다운로드 표시는 최대 4 KiB의 글을
  담을 수 있고, 이는 경미한 이상 한 번으로 처리됩니다.
- 원격 CI 실행은 0건입니다. 인증이 아니고, 모든 시험이 통과한 상태도 아닙니다.
