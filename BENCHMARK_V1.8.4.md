# #7 닫힌 어휘 게이트·조기 종결·진입점 게이트 — 실측 성적표 (Ultra Brain v1.8.4 · Legacy v1.69)

본체 Ultra Brain **v1.8.4**(코드 `549f049`, 릴리즈 `44469ae`)와 사령부 Ultra-Brain-Legacy **v1.69**(`f375aac`)의 성적표입니다. 소스는 싣지 않습니다. 기계가 읽는 원자료는 [evidence/07-v184-v169-benchmark.json](evidence/07-v184-v169-benchmark.json), 판정은 [EVALUATION_STAGE8_V184.md](EVALUATION_STAGE8_V184.md), 직전 성적표는 [BENCHMARK_V1.8.3.md](BENCHMARK_V1.8.3.md)입니다.

## 읽기 전에

1. **이 문서는 "무엇을 바꿨고 무엇이 측정됐는가"만 적습니다.** 결함이 실제로 해소됐는지의 판정은 구현자가 아니라 9차 판정이 합니다.
2. **모든 수치에 출처가 있습니다.** CI 값은 GitHub Actions `windows-latest`가 해당 커밋에서 새로 빌드해 실행한 결과입니다. 로컬 개발기에서는 Smart App Control이 빌드의 실행을 막아 엔진 위의 수치는 전부 CI의 것입니다.
3. **"올그린"이 아닙니다.** 전수 회귀의 81건 FAIL/ERROR는 역사적 오너 면제 집합입니다. 그리고 v1.8.4의 CI는 처음에 통과하지 못했습니다(§2).
4. **어떤 공인 기관의 인증도 아닙니다.** 구현(Claude Fable 5.2)과 감정(Claude Opus 5.5, Claude Sonnet 5.5)은 같은 회사의 모델입니다.
5. 오너 지시문의 표현 중 측정·코드와 다른 것은 §8에 정정했습니다.

## 1. 전수 회귀

같은 6,888개 케이스 ID, 같은 인터프리터(기준 CPython 3.13, `ef8f5102…`)이며 한 번 돌렸습니다.

| 버전 | 커밋 | 결과 | 신규 회귀 | 창 (UTC, 2026-11-13) |
|---|---|---|---|---|
| v1.8.4 | `549f049` | **6,790 PASS / 41 FAIL / 40 ERROR / 17 SKIP** | 0건 | 14:19:23 ~ 14:56:18 |

- 누락 0, 실행 중 소스 변경 0, 지정 핵심 191/191입니다.
- 81건의 FAIL/ERROR는 역사적 오너 면제 집합입니다. raw ALL PASS가 아닙니다.
- 6,888건은 사령탑과 메타섹터 이전의 케이스 ID입니다. 그 시험들은 이 안에 없고 CI의 단위 시험에 들어 있습니다.
- v1.8.1에서 확률적으로 실패했던 기존 시험(`9443` 부분 문자열 검사)은 이 실행에서 나타나지 않았습니다. 오너 지시로 고치지 않았으므로 다시 나올 수 있습니다.

## 2. CI — 본체

**첫 실행에서 통과하지 못했습니다.** 다섯 번 실패한 뒤 코드 커밋에서 세 번, 릴리즈 커밋에서 한 번 통과했습니다.

| run | 커밋 | 결과 | 원인 |
|---|---|---|---|
| 43440450462 | `96eeb9a` | 실패 | 계약 자가 시험은 실제 프로세스에서 통과했으나, 러너 구동기의 기대 문구에 v1.8.3의 코퍼스 수와 다이제스트가 남아 있었다 |
| 43440712688 | `6e1a1f1` | 실패 | 도크 명령줄 게이트는 설계대로 동작했으나, 게이트가 낸 정상 종료 코드 1이 워크플로 단계를 끝냈다 |
| 43440786434 · 43440803099 · 43440821115 | `53114a7` | 실패 | 같은 원인 |
| 43441112272 · 43441553065 · 43441571676 | `549f049` | **success** | — |
| 43448400861 | `44469ae` (main) | **success** | — |

통과한 네 번의 값은 다음과 같습니다.

| 항목 | v1.8.3 | v1.8.4 |
|---|---|---|
| 실프로세스 판정 | 11/11 PASS | **11/11 PASS** (엔진 러너 9 + 계약 러너 2) |
| 단위 테스트 | 265/265 PASS (181 + 84) | **282/282 PASS** (198 + 84), +17 |
| Clang 20.1.8 `-std=c11 -O2 -Wall -Wextra -pedantic -Werror` | 경고 0, 러너 284,160 B | 경고 0, 엔진 러너 **241,664 B**, 계약 러너 225,280 B (한도 각 307,200 B) |
| MSVC `/utf-8 /W4 /WX` | 경고 0 | 경고 0 (두 러너) |
| 자가 시험 | 18/18 (한 바이너리) | 엔진 **16/16**, 계약 **2/2** (두 컴파일러) |
| 메타섹터 자가 시험 | 골든 벡터 81개, 등록 집합 24건, 코퍼스 2,368건, 다이제스트 `0xDAE9EE5E` | 골든 벡터 **184개**, 등록 집합 24건, 어휘 위생 검사, 코퍼스 **3,520건**, 다이제스트 `0xFC83EB60` |
| 빌드 의존 헤더 | 25개 | 엔진 23개, 계약 25개 |
| P6 기준 영수증 | SHA-256 `b53b511e…` 불변 | 불변 |
| 사이클 경로 | MAIN 헤더 8개가 v1.7.91 바이트와 동일. 사이클당 28 프레임 | 같음 |

