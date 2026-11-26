# 신종 공격 변수 성적표와 AI 모델 합동 연쇄 공격 — 2026-11-26 / New Attack Variables and a Joint Chain Attack by Three AI Models — 2026-11-26

이 문서는 측정값과 판정 기록의 원장입니다. 어떤 공인 기관의 인증도 아니며, 비공개 저장소의 소스 코드는 싣지 않습니다.
공격과 판정은 모두 AI 모델이 수행했습니다. 사람이나 외부 기관의 독립 감사가 아닙니다.
This document is a ledger of measured figures and judgment records. It is not a certification by any accredited body, and
it publishes no source code of the private repositories. Every attack and every judgment was made by AI models; this is
not an independent audit by people or by an outside organisation.

---

## 한국어

### 0. 용어

| 용어 | 뜻 |
|---|---|
| 본 저장소 / 사양 저장소 | 제품 소스를 담은 비공개 저장소 Ultra-Brain / 사양서와 판정 기록을 담은 비공개 저장소 Ultra-Brain-Legacy |
| 소유자(Owner) | 이 프로젝트의 최종 결정권자인 개인 사용자 |
| 가드 커널 | 개발 행위 하나하나를 측정해 통과·관찰·보류·차단·봉인 중 하나로 판정하는 본 저장소의 자체 검사 계층 |
| 슬롯 | 행위 하나에 적어야 하는 대상·내용·위치·이유·방법·증거 해시 칸 |
| 명령 기록 | 행위자가 실행한 셸 명령의 목록. 행위자가 적는 것이므로 빠뜨릴 수 있고, 가드는 여기서 신호를 더하기만 함 |
| 원장 / 증인 / 격리 | 가드의 해시 사슬 기록 / 사슬 끝을 따로 보관한 파일 / 쓰지 못한 강경 사건을 모아 두는 파일(비어 있지 않으면 저장소 전체가 읽을 수 없음 상태) |
| 살라미 창 | 마지막 감사 이후 누적된 변경 줄 수의 상한(단계 예산의 2배) |
| 프로세스 종료 정적 검사기 | 소스 안의 프로세스·작업 종료 호출을 찾는 검사기. 두 가지 검출기의 합집합으로 판정 |
| 데모(Demo) 경계 | 공개용 파생 저장소에 실을 수 없는 비공개 모듈과 그 원문을 구분하는 규칙 |
| 전수 회귀 시험 / 핵심 시험 / 면제 집합 | 6,888건의 시험 / 그중 반드시 통과해야 하는 191건 / 이전 릴리즈에서 소유자가 사유를 기록하고 면제한 실패·오류 81건 |
| 자체 시험 디렉터리 | 본 저장소의 C·네이티브 계층 시험 묶음(전수 회귀 6,888건과 별도) |
| 자체 검증 계측기 | 탑재된 헤더가 사양 저장소의 정본과 같은지 등을 검사하는 도구 |
| 미탐 / 오탐 | 공격인데 통과함 / 정상 작업인데 막힘 |
| 차단 사항(판정) | 판정 모델이 "이대로 진행하면 안 된다"고 표시한 지적 |
| BLOCKED / NOT_RUN | 실행되지 못함 / 실행하지 않음 |

### 1. 범위와 측정 조건

| 항목 | 값 |
|---|---|
| 대상 | 본 저장소 커밋 `553d82b`(측정 기준) → `9aea59e`(결함 수정 반영, 코드 커밋 `08da2c5`, `cb0cefa`, `9aea59e`). 버전 표기 1.13.0 변경 없음 |
| 측정 원장 보관 | 사양 저장소 커밋 `315782e` |
| 측정 장비 | 개발용 PC 1대(Windows 11 Pro), CPython 3.13.14, Clang 19.1.5 |
| 결과 표기 | PASS / FAIL / BLOCKED / NOT_RUN. 모든 수치는 격리된 임시 저장소와 임시 데이터에서 얻은 합성 측정이며, 실운영 측정은 0건입니다 |
| 원격 CI | 실행 0건. 모든 커밋에 `[skip ci]`를 붙였고, 게시 후 실행 목록으로 0건을 확인했습니다 |

### 2. 소유자 본인 확인 1회

- 소유자가 직접 연 콘솔에서 Windows Hello 본인 확인 검사를 1회 실행했고, 결과는 **verified = true**입니다. 이 검사는 아무것도 바꾸지 않습니다.
- 이후 모든 단계는 사람의 입력 없이 진행했습니다.

### 3. 정량 실측 (측정 기준 커밋 / 최종 커밋)

| 항목 | 측정 기준 | 최종 |
|---|---|---|
| 전수 회귀 시험 6,888건 | 통과 6,792 / 실패 39 / 오류 40 / 건너뜀 17, 신규 회귀 0, 누락 0, 핵심 191/191 | 통과 6,792 / 실패 39 / 오류 40 / 건너뜀 17, 신규 회귀 0, 누락 0, 핵심 191/191, 측정 기준 실행과 모든 시험의 상태 동일. 같은 커밋의 첫 실행은 통과 6,791 / 실패 39 / 오류 41 / 건너뜀 17, 핵심 190/191, 신규 회귀 1이었습니다: 연결 준비 시험 1건이 하위 프로세스 종료 대기 10 s를 넘겨 오류(가드 코드를 쓰지 않는 시험, 단독 실행 5회 모두 통과). 두 실행 모두 원장에 보관 |
| 자체 시험 디렉터리 | 1,138건 실행, 통과(건너뜀 20) | 1,167건 실행, 통과(건너뜀 20; 새 시험 29건 포함) |
| 가드 판정 1회(순수 계산, 100,000회) | 중앙값 0.0042 ms, p95 0.0107 ms, p99 0.0164 ms(예산 0.4 ms) | 중앙값 0.016 ms, p95 0.040 ms, p99 0.055 ms(수정 후 증가, 예산 0.4 ms 이내) |
| 명령 왕복(엔진 1회 처리와 영수증, 100회) | 중앙값 0.451 ms, p95 0.845 ms, p99 1.004 ms | 중앙값 0.203 ms, p95 0.297 ms, p99 0.455 ms |
| 엔진 메모리(세션 중) | 전용 메모리 815,104 B(상한 15 MiB 이내) | 전용 메모리 831,488 B(상한 15 MiB 이내) |
| 가드 측정 1회(본 저장소 복제본, 파일 1,977개) | 1.32 – 1.51 s | 1.52 – 1.65 s(파일 1,978개) |
| SHA-256 처리량 | 2,431.7 MiB/s | 2,654.2 MiB/s |
| 자체 검증 계측기: 헤더 탑재 검사 / 허용 목록 검사 | 52/52 일치, 통과(2.46 s) | 52/52 일치, 통과(2.24 s) |

- 실패 39건과 오류 40건은 모두 면제 집합 81건 안에 있습니다. 모든 시험이 통과한 상태가 아닙니다.

### 4. 신종 공격 변수와 AI 모델 세 개의 합동 연쇄 공격

| 단계 | 수행 모델 | 표적 | 변수 | 미탐 | 오탐 |
|---|---|---|---:|---:|---:|
| A 입구(계수기·경계 소모와 위장) | Claude Opus 5.5 | 슬롯 글, 반복 방지 계수, 살라미 창, 시계, 쓰기 잠금, id | 19 | 20 | 6 |
| B 환경(OS·셸 경계) | Claude Sonnet 5.5 | 프로세스 종료 정적 검사기, 명령 기록 판독, 무시 폴더, 링크, 대체 데이터 스트림 | 116 | 68 | 3 |
| C 핵심(원장·색인·데모 경계) | Claude Fable 5.2 | 원장·증인·격리, Git 색인 표시, 경로 철자 | 14 | 4 | 0 |
| C 보조 | 빌더(Claude Opus 5.5) | 비공개 원문 복사, 원장 되감기 | 9 | 3 | 0 |

- 변수 하나가 여러 사례로 실행되어 미탐을 둘 이상 낼 수 있습니다(Opus 변수 19개에서 미탐 20건).
- 한 저장소에서 네 단계를 잇는 연쇄 공격(쓰기 잠금 경합으로 차단 판정을 원장에서 지움 → 줄 없는 통과로 반복 방지 계수 초기화 → 처음부터 무시되던 폴더에 3,000줄 → 전각 글자로 비슷하게 만든 비공개 모듈 이름 아래 비공개 원문과 보이지 않는 슬롯 글)은 측정 기준 커밋에서 **네 단계 모두 통과**했고, 끝난 뒤 저장소는 경고 없는 상태였습니다.
- 최종 커밋에서는 첫 단계에서 멈춥니다. 잠금 경합 중 판정된 강경 사건이 격리 파일로 가서 저장소가 소유자 복구 전까지 읽을 수 없음 상태가 됩니다(실패 시 닫힘). 나머지 단계는 따로 실행해도 거부 또는 소유자 보류입니다.
- Fable의 첫 보고는 안전 분류기 중단으로 "실행하지 않음"이었으나, 같은 에이전트가 이어서 설계와 실행을 마쳤습니다. 두 보고를 모두 기록했습니다.