- **계약 러너 분리.** 사령탑과 메타섹터의 자가 시험을 엔진 이미지에서 떼어 별도 실행 파일로 옮겼습니다. 엔진 러너는 두 계약 헤더를 포함하지 않으며 크기가 v1.7.91과 같아졌습니다.
- 코퍼스 다이제스트는 C 구현과 Python 호스트가 같은 3,520개 명령에서 각각 계산한 값이 같다는 뜻입니다. 두 구현이 다르게 판정하는 명령이 **그 코퍼스 안에는** 없습니다. 코퍼스 밖은 이 값이 말해 주지 않습니다.
- 골든 벡터 184개 가운데 앞 47개는 §4.1의 43건과 접속어를 뺀 4문장이고(모두 오너 대기), 다음 33개는 등록 단어만 쓴 정식 지휘입니다(모두 통과).

## 3. 상주 데몬 soak (30초, 15 MiB Job Object)

| 버전 | run | Job 프로세스 최대 | MiB | 사이클 |
|---|---|---|---|---|
| v1.8.4 | 43441112272 | 1,290,240 B | 1.23 | 5,937 |
| v1.8.4 | 43441553065 | **1,318,912 B** | **1.26** | 4,269 |
| v1.8.4 | 43441571676 | 1,224,704 B | 1.17 | 4,601 |
| v1.8.4 | 43448400861 | **1,339,392 B** | **1.28** | 4,178 |

- 엔진 유휴 CPU는 네 번 모두 **0.0초**입니다.
- 15 MiB 한도(15,728,640 B)의 7.8 ~ 8.5 %입니다.
- **사령부 사양서의 "1.25 MiB 이하"(1,310,720 B)는 지켜지지 않았습니다.** 네 표본 중 둘이 8,192 B와 28,672 B 넘었습니다. v1.8.2와 v1.8.3의 여덟 표본은 모두 안이었습니다([BENCHMARK_V1.8.3.md §3](BENCHMARK_V1.8.3.md)).
- **원인은 측정하지 않았습니다.** v1.8.4의 엔진 이미지는 v1.8.3보다 42,496 B 작고 메타섹터 코드가 들어 있지 않습니다. 구현자는 자가 시험을 떼면 최대치가 내려갈 것이라고 예상했으나 틀렸습니다. 최대치는 이미지 크기를 따르지 않습니다.
- 같은 바이너리가 실행마다 수만 B씩 다릅니다. 이 네 번의 범위는 114,688 B입니다.
- 사령부 v1.69는 이 사실을 사양서 36.4절에 "지켜지지 않았다"로 기록했고, 수치를 어떻게 할지는 오너 결정으로 남겼습니다.

## 4. 게이트 실측

### 4.1 8차 판정 반례 43건 — 전수 기록

**43건이 무엇인가.** 8차 판정 §2.1이 "오너 없이 엔진에 닿았다"고 실행 확인한 입력은 41건입니다. 여기에 구현자가 v1.8.3 노트에 한계로 공개했고 8차가 확인한 2건(`read f-o-r-m-a-t`, `read status obliterate disk`)을 더한 것이 43건입니다. 8차 원문에 "43"이라는 수는 없습니다. 구현자가 센 수입니다.

**측정 방법.** 두 태그(`v1.8.3`, `v1.8.4`)의 소스를 각각 꺼내, 오너 게이트를 설정하지 않은 사령탑에 43건을 넣었습니다. 엔진은 그 태그의 단위 시험이 쓰는 대역입니다. "사이클 실행"은 오너 없이 한 사이클이 돌았다는 뜻입니다. 실엔진 위의 확인은 CI가 했습니다(이 절 끝).

| 유형 | 건수 | v1.8.3: 오너 없이 사이클 실행 | v1.8.4: 오너 없이 사이클 실행 | v1.8.4: 오너 대기 |
|---|---|---|---|---|
| 레이더 단어의 변형 | 11 | 11 | **0** | 11 |
| 단어표 밖의 말 | 13 | 13 | **0** | 13 |
| 단독 EXECUTE·WRITE 절 | 9 | 9 | **0** | 9 |
| 접속어로 약해지는 판정 | 4 | 4 | **0** | 4 |
| 등록되지 않은 접속 표현 | 3 | 3 | **0** | 3 |
| 숨긴 표기 | 3 | 3 | **0** | 3 |
| **합계** | **43** | **43** | **0** | **43** |