### 5. 고밀도 조합 시험 2,264건

| 수준 | 건수 | 측정 기준 미탐 / 오탐 | 최종 미탐 / 오탐 |
|---|---:|---|---|
| 순수 판정(슬롯 글 × 신호 × 위험 상태 × 증거 × 구역) | 1,061 | 15 / 0 | 0 / 0 |
| 명령 기록 판독(명령 × 래퍼·난독화, 두 항목 기록) | 384 | 227 / 5 | 0 / 3 |
| 프로세스 종료 정적 검사기(닫는 호출 × 핸들 철자 × 범위) | 243 | 204 / 0 | 54 / 0 |
| 데모 경계(비공개 대상 × 경로 철자 × 상태) | 336 | 36 / 0 | 0 / 0 |
| 실제 저장소(분할 × 인코딩 × 경합·강제 종료·시계 × 위치 × 크기) | 240 | 108 / 0 | 0 / 0 |
| 합계 | 2,264 | 590 / 5 | 54 / 3 |

- 모든 케이스는 축 2개 이상을 교차하며, 축 조합의 SHA-256으로 만든 케이스 id의 중복은 0건입니다.
- 두 커밋 모두 크래시 0, 영수증 유실 0, 고아 잠금 0입니다. 강제 종료는 원장 한 줄을 쓴 직후, 증인을 쓰기 전에 했고, 다음에 연 객체가 증인을 복구했습니다.
- 최종 미탐 54건은 모두 기록된 한계입니다: 실행 중 만든 사전에 담아 부르는 닫기 호출, 람다에 넘겨 부르는 닫기 호출, 이름 규칙에 따른 래퍼 1종. 최종 오탐 3건은 PowerShell 명령 안에 따옴표를 겹쳐 쓴 정상 작업의 기록으로, 해석이 모호해 거부되지만 위반 횟수에는 더하지 않습니다.
- 측정 방법 정정 두 가지를 두 커밋에 같게 적용했습니다: 거부된 명령 기록은 잡힘으로 셈, 강제 종료 전에 적대 행위에 닿지 못한 사례는 차단 기록 유실로 세지 않음. 하네스 결함 두 번(구문 오류, 가상 환경 실행기로 띄운 잠금 보유자)도 기록했습니다.

### 6. 보수 연구와 구현: 판정 차단 사항 수 (Opus / Sonnet / Fable)

| 회차 | 차단 사항 |
|---|---|
| 설계 2 → 9 | 14 / 8 / 8 → 6 / 2 / 2 → 2 / 0 / 1 → 1 / 1 / 1 → 1 / 1 / 1 → 1 / 1 / 2 → 1 / 0 / 0 → **0 / 0 / 0** |
| 구현 1 → 4 | 1 / 3 / 3 → 0 / 0 / 1 → 0 / 1 / 1 → **0 / 0 / 0** |
| 구현 5(자체 시험 디렉터리 결과 반영) | 0 / 0 / 1 → **0 / 0 / 0** (구현 6: 검사 표본을 시험이 읽는 데이터 파일로 옮김) |

- 순서: 설계 → 판정 → 수정 → 다시 판정, 차단 사항 0이 될 때까지. 고친 것: 슬롯 글의 닮은꼴·보이지 않는 글자·방향 제어 문자, 위치 칸의 경계 일치, 유한하지 않거나 앞선 시계, 잠금 경합 중 강경 사건 격리, 반복 방지 원인 키, 감사 범위의 창 확대, 해제 뒤 같은 우회를 다시 차단, id 검사, 명령 기록 판독(래퍼·난독화·치환·주석·긴 옵션 접두), 종료 호출의 다른 철자, 무시 폴더의 코드·중첩 저장소·링크, 하드 링크, 객체 저장소 곁 파일, 경로 철자 접기, 비공개 원문의 다른 파일 복사.
- 최종 커밋 뒤 자체 시험 디렉터리가 본 저장소 자신의 검사기로 실패 4건(원인 3가지)을 잡았습니다(시험 문자열이 프로세스 생성 검사에 걸림, 고정된 검사기 동작과 다른 판독, 데모 후보 모듈의 동적 가져오기). 검사기·고정값·허용 목록은 건드리지 않고 새 코드만 고쳤습니다.