| # | 유형 | 지휘문 | v1.8.3 | v1.8.4 | v1.8.4 게이트 위험도 | v1.8.4 사유 |
|---|---|---|---|---|---|---|
| 1 | 레이더 단어의 변형 | `read status reformat disk` | 사이클 실행 (위험도 0) | **오너 대기** | 3 | RISK_3, UNREGISTERED_WORD, ROOT, RISK_AT_OWNER_THRESHOLD · 어근 format |
| 2 | 레이더 단어의 변형 | `read status shutdown_now` | 사이클 실행 (위험도 0) | **오너 대기** | 3 | RISK_3, UNREGISTERED_WORD, ROOT, RISK_AT_OWNER_THRESHOLD · 어근 shutdown |
| 3 | 레이더 단어의 변형 | `read status power_off` | 사이클 실행 (위험도 0) | **오너 대기** | 3 | RISK_3, UNREGISTERED_WORD, ROOT, RISK_AT_OWNER_THRESHOLD · 어근 poweroff |
| 4 | 레이더 단어의 변형 | `read status wipeall` | 사이클 실행 (위험도 0) | **오너 대기** | 3 | RISK_3, UNREGISTERED_WORD, ROOT, RISK_AT_OWNER_THRESHOLD · 어근 wipe |
| 5 | 레이더 단어의 변형 | `read status wipeout disk` | 사이클 실행 (위험도 0) | **오너 대기** | 3 | RISK_3, UNREGISTERED_WORD, ROOT, RISK_AT_OWNER_THRESHOLD · 어근 wipe |
| 6 | 레이더 단어의 변형 | `read status format_c` | 사이클 실행 (위험도 0) | **오너 대기** | 3 | RISK_3, UNREGISTERED_WORD, ROOT, RISK_AT_OWNER_THRESHOLD · 어근 format |
| 7 | 레이더 단어의 변형 | `read status rm_rf` | 사이클 실행 (위험도 0) | **오너 대기** | 3 | RISK_3, UNREGISTERED_WORD, ROOT, RISK_AT_OWNER_THRESHOLD · 어근 rm |
| 8 | 레이더 단어의 변형 | `read status re-boot` | 사이클 실행 (위험도 0) | **오너 대기** | 3 | RISK_3, UNREGISTERED_WORD, ROOT, RISK_AT_OWNER_THRESHOLD · 어근 reboot |
| 9 | 레이더 단어의 변형 | `read status shut-down` | 사이클 실행 (위험도 0) | **오너 대기** | 3 | RISK_3, UNREGISTERED_WORD, ROOT, RISK_AT_OWNER_THRESHOLD · 어근 shutdown |
| 10 | 레이더 단어의 변형 | `read status init0` | 사이클 실행 (위험도 0) | **오너 대기** | 3 | RISK_3, UNREGISTERED_WORD, ROOT, RISK_AT_OWNER_THRESHOLD · 어근 init |
| 11 | 레이더 단어의 변형 | `read status formatting disk` | 사이클 실행 (위험도 0) | **오너 대기** | 3 | RISK_3, UNREGISTERED_WORD, ROOT, RISK_AT_OWNER_THRESHOLD · 어근 format |
| 12 | 단어표 밖의 말 | `read status destroy volume` | 사이클 실행 (위험도 0) | **오너 대기** | 3 | RISK_3, UNREGISTERED_WORD, ROOT, RISK_AT_OWNER_THRESHOLD · 어근 destroy |
| 13 | 단어표 밖의 말 | `read status nuke home` | 사이클 실행 (위험도 0) | **오너 대기** | 3 | RISK_3, UNREGISTERED_WORD, ROOT, RISK_AT_OWNER_THRESHOLD · 어근 nuke |
| 14 | 단어표 밖의 말 | `read status overwrite mbr` | 사이클 실행 (위험도 0) | **오너 대기** | 3 | RISK_3, UNREGISTERED_WORD, ROOT, RISK_AT_OWNER_THRESHOLD · 어근 overwrite |
| 15 | 단어표 밖의 말 | `read status zero disk` | 사이클 실행 (위험도 0) | **오너 대기** | 2 | UNREGISTERED_WORD, RISK_AT_OWNER_THRESHOLD |
| 16 | 단어표 밖의 말 | `read status uninstall windows` | 사이클 실행 (위험도 0) | **오너 대기** | 3 | RISK_3, UNREGISTERED_WORD, ROOT, RISK_AT_OWNER_THRESHOLD · 어근 uninstall |
| 17 | 단어표 밖의 말 | `read status unlink everything` | 사이클 실행 (위험도 0) | **오너 대기** | 3 | RISK_3, UNREGISTERED_WORD, ROOT, RISK_AT_OWNER_THRESHOLD · 어근 unlink |
| 18 | 단어표 밖의 말 | `read status clear everything` | 사이클 실행 (위험도 0) | **오너 대기** | 2 | UNREGISTERED_WORD, RISK_AT_OWNER_THRESHOLD |
| 19 | 단어표 밖의 말 | `read status reset pc` | 사이클 실행 (위험도 0) | **오너 대기** | 3 | RISK_3, UNREGISTERED_WORD, ROOT, RISK_AT_OWNER_THRESHOLD · 어근 reset |
| 20 | 단어표 밖의 말 | `read status regedit` | 사이클 실행 (위험도 0) | **오너 대기** | 2 | UNREGISTERED_WORD, RISK_AT_OWNER_THRESHOLD |
| 21 | 단어표 밖의 말 | `read status powershell` | 사이클 실행 (위험도 0) | **오너 대기** | 2 | UNREGISTERED_WORD, RISK_AT_OWNER_THRESHOLD |
| 22 | 단어표 밖의 말 | `read status sudo` | 사이클 실행 (위험도 0) | **오너 대기** | 2 | UNREGISTERED_WORD, RISK_AT_OWNER_THRESHOLD |
| 23 | 단어표 밖의 말 | `read status chmod` | 사이클 실행 (위험도 0) | **오너 대기** | 2 | UNREGISTERED_WORD, RISK_AT_OWNER_THRESHOLD |
| 24 | 단어표 밖의 말 | `read windows system 32` | 사이클 실행 (위험도 1) | **오너 대기** | 3 | RISK_3, UNREGISTERED_WORD, ROOT, RISK_AT_OWNER_THRESHOLD · 어근 system32 |
| 25 | 단독 EXECUTE·WRITE 절 | `restart computer` | 사이클 실행 (위험도 2) | **오너 대기** | 2 | UNREGISTERED_WORD, RISK_AT_OWNER_THRESHOLD |
| 26 | 단독 EXECUTE·WRITE 절 | `stop service` | 사이클 실행 (위험도 2) | **오너 대기** | 2 | UNREGISTERED_WORD, RISK_AT_OWNER_THRESHOLD |
| 27 | 단독 EXECUTE·WRITE 절 | `run payload` | 사이클 실행 (위험도 2) | **오너 대기** | 2 | UNREGISTERED_WORD, RISK_AT_OWNER_THRESHOLD |
| 28 | 단독 EXECUTE·WRITE 절 | `exec payload` | 사이클 실행 (위험도 2) | **오너 대기** | 2 | UNREGISTERED_WORD, RISK_AT_OWNER_THRESHOLD |
| 29 | 단독 EXECUTE·WRITE 절 | `start installer` | 사이클 실행 (위험도 2) | **오너 대기** | 2 | UNREGISTERED_WORD, RISK_AT_OWNER_THRESHOLD |
| 30 | 단독 EXECUTE·WRITE 절 | `set registry` | 사이클 실행 (위험도 2) | **오너 대기** | 3 | RISK_3, RISK_AT_OWNER_THRESHOLD |
| 31 | 단독 EXECUTE·WRITE 절 | `put owner key` | 사이클 실행 (위험도 2) | **오너 대기** | 3 | RISK_3, RISK_AT_OWNER_THRESHOLD |
| 32 | 단독 EXECUTE·WRITE 절 | `save secrets public` | 사이클 실행 (위험도 2) | **오너 대기** | 3 | RISK_3, UNREGISTERED_WORD, RISK_AT_OWNER_THRESHOLD |
| 33 | 단독 EXECUTE·WRITE 절 | `write ledger` | 사이클 실행 (위험도 1) | **오너 대기** | 3 | RISK_3, RISK_AT_OWNER_THRESHOLD |
| 34 | 접속어로 약해지는 판정 | `read status and run payload` | 사이클 실행 (위험도 2) | **오너 대기** | 2 | UNREGISTERED_WORD, RISK_AT_OWNER_THRESHOLD, COMPOUND_NOT_ALL_READ |
| 35 | 접속어로 약해지는 판정 | `read status and restart computer` | 사이클 실행 (위험도 2) | **오너 대기** | 2 | UNREGISTERED_WORD, RISK_AT_OWNER_THRESHOLD, COMPOUND_NOT_ALL_READ |
| 36 | 접속어로 약해지는 판정 | `show ledger then stop service` | 사이클 실행 (위험도 2) | **오너 대기** | 2 | UNREGISTERED_WORD, RISK_AT_OWNER_THRESHOLD, COMPOUND_NOT_ALL_READ |
| 37 | 접속어로 약해지는 판정 | `read status, exec payload` | 사이클 실행 (위험도 2) | **오너 대기** | 2 | UNREGISTERED_WORD, RISK_AT_OWNER_THRESHOLD, COMPOUND_NOT_ALL_READ |
| 38 | 등록되지 않은 접속 표현 | `read status or frobnicate x` | 사이클 실행 (위험도 0) | **오너 대기** | 2 | UNREGISTERED_WORD, RISK_AT_OWNER_THRESHOLD |
| 39 | 등록되지 않은 접속 표현 | `read status before frobnicate x` | 사이클 실행 (위험도 0) | **오너 대기** | 2 | UNREGISTERED_WORD, RISK_AT_OWNER_THRESHOLD |
| 40 | 등록되지 않은 접속 표현 | `read status while reset disk` | 사이클 실행 (위험도 0) | **오너 대기** | 3 | RISK_3, UNREGISTERED_WORD, ROOT, RISK_AT_OWNER_THRESHOLD · 어근 reset |
| 41 | 숨긴 표기 | `read status base64 Zm9ybWF0IGM6` | 사이클 실행 (위험도 0) | **오너 대기** | 2 | UNREGISTERED_WORD, RISK_AT_OWNER_THRESHOLD |
| 42 | 숨긴 표기 | `read f-o-r-m-a-t` | 사이클 실행 (위험도 0) | **오너 대기** | 3 | RISK_3, UNREGISTERED_WORD, ROOT, RISK_AT_OWNER_THRESHOLD · 어근 format |
| 43 | 숨긴 표기 | `read status obliterate disk` | 사이클 실행 (위험도 0) | **오너 대기** | 2 | UNREGISTERED_WORD, RISK_AT_OWNER_THRESHOLD |