### 7. 고치지 않은 것

- 소유자 결정이 필요한 2건: NTFS 대체 데이터 스트림 검사(새 운영체제 호출이 "허용된 프로세스 생성 위치" 목록의 새 행을 요구하며, 그 목록은 소유자만 바꿈), 같은 사용자의 원장·증인 일관 재기록(열쇠 있는 증인이 필요하며, 소유자 인증 경로가 아직 연결되지 않음). 두 항목의 미탐은 그대로 남습니다.
- 정적 분석의 한계로 기록: 다른 함수의 반환값이나 실행 중 만든 컨테이너·문자열로 운반된 호출과 명령, PATH로 찾는 이름만 있는 프로그램, 8.3 짧은 이름, 건너뛰는 빌드 폴더와 무시된 비코드 파일.

### 8. 기여 구분

- 가드 커널 자신: 빌더의 변경을 격리된 시험 사본에서 감사해 모든 가드 파일과 가드 시험 파일을 소유자 구역으로, 변경된 기존 시험과 시험 문자열의 표시어를 경고로, 새 기동기 이름을 프로세스 생성 위험으로 표시했습니다(강경 결함 0). 정적 검사기·자체 검증 계측기·전수 회귀 실행기·자체 시험 디렉터리가 회귀를 판정했습니다. 기록된 공격 패턴 32종은 커널 형식으로 기록만 했고, 커널이 스스로 패턴을 만들지는 않습니다.
- AI 모델 세 개: 공격 변수의 설계와 실행, 설계 판정 8회, 구현 판정 6회.
- 빌더: 시험 하네스, 조합 생성기, 연쇄 공격, 벤치, 보수 설계와 구현, 기록. 빌더 자신의 결함(이식 중 함수 2개 누락, 잠금 교착, 시험 기대값 오류)도 판정 전에 찾아 고쳤습니다.

### 9. 이 성적표가 주장하지 않는 것

- 모든 시험이 통과했다는 주장이 아닙니다(면제 집합 81건).
- 사람이나 외부 기관의 감사가 아닙니다. 공격·판정 모델과 수정 모델이 같은 회사의 모델입니다.
- 실운영 데이터, 원격 CI, 실제 장시간 운용은 측정하지 않았습니다(NOT_RUN).

### 10. 검증 방법

- 이 저장소의 `SHA256SUMS_v1.2.5.txt`로 이번 릴리즈 파일의 SHA-256을 확인할 수 있습니다.
- 증거 JSON(`evidence/09-2026-11-26-joint-chain.json`)에 위 수치의 원값이 있습니다.

---

## English

### 0. Terms

| Term | Meaning |
|---|---|
| main repository / specification repository | the private repository Ultra-Brain with the product source / the private repository Ultra-Brain-Legacy with the specifications and judgment records |
| Owner | the private individual who makes the final decisions for this project |
| guard kernel | the main repository's own checking layer that measures each development act and judges it pass, observe, hold, block or seal |
| slots | the fields an act must fill: target, content, place, reason, method and an evidence hash |
| command record | the list of shell commands the actor ran; written by the actor, so it may leave things out, and the guard only adds signals from it |
| ledger / witness / quarantine | the guard's hash-chained record / a separate file holding the chain's end / a file of hard events that could not be written (when it is not empty the whole repository reads as unreadable) |
| salami window | the limit on lines changed since the last audit (twice the step budget) |
| process-termination static checker | the checker that finds process and job termination calls in the source; it judges by the union of two detectors |
| Demo boundary | the rule that separates private modules and their text from what may go into the public derivative repository |
| full regression / core tests / exemption set | the 6,888 tests / the 191 of them that must pass / the 81 failures and errors the Owner exempted with recorded reasons in earlier releases |
| native test directory | the main repository's test suite for its C and native layers (separate from the 6,888) |
| self-verification instrument | the tool that checks, among other things, that mounted headers equal the specification repository's canon |
| miss / false positive | an attack that passed / legitimate work that was stopped |
| blocking finding | a finding a judging model marked as "must not proceed as is" |
| BLOCKED / NOT_RUN | could not run / was not run |

### 1. Scope and conditions