- 위험도 열은 게이트의 판정입니다. 엔진이 보는 첫 동사의 위험도는 바꾸지 않았습니다.
- 접속어를 뺀 4문장(`read status run payload` 등)은 v1.8.3에서도 대기했고 v1.8.4에서도 대기합니다.
- **살아 있음 확인.** 등록 단어만 쓴 정식 지휘 10건(`read status`, `show ledger`, `read status and show ledger`, `write note`, `read ledger 2` 등)은 두 버전 모두 오너 없이 통과합니다. 게이트가 모든 것을 막는 상수는 아닙니다.
- **실엔진(CI run 43448400861).** `read status reformat disk`, `read status and format c:`, `check and wipe disk`, `show everything then erase disk`, `read status format c:`, `delete system now`가 모두 오너 대기, 사이클 0, 엔진 미기동이었습니다(§4.3). `read status`와 `read status and show ledger`는 한 사이클로 보고됐습니다.
- **이 43건은 구현자가 재현한 뒤 골든 벡터에 넣은 입력입니다.** 43/43은 "그 입력들이 이제 대기한다"는 측정이고, 다른 입력에 대한 증거가 아닙니다.

### 4.2 무엇이 막는가

v1.8.4의 게이트는 네 가지를 겹쳐 둡니다. 설계 단계에서 규칙을 하나씩만 켜고 43건을 돌렸습니다(구현 전 시제품, 사양서 §6).

| 켠 규칙 | 43건 중 막은 수 |
|---|---|
| 파괴 어근 레이더만 (어근 43개) | 20 |
| 복합 규칙만 | 4 |
| 닫힌 어휘 + 위험도 행렬(문턱 2) | **43** |
| 전부 | 43 |

- **차단의 근거는 어휘입니다.** 레이더는 표지이자 2차 방어선입니다. 목록은 본질적으로 불완전합니다.
- **닫힌 어휘.** 절의 첫 단어는 고정 동사표의 동사, 나머지는 등록 명사 68개(공개 39·상태 14·보호 15)·조사류 15개·숫자·버전 표기·목적어 자리의 READ 동사여야 합니다. 모르는 단어는 오너 대기입니다.
- **위험도 행렬과 문턱.** 위험도는 [동사 분류][절에서 가장 넓은 명사 범위]에서 나옵니다. 문턱은 2입니다(v1.8.3까지 3).

| 동사 분류 | 공개 명사 | 상태 명사 | 보호 명사 |
|---|---|---|---|
| READ | 0 | 0 | 1 |
| WRITE | 1 | 2 | 3 |
| EXECUTE | 2 | 2 | 3 |
| DELETE | 3 | 3 | 3 |

- **READ는 보호 명사에도 통과합니다.** `read secrets`, `show password`는 위험도 1로 오너 없이 통과합니다. 구현자의 설계 선택이었고 9차 감정이 결함으로 지적했습니다. 엔진 사이클은 실제로 아무것도 읽어 오지 않습니다(§6).
- **WRITE는 명사가 "공개" 범위이면 통과합니다.** 구현자는 영수증·골든·다이제스트·해시·체인·봉인·헤더·엔진·러너·데몬·도크를 공개 범위에 넣었습니다. 그래서 `write receipt`, `update golden headers`, `set engine`이 위험도 1로 통과합니다. 9차 감정은 이것을 "상태 쓰기는 오너"(D1)가 지켜지지 않는 빈틈으로 판정했습니다.
- **전수 조합(구현자 재현, 두 감정의 수와 일치).** 등록 동사 35개 × 등록 명사 68개 = 2,380건 가운데 **856건이 오너 없이 통과**합니다: READ 544건(그중 보호 명사 120건), WRITE × 공개 명사 312건. DELETE·EXECUTE·ESCALATE는 0건입니다.
- **버전 표기는 점으로 이은 숫자를 모두 받습니다.** `set engine 10.0.0.1`이 통과합니다. 길이와 단 수 제한이 없습니다.
- **어근 2계층.** 긴 어근 35개는 이음 문자(`_` `-` `.`)를 뺀 단어 안이나 인접한 두 단어에 걸친 부분 문자열로 찾습니다. 짧은 어근 8개(`dd` `del` `rd` `rm` `reg` `init` `reset` `drop`)는 글자열·숫자열 전체가 일치할 때만 찾습니다. 본체 저장소의 영어 단어 12,149개에서 `rd`는 202개, `rm`은 172개, `dd`는 102개(동사 `add` 포함) 안에 들어 있기 때문입니다.
- **복합 규칙.** 절이 둘 이상이면 모든 절이 READ일 때만 통과합니다. `write note`는 통과하고 `read status and write note`는 대기합니다.
- **비교 수치.** 위험한 절 하나가 든 복합 명령 3,456건을 만들어 넣으면 v1.8.1의 첫 동사 규칙은 2,919건(84.5 %)을 오너 없이 통과시키고 v1.8.4는 0건을 통과시킵니다. 이 3,456건은 구현자가 만든 코퍼스(위험 절 24종 × 접속 표현 16종 × 위치)이며 위험 절은 구현자가 골랐습니다. **범위의 증거가 아니라 자기 일관성의 증거입니다.**
- **187,486건.** 구현 전 **설계 시제품**의 속성 검사 건수입니다. 제품 코드의 검사가 아닙니다. 첫 실행에서 1건이 실패해 복합 규칙을 고쳤고, 두 번째 실행에서 0건이었습니다.

### 4.3 조기 종결

v1.8.3까지는 거부·대기 지휘에도 엔진을 띄워 인사(HELLO)와 종료(BYE)를 주고받았습니다. v1.8.4는 엔진이 생기기 전에 판정합니다. 아래는 CI의 출력입니다(run 43448400861).

```text
COMMANDER OWNER_WAIT error OWNER_REQUIRED dna11 78570505731 risk 0 cycles served 0
  engine not started: frames sent 0, received 0
  curriculum: highest risk 3 reasons ['RISK_3', 'UNREGISTERED_WORD', 'ROOT', 'RISK_AT_OWNER_THRESHOLD'] radar [] roots ['format']
    clause 1: risk 3 ['RISK_3', 'UNREGISTERED_WORD', 'ROOT', 'RISK_AT_OWNER_THRESHOLD'] 'read status reformat disk'
COMMANDER OWNER_WAIT error OWNER_REQUIRED dna11 6742142441 risk 0 cycles served 0
  engine not started: frames sent 0, received 0
  curriculum: highest risk 3 reasons ['VERB_UNKNOWN', 'RADAR', 'STRUCTURE', 'RISK_3', 'UNREGISTERED_WORD', 'ROOT', 'RISK_AT_OWNER_THRESHOLD', 'COMPOUND_NOT_ALL_READ'] radar ['format', 'drive root'] roots ['format']
    clause 1: risk 0 [] 'read status'
    clause 2: risk 3 ['VERB_UNKNOWN', 'RADAR', 'STRUCTURE', 'RISK_3', 'UNREGISTERED_WORD', 'ROOT', 'RISK_AT_OWNER_THRESHOLD'] 'format c:'
```

- 첫 줄의 `risk 0`은 엔진이 보는 첫 동사의 위험도입니다. 게이트의 판정은 `curriculum` 줄이고, 증적에는 `gate_risk`로도 실립니다(증적 스키마 `/4`).
- 대기 증적은 지휘문의 모든 절을 나열합니다. 보관 창은 32개이고(v1.8.3까지 8개), 그 뒤의 절은 창 함수로 읽어 함께 싣습니다.
- 통과한 지휘는 이전과 같이 엔진 세션을 열어 한 사이클(28 프레임)을 돕니다.

### 4.4 진입점