| Item | Value |
|---|---|
| Subject | main repository commit `553d82b` (measurement baseline) → `9aea59e` (fixes; code commits `08da2c5`, `cb0cefa` and `9aea59e`). Version label 1.13.0 unchanged |
| Measurement ledger | specification repository commit `315782e` |
| Machine | one development PC (Windows 11 Pro), CPython 3.13.14, Clang 19.1.5 |
| Result labels | PASS / FAIL / BLOCKED / NOT_RUN. Every figure is a synthetic measurement on isolated temporary repositories and data; production measurements: 0 |
| Remote CI | 0 runs. Every commit carries `[skip ci]`, and the run list was checked after publishing: 0 runs |

### 2. One identity check by the Owner

- The Owner ran the Windows Hello identity check once, from a console the Owner opened; the result is **verified = true**. The check changes nothing.
- Every later step ran without human input.

### 3. Measurements (baseline commit / final commit)

| Item | Baseline | Final |
|---|---|---|
| Full regression, 6,888 tests | 6,792 pass / 39 fail / 40 error / 17 skipped, 0 new regressions, 0 missing, core 191/191 | 6,792 pass / 39 fail / 40 error / 17 skipped, 0 new regressions, 0 missing, core 191/191; every test's status identical to the baseline run. The first run on the same commit was 6,791 pass / 39 fail / 41 error / 17 skipped, core 190/191, 1 new regression: a connection readiness test waited more than 10 s for a child process to exit (the test does not use the guard code; it passed 5 of 5 runs alone). Both runs are kept in the ledger |
| Native test directory | 1,138 run, OK (20 skipped) | 1,167 run, OK (20 skipped; includes 29 new tests) |
| One guard judgment (pure computation, 100,000 calls) | median 0.0042 ms, p95 0.0107 ms, p99 0.0164 ms (budget 0.4 ms) | median 0.016 ms, p95 0.040 ms, p99 0.055 ms (higher after the fixes; within the 0.4 ms budget) |
| Command round trip (one engine cycle and its receipt, 100 calls) | median 0.451 ms, p95 0.845 ms, p99 1.004 ms | median 0.203 ms, p95 0.297 ms, p99 0.455 ms |
| Engine memory (during a session) | private 815,104 B (within the 15 MiB ceiling) | private 831,488 B (within the 15 MiB ceiling) |
| One guard measurement (clone of the main repository, 1,977 files) | 1.32 – 1.51 s | 1.52 – 1.65 s (1,978 files) |
| SHA-256 throughput | 2,431.7 MiB/s | 2,654.2 MiB/s |
| Self-verification instrument: header mount check / allow-list check | 52/52 equal, PASS (2.46 s) | 52/52 equal, PASS (2.24 s) |

- The 39 failures and 40 errors are all inside the 81-case exemption set. This is not an all-pass state.

### 4. New attack variables and a joint chain attack by three AI models

| Stage | Model | Targets | Variables | Misses | False positives |
|---|---|---|---:|---:|---:|
| A entry (spend and disguise the counters and boundaries) | Claude Opus 5.5 | slot text, anti-loop counters, salami window, clock, writer lock, ids | 19 | 20 | 6 |
| B environment (OS and shell boundary) | Claude Sonnet 5.5 | process-termination static checker, command-record reader, ignored folders, links, alternate data streams | 116 | 68 | 3 |
| C core (ledger, index, Demo boundary) | Claude Fable 5.2 | ledger, witness, quarantine, Git index flags, path spellings | 14 | 4 | 0 |
| C supplement | the builder (Claude Opus 5.5) | copies of private text, ledger rollback | 9 | 3 | 0 |

- One variable can run as several cases and give more than one miss (Opus: 19 variables, 20 misses).
- A chain of four steps on one repository (a block erased from the ledger by writer-lock contention → the anti-loop counter reset by a pass with no lines → 3,000 lines in a folder ignored from the start → private text under a look-alike private module name made with a full-width letter, with invisible slot text) **passed all four steps** on the baseline commit, and the repository showed no warning afterwards.
- On the final commit the chain stops at the first step: the hard event judged under lock contention goes to the quarantine, and the repository reads as unreadable until the Owner's recovery (fail-closed). Run on their own, the other steps are refused or held for the Owner.
- Fable's first report was "not run" after a safety classifier stop; the same agent then finished the design and the run. Both reports are recorded.

### 5. Dense combination test, 2,264 cases