| 경로 | v1.8.3 | v1.8.4 |
|---|---|---|
| 사령탑(`si_commander`) | 게이트 | 게이트 + 조기 종결 |
| 도크 명령줄(`metabolism_dock --command`) | 게이트 없음 | 같은 게이트를 먼저 거침. 오너가 필요한 지휘는 엔진 미기동, 종료 코드 1 |
| 모니터 콘솔, 일반 송신 | 게이트 없음 | 같은 게이트를 먼저 거침. 오너가 필요한 지휘는 모든 절을 나열한 예/아니오 대화상자 뒤에 보냄 |
| 모니터 콘솔, "시험 처리" 버튼 | 게이트 없음 | **접수 거부만 적용.** 오너 대기 판정이어도 절 목록 대화상자를 건너뛰고 39자 미리보기 대화상자 한 번 뒤에 보냄 (9차 감정이 찾음) |
| 원시 세션 함수(`DockSession.command`·`cycle`, `metabolism_dock.run_command`) | 게이트 없음 | **게이트 없음** |
| 사령탑의 내부 사이클 함수를 직접 만든 디스패치로 부르는 것 | 게이트 없음 | **게이트 없음** (9차 감정이 찾음) |
| 상주 데몬의 데이터 채널 | 게이트 없음 | **게이트 없음** (4 KiB 바이트 조각이며 지휘문이 아님) |
| 같은 사용자의 다른 프로세스가 엔진 프로토콜을 직접 말하는 것 | 가능 | **가능** |

CI의 도크 명령줄 출력입니다(run 43448400861). 문법 밖의 문장을 넣었습니다.

```text
GATE OWNER_WAIT error OWNER_REQUIRED: no Owner gate is configured
  clause 1: risk 2 ['VERB_UNKNOWN', 'STRUCTURE', 'UNREGISTERED_WORD', 'RISK_AT_OWNER_THRESHOLD'] 'control tower test command: one metabolism cycle'
engine not started: frames sent 0, received 0
DOCK OWNER_WAIT
```

- **사양서의 A안과 다릅니다.** 설계 사양은 세션의 `command`가 게이트 판정 객체를 요구하도록 했습니다. 구현은 원시 함수를 그대로 두고, 사람이 친 지휘를 받는 두 진입점이 게이트를 먼저 부르게 했습니다. P6 기준 영수증과 엔진 시험이 임의 문장과 한국어 문장을 원시 함수로 보내기 때문입니다. 원시 함수를 부를 수 있는 제품 파일은 단위 시험이 고정합니다.
- **콘솔의 확인은 오너 인증이 아닙니다.** 대화상자이며 오너 토큰이 아닙니다.
- **시험 처리 경로는 구현자가 놓친 분기입니다.** `read status and show ledger and list receipts and format c:`를 시험 처리로 보내면 대화상자에는 `read status and show ledger and list re…`까지만 보입니다. 이 경로의 시험은 없습니다.
- **진입점 고정 시험은 약합니다.** 별칭 import, 다른 변수 이름, `getattr`, 데몬 함수 호출, 내부 사이클 함수 호출의 다섯 변형 가운데 하나도 잡지 못합니다.
- 오너 서명 도구가 서명 전에 지휘 원문을 보여 주는 것(사양서 4.8절)은 구현하지 않았습니다.

### 4.5 호스트 비용 (run마다 표본 1개, 새 프로세스의 첫 호출)

| 단계 | v1.8.3 (4 run) | v1.8.4 (4 run) |
|---|---|---|
| 수신+분해, `read status` | 290.6 ~ 721.9 µs | 197.0 ~ 492.0 µs |
| 수신+분해, 복합 명령 | 214.3 ~ 613.0 µs | 311.1 ~ 613.5 µs |
| 사이클 왕복, `read status` | 0.667 ms (1 run) | 0.534 ~ 1.153 ms |
| 사이클 왕복, 복합 명령 | — | 0.578 ~ 1.348 ms |

- run 사이의 편차가 버전 사이의 차이보다 큽니다. 닫힌 어휘 검사만의 비용은 분리해 재지 않았습니다.
- 엔진의 warm 왕복 중앙값 0.244 ms는 2026-11-11 코텍스 이전 빌드의 값입니다. v1.8.4 엔진은 **미측정**입니다.

### 4.6 기존 지휘의 동작 변화

| 지휘문 | v1.8.3 | v1.8.4 | 이유 |
|---|---|---|---|
| `run build`, `start engine`, `stop daemon` | 통과 | 오너 대기 | EXECUTE는 위험도 2, 문턱이 2가 됨 |
| `write memory`, `set policy` | 통과 | 오너 대기 | 상태 명사에 쓰기는 위험도 2 |
| `read ledger-2` | 통과 | 오너 대기 | `ledger-2`는 등록되지 않은 단어. `read ledger 2`는 통과 |
| `show information` | 통과 | 오너 대기 | 등록되지 않은 단어. `information` 안의 `format`에 어근 표지도 붙음 |
| `read status and write note` | 통과 | 오너 대기 | 복합 규칙. `write note` 단독은 통과 |
| `read status?`, `please read status` | 오너 대기 | 오너 대기 | 대화체는 지휘 문법이 아님. 오너 결정(2026-11-13)으로 정상 사양 |

닫힌 어휘는 대기하는 무해한 지휘를 늘립니다. 실제 지휘 중 얼마가 대기하는지는 **측정하지 않았습니다.**

## 5. 8차 판정의 결함 13건에 대해 바꾼 것

| # | 8차 판정 | v1.8.4 |
|---|---|---|
| 1 | 오너 키 대칭성과 C의 정수 nonce | 바꾸지 않음 |
| 2 | 사령탑을 거치지 않는 진입점 | 도크 명령줄과 모니터 콘솔이 게이트를 먼저 거침. 원시 함수와 상주 데몬의 데이터 채널은 그대로(§4.4) |
| 3 | 어휘의 빈틈과 접속어로 인한 완화 | 닫힌 어휘, 위험도 행렬, 문턱 2, 2계층 어근 레이더, 복합 규칙(§4.2) |
| 4 | 꾸민 증명 산출물로 PROVEN 도달 | 바꾸지 않음 |
| 5 | 토큰 원장의 삭제·동시 사용 | 바꾸지 않음 |
| 6 | R0 증명기 오판 | 바꾸지 않음 |
| 7 | 보고 증거 위조 | 바꾸지 않음 |
| 8 | 경로 별칭과 보호 목록 누락 | 바꾸지 않음 |
| 9 | 오너가 보는 것과 서명하는 것의 차이 | 대기 증적이 모든 절을 나열함. 오너 서명 도구와 리팩토링 주제 결속은 바꾸지 않음 |
| 10 | 보고·학습의 위험도가 최고 위험도가 아니다 | 증적에 `gate_risk`를 추가함. `risk`와 학습기의 칸 선택은 첫 동사 그대로 |
| 11 | 접수 이전에 엔진이 로드된다 | 조기 종결(§4.3) |
| 12 | 과잉 대기와 거부 | 줄지 않고 늘었음(§4.6). 대화체 대기는 오너 결정으로 정상 사양이 됨. 오탐률 미측정 |
| 13 | 시험의 공백 (DPAPI 경로 0건, 로컬 실엔진 NOT_RUN) | 바꾸지 않음 |

이 릴리즈가 겨냥한 것은 2·3·9·10·11번입니다. 승인과 증명 쪽(1·4·5·6·7·8번)은 `si_owner.py`와 `si_refactor.py`가 v1.8.1 이후 그대로입니다.

## 6. 구현자가 릴리즈 노트에 적은 한계

- **의미를 이해하지 않습니다.** 어휘 안에서 게이트가 아는 것은 행렬의 칸뿐입니다. `write note`는 note가 무엇이든 통과합니다.
- **어휘가 작습니다.** 어휘에 없는 말은 모두 대기합니다. 실제 지휘 중 대기하는 비율은 미측정입니다.
- **긴 어근은 무해한 단어에도 표지를 답니다.** `information` 안의 `format`이 그렇습니다. 그런 단어는 등록되지 않은 단어로 이미 대기합니다.
- **한국어 단어가 든 절은 대기합니다.** 한국어 단어는 등록돼 있지 않고 어미로 이은 절은 나누지 못합니다. 릴리즈 노트는 "한국어 지휘는 항상 대기한다"고 적었는데 틀렸습니다. 한글이 등록 접속어뿐이면 통과합니다(`read status 그리고 show ledger`). 8차가 이미 지적한 문장을 다시 쓴 것입니다.
- **원시 함수에는 게이트가 없습니다.** 같은 프로세스의 코드는 `DockSession.command`를 부를 수 있고, 같은 사용자의 다른 프로세스는 엔진을 띄워 프로토콜을 직접 말할 수 있습니다.
- **콘솔의 확인은 대화상자입니다.** 오너 토큰이 아닙니다.
- **엔진 사이클은 외부 행동을 하지 않습니다.** 게이트가 정하는 것은 어떤 문장이 사이클·영수증·학습기 갱신을 받는가입니다. 안전상 가치는 앞으로 만들 실행기가 이 게이트를 거친 입력만 받는지에 달려 있고, 그 실행기는 없습니다.
- **승인·증명 쪽 결함은 열려 있습니다**(§5의 1·4·5·6·7·8번).
- **탑재 헤더의 주석 불일치.** `c_seed_curriculum.h` 머리 주석의 "Limits" 단락에 v1.8.2의 문장(`read status obliterate disk` passes)이 남아 있어 코드와 다릅니다. 코드는 그 입력을 대기시킵니다(§4.1의 43번).

## 7. 사령부 도킹 (Legacy v1.69)과 Labs