| Level | Cases | Baseline misses / false positives | Final misses / false positives |
|---|---:|---|---|
| pure judgment (slot text × signal × risk state × evidence × zone) | 1,061 | 15 / 0 | 0 / 0 |
| command-record reader (command × wrapper or obfuscation, two-entry records) | 384 | 227 / 5 | 0 / 3 |
| process-termination static checker (closing call × handle spelling × scope) | 243 | 204 / 0 | 54 / 0 |
| Demo boundary (private target × path spelling × state) | 336 | 36 / 0 | 0 / 0 |
| real repositories (split × encoding × contention, forced kill or clock × place × size) | 240 | 108 / 0 | 0 / 0 |
| total | 2,264 | 590 / 5 | 54 / 3 |

- Every case crosses at least two axes; the case ids (SHA-256 of the axis combination) have 0 duplicates.
- On both commits: 0 crashes, 0 lost receipts, 0 orphan locks. The forced kill came right after one ledger line was written and before the witness; the next object repaired the witness.
- The 54 final misses are all recorded limits: a closing call kept in a dictionary built at run time, a closing call handed to a lambda, and one wrapper by the naming rule. The 3 final false positives are records of legitimate work with nested double quotes inside a PowerShell command; they are ambiguous and refused, but not counted as a violation.
- Two measurement corrections were applied to both commits alike: a refused command record counts as caught, and a case whose hostile act was never reached before the forced kill is not counted as a lost block record. Two harness faults (a syntax error, and lock holders started through the virtual-environment launcher) are recorded as well.

### 6. Repair research and implementation: blocking findings (Opus / Sonnet / Fable)

| Rounds | Blocking findings |
|---|---|
| design 2 → 9 | 14 / 8 / 8 → 6 / 2 / 2 → 2 / 0 / 1 → 1 / 1 / 1 → 1 / 1 / 1 → 1 / 1 / 2 → 1 / 0 / 0 → **0 / 0 / 0** |
| implementation 1 → 4 | 1 / 3 / 3 → 0 / 0 / 1 → 0 / 1 / 1 → **0 / 0 / 0** |
| implementation 5 (after the native test directory) | 0 / 0 / 1 → **0 / 0 / 0** (implementation 6: the scanner samples moved to a data file the test reads) |

- Order: design → judgment → fix → judgment again, until 0 blocking findings. Fixed: look-alike, invisible and direction-control characters in slot text, bounded matching of the place slot, a clock that is not finite or runs ahead, quarantine of hard events under lock contention, the anti-loop cause key, an audit that covers the whole window, the same bypass blocked again after a hold is lifted, id checks, the command-record reader (wrappers, obfuscation, substitutions, comments, long-option prefixes), other spellings of termination calls, code, nested repositories and links in ignored folders, hard links, the object store's side files, folded path spellings, and copies of private text into other files.
- After the final commit the native test directory caught 4 failures from 3 causes with the main repository's own checkers (test strings that the process-creation scan read as launches, a reading that differed from a pinned checker behaviour, a dynamic import in a Demo candidate module). Only the new code was changed; no checker, pinned value or allow-list was touched.

### 7. What was not fixed

- Two items need the Owner's decision: the NTFS alternate data stream check (a new operating-system call needs a new row in the "allowed process-creation places" list, which only the Owner changes), and a consistent rewrite of the ledger and witness by the same user (needs a keyed witness; the Owner's identity route is not connected yet). Their misses remain.
- Recorded as limits of static analysis: calls and commands carried through another function's return value or a container or string built at run time, programs named without a path and found by PATH, 8.3 short names, the skipped build folders and ignored non-code files.

### 8. Who contributed what

- The guard kernel itself: it audited the builder's change in an isolated test copy and marked every guard file and guard test file as an Owner zone, flagged changed existing tests and marker words in test strings, and flagged the new launcher names as a process-creation risk (0 hard faults). The static checker, the self-verification instrument, the full-regression runner and the native test directory judged the regressions. The 32 recorded attack patterns were written in the kernel's format; the kernel did not create patterns by itself.
- The three AI models: design and execution of the attack variables, 8 design judgments, 6 implementation judgments.
- The builder: the test harness, the combination generator, the chain attack, the benchmarks, the repair design and implementation, the records. The builder's own faults (two functions dropped during a splice, a lock deadlock, wrong test expectations) were found and fixed before judgment.

### 9. What this scorecard does not claim

- It does not claim that every test passes (exemption set of 81).
- It is not an audit by people or an outside body. The attacking, judging and fixing models are models of the same company.
- Production data, remote CI and long-running operation were not measured (NOT_RUN).

### 10. How to verify

- `SHA256SUMS_v1.2.5.txt` in this repository lists the SHA-256 of this release's files.
- The evidence JSON (`evidence/09-2026-11-26-joint-chain.json`) holds the raw values of the figures above.