| 항목 | 값 | 근거 |
|---|---|---|
| 계약 헤더 | `c_seed_curriculum.h`(blob `474cb59d…`)와 `c_seed_si_commander.h`(blob `c6e1dad3…`)가 **본체 v1.8.4**와 동일 | 두 저장소의 태그 트리 |
| 러너 / 메시 | **26/26** (플래그 19종 모두 exit 0) / **33/33** | Legacy run [43452571685](https://github.com/CSY8515/Ultra-Brain-Legacy/actions/runs/43452571685) (태그), [43452226365](https://github.com/CSY8515/Ultra-Brain-Legacy/actions/runs/43452226365) (main), [43451026772](https://github.com/CSY8515/Ultra-Brain-Legacy/actions/runs/43451026772) (발행 전 검증 브랜치). 세 번 모두 첫 실행에서 통과 |
| 컴파일러 | Clang 20.1.8·MSVC 경고 0. 러너 297,984 B, 메시 294,912 B | 같은 run |
| 메타섹터 자가 시험 | 3,520건, 다이제스트 `0xFC83EB60`, 어근 35 + 8개, 보관 절 32개. 등록 집합 24건 포함 | 같은 run |
| 제97조·불변식 85호 | v1.68에서 제정, 본체 v1.8.3에서 구현, **v1.69에서 탑재**. 사령부 CI가 "절 분해 이전에 명령 전체 거부"를 판정함 | [v1.69 릴리즈](https://github.com/CSY8515/Ultra-Brain-Legacy/releases/tag/v1.69), 사양서 36.2절 |
| 불변식 84호 "실측 1.25MiB 이하" | 본체 v1.8.4의 네 표본 중 둘에서 **지켜지지 않음**으로 기록. 조문은 고치지 않았고 오너 결정으로 남김 | 사양서 36.4절 |
| 새 조문·새 불변식 | 없음. 앞 절의 원문은 고치지 않고 정정표(36.4절)로 수록 | 사양서 제36절 |
| Labs v1.1.0 | 변경 없음 | [BENCHMARK_V1.8.0.md §1](BENCHMARK_V1.8.0.md) |

사령부 러너는 본체와 달리 엔진 검사와 계약 검사를 한 바이너리에 그대로 둡니다. 사령부 CI에는 크기 한도 판정이 없습니다.

## 8. 오너 지시문 표현 대조

| 지시문 | 측정과 코드 | 처리 |
|---|---|---|
| "8차에서 지적된 43건 반례" | 8차 §2.1의 실행 확인 입력은 41건입니다. 43건은 구현자가 한계로 공개한 2건을 더해 센 수이고, 8차 원문에 "43"은 없습니다 | §4.1에 출처를 밝히고 43건 전부를 수록 |
| "미등록 동사/목적어 공격" | 43건은 여섯 유형입니다. 미등록 단어는 그중 하나이고, 단독 EXECUTE·WRITE 절 9건과 접속어 완화 4건은 등록된 동사의 문제였습니다 | 유형별로 수록 |
| "닫힌 어휘(명사 68개)" | 맞습니다. 공개 39·상태 14·보호 15개이고, 조사류 15개와 동사표가 더 있습니다 | 그대로 수록 |
| "완전 차단(Established)" | 43/43이 대기한다는 것은 측정입니다. "완전"은 구현자가 주장하지 않습니다(§4.1 끝, §6) | 판정은 9차에 맡김 |
| "D1 오너 문턱(Risk 2 실행/상태쓰기 오너 확인 강제)" | 문턱 2와 실행 동사 대기는 맞습니다. 쓰기는 명사가 공개 범위이면 위험도 1로 통과하고(312건), READ는 보호 명사에도 위험도 1로 통과합니다(120건) | "실행은 항상 대기, 쓰기는 명사 분류에 따라"로 표기(§4.2) |
| "D3(32개 절 전수 가시화)" | 32는 보관 창의 크기입니다. 대기 증적은 32개를 넘는 절도 포함해 모든 절을 싣습니다 | "모든 절 나열, 창 32개"로 표기 |
| "D4(dock·관제탑 진입점 사령탑 철통 게이트 일원화)" | 도크 명령줄과 모니터 콘솔이 게이트를 먼저 거칩니다. 원시 함수, 상주 데몬의 데이터 채널, 다른 프로세스는 게이트 밖입니다. 콘솔의 확인은 대화상자이고 "시험 처리" 버튼은 절 목록을 건너뜁니다. 사양서의 일원화 안(A안)은 구현하지 않았습니다. 그리고 본체 코드에서 "관제탑"은 Connection 관제탑이라는 다른 구성 요소입니다. 게이트가 붙은 화면은 대사 모니터 콘솔입니다 | "도크 명령줄과 모니터 콘솔 일반 송신에 게이트"로 표기(§4.4) |
| "오너 주권이 완전 성립" | 오너 토큰의 대칭 키, 꾸민 증명 산출물, 원장 삭제는 바꾸지 않았습니다(§5) | 판정은 9차에 맡김 |
| "반례 0개의 완전 무결격 결정론적 보안 제어 커널 … 공인" | 판정이 결정론적인 것은 맞습니다(같은 입력에 같은 판정, 부동소수점 없음). "반례 0개"와 "무결격"은 구현자가 주장하지 않습니다. 이 문서는 어떤 기관의 공인도 아닙니다 | 판정은 9차에 맡김 |
| "4대 모델 교차 평가 … Fable 5.1" | 이 작업 환경에서 실행할 수 있는 모델은 Opus 5.5, Sonnet 5.5였습니다. Fable 5.1은 실행할 수 없어 **NOT_RUN**입니다. 다른 모델의 출력을 Fable 5.1의 것으로 싣지 않았습니다 | Fable 5.2(구현), Opus 5.5, Sonnet 5.5 세 모델 수록 |
| "1.25 MiB" (사령부 불변식 84호) | v1.8.4에서 네 표본 중 둘이 넘었습니다 | §3에 그대로 수록 |

## 9. 검증 해시·식별자

| 대상 | 식별자 |
|---|---|
| 본체 v1.8.4 코드 / 릴리즈 커밋 | `549f049a1a899c1fe46e4ebb2ef303c8d62c8858` / `44469aeb19ea0d651f641abfc98193c5decd7d92` |
| 전수 회귀 결과 SHA-256 | `e8cc2349f7ad48062f35d1a71ae9bb703ffbe60412d2c40ccf6927d72d480df6` |
| 본체 기준 영수증 파일 SHA-256 | `b53b511ed937b75cb83d63e37ace33b5ab7afbfbbddfbf73f35d5cab575149dc` |
| 메타섹터 계약 헤더 (v1.8.4, 사령부 탑재본) | Git blob `474cb59dcaef2744313ee9b9547fa216dcfb3a31`, 74,276 B, SHA-256 `b7235ca02902c2350865ee2e20be5cf4a6d0e7e99691b1bf727157a2ffe29596` |
| 사령탑 계약 헤더 (v1.8.4, 사령부 탑재본) | Git blob `c6e1dad3a1d077ac862421e761256fc34af5a0c0`, 61,401 B, SHA-256 `0e11a0400b66665af9c13a367cfb03a2df0115e39c47cb30be101a9532c9b324` |
| 어휘 목록 SHA-256 (호스트 고정값) | `c33a701ac581087ac7a7f670ae28a8cb353b23d7abfebff0a23f86f37c8914b4` |
| 사령부 v1.69 태그 커밋 | `f375aac0aad9b5483cac47a8c9a0a557d699f5b5`, [릴리즈](https://github.com/CSY8515/Ultra-Brain-Legacy/releases/tag/v1.69) |
| Labs v1.1.0 태그 커밋 | `a69a0cd3368475b56e28fd9492cf2b351232dcf1`, [릴리즈](https://github.com/CSY8515/Ultra-Brain-Labs/releases/tag/v1.1.0) |

본체 저장소는 비공개라 run 링크는 권한이 있어야 열립니다. 사령부와 Labs의 run은 공개입니다.

## 10. 공개하지 않는 것

- 본체의 소스 코드, 테스트 코드, 빌드 스크립트.
- 오너 승인 경로, 봉인 파일, 원장 내용, 키 파일, 토큰, 설치 EXE.

---

Claude는 Anthropic의 제품입니다. 이 문서는 Ultra Brain 프로젝트의 독립적인 기록이며, 어떤 공인 기관의 인증도 아닙니다.
