# 측정 성적표와 AI 모델 적대적 검토 — 2026-11-25 / Measured Scorecard and AI-Model Adversarial Review — 2026-11-25

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
| 본 저장소 | 제품 소스를 담은 비공개 저장소 Ultra-Brain |
| 사양 저장소 | 사양서와 판정 기록을 담은 비공개 저장소 Ultra-Brain-Legacy |
| 소유자(Owner) | 이 프로젝트의 최종 결정권자인 개인 사용자 |
| 핵심 시험 | 전수 회귀 시험 가운데 반드시 통과해야 하는 지정 시험 191건 |
| 기준 실행 | 측정 기준 커밋은 직전 변경(커밋 `0fd4b0f`, 2026-11-24)의 전수 회귀 실행과, 최종 커밋은 측정 기준 커밋의 실행과 비교. 케이스별 결과 기록을 보관 |
| 신규 회귀 / 누락 | 기준 실행에서 통과했는데 이번에 실패하거나 오류가 난 시험 / 계획했으나 결과가 기록되지 않은 시험 |
| 엔진 | 명령 1건을 한 주기로 처리하고 결과 영수증을 돌려주는 C 실행 파일(별도 프로세스) |
| 코어 DLL | 고정 크기 작업 슬롯과 무결성 검사를 제공하는 C 라이브러리 |
| 서브모듈 | 엔진 곁에서 각자 별도 프로세스로 도는 C 프로그램 4종(감시, 감사, 도구, 전송) |
| 계약 러너 | 사양이 정한 규약의 자체 시험을 실행하는 C 프로그램 |
| C 전용 파이프 왕복 계측기 | 파이프로 64 B 프레임 하나를 보내고 확인 응답을 받기까지의 시간을 C 코드만으로 재는 프로그램 |
| 명령 왕복 | 명령 1건을 엔진에 보내 결과 영수증을 받기까지의 시간 |
| 유출 검출기 | 비공개 파일(소유자 계정, 복구 키, 자격 증명, 데이터베이스)이 공개용 파생 저장소로 들어가는 경로를 막는 검사 |
| 승인 토큰 | 소유자가 서명한 일회용 승인 표. 위험한 행위 하나에만 쓸 수 있음 |
| 계층(tier) | 공격의 세 단계: 1차 정면, 2차 내부 침투 가정, 3차 AI 에이전트 부정행위와 퍼징 |
| 면제 집합 | 이전 릴리즈에서 소유자가 사유를 기록하고 면제한 실패·오류 시험 81건 |
| 상위 중앙값 | 표본을 정렬했을 때 가운데 두 값 중 큰 값 |
| 지연 예산 | 사양이 정한 상한값. 파이프 왕복의 예산은 38 µs |
| 자체 검증 계측기 | 시험과 검사를 한 항목씩 실행하고 기록하는 프로젝트 자체 도구(내부 이름 "drydock") |
| 백신 시험 | 공격·스트레스에서 쓴 패턴을 일부러 다시 주입해 계속 차단되는지 확인하는 회귀 시험 |
| 데모(Demo) | 공개용 파생 저장소 Ultra-Brain-Demo. 현재 비어 있고, 무엇을 실을지는 소유자가 따로 결정 |
| 돌파 / 부분 / 차단 | 공격이 보호 장치를 넘음 / 소스가 이미 밝힌 잔여 위험을 재현했거나 일부만 성공 / 보호 장치가 막음 |
| BLOCKED | 시험·공격이 실행되지 못한 상태(위의 "차단"과 다름) |
| 차단 사항(판정) | 판정 모델이 "이대로 진행하면 안 된다"고 표시한 지적 |
| 변이 시험 | 코드를 의도적으로 변형한 사본에서 시험이 그 변형을 잡는지 확인하는 방법 |

### 1. 범위와 측정 조건

| 항목 | 값 |
|---|---|
| 대상 | 본 저장소 커밋 `083af88`(측정 기준) → `2d08614`(결함 수정 반영). 버전 표기 1.13.0 변경 없음 |
| 측정 원장 보관 | 사양 저장소 커밋 `bd138ec` |
| 측정 장비 | 개발용 PC 1대(Windows 11 Pro, 논리 프로세서 14개), CPython 3.13.14, Clang 19.1.5 |
| 측정 시각 | 2026-11-25 09:44 – 13:43 UTC |
| 결과 표기 | PASS / FAIL / BLOCKED / NOT_RUN. 모든 수치는 격리 사본과 임시 데이터에서 얻은 합성 측정이며, 실운영 측정은 0건입니다 |
| 원격 CI | 실행 0건. 소유자 규칙에 따라 무료 사용량을 지키기 위해 2026-12-01 전에는 원격 실행을 하지 않습니다 |

### 2. 소유자 본인 확인 1회

- 소유자가 직접 연 콘솔에서 Windows Hello 본인 확인 검사를 1회 실행했고, 결과는 **verified = true**(소요 4.2 s)입니다.
- 이 검사는 아무것도 바꾸지 않습니다. 도구가 소유자 대신 띄운 프로세스는 설계상 거부됩니다.

### 3. 정량 실측

| 항목 | 반복 | 결과 |
|---|---|---|
| 전수 회귀 시험 6,888건(측정 기준 커밋) | 1회 | 통과 6,792 / 실패 39 / 오류 40 / 건너뜀 17. 신규 회귀 0, 누락 0, 핵심 시험 191/191. 케이스별 결과가 기준 실행과 6,888건 모두 동일. 소요 35분 14초 |
| 전수 회귀 시험 6,888건(최종 커밋) | 1회 | 통과 6,792 / 실패 39 / 오류 40 / 건너뜀 17. 신규 회귀 0, 누락 0, 핵심 시험 191/191. 케이스별 결과가 측정 기준 실행과 6,888건 모두 동일. 소요 35분 4초 |
| 명령 왕복(엔진 1회 처리와 영수증) | 100회 | 중앙값 0.205 ms, p90 0.288 ms, p99 0.902 ms, 최대 1.359 ms |
| 엔진 응답 확인(ping) | 500회 | 중앙값 0.026 ms, p99 0.104 ms |
| 엔진 세션 시작(프로세스 생성과 연결) | 20회 | 중앙값 57.2 ms, p99 98.6 ms |
| 엔진 메모리(세션 중) | 1회 | 전용 메모리 819,200 B, 최대 작업 집합 3,624,960 B. 상한 15 MiB(15,728,640 B) 이내 |
| C 전용 파이프 왕복 계측기(136표본) | 5회 | 상위 중앙값 16.5 / 26.3 / 21.6 / 25.7 / 20.1 µs. 5회 모두 지연 예산 38 µs 이내 |
| Python 파이프 왕복(64 B) | 10,000회 | 중앙값 5.3 µs, p99 7.9 µs |
| 호출 경로 정적 분석기: 지점별 보고 / 파일 트리 전체 보고 | 각 20회 | 중앙값 126.7 ms / 2,157 ms |
| C 헤더 평가 순서 검사(헤더 54개) | 10회 | 중앙값 640.6 ms, 위반 0 |
| 자체 검증 계측기, 빠르게 끝나는 검사 3개 | 검사당 3회 | 0.97 – 12.87 s(PASS). 같은 목록의 검사 1개는 NOT_RUN(계측기 자체 기록의 사유: 이 버전에서 실행 명령이 연결되지 않음. 증거 JSON에 원문 수록) |
| 빌드(Clang) | 1회 | 코어 DLL 0.55 s, 엔진 1.34 s, 계약 러너 1.36 s, 서브모듈 일체 12.34 s |
| 바이너리 크기 | — | 엔진 256,000 B, 계약 러너 264,192 B, 코어 DLL 118,272 B, 서브모듈 82,944 – 116,736 B |
| 코어 DLL 처리량 | — | NOT_RUN. Windows Smart App Control이 다시 빌드한 실행 파일(DLL)을 차단했고, 제품 규칙이 같은 빌드의 재시도를 금지합니다(우회하지 않음) |

- 실패 39건과 오류 40건은 모두 면제 집합 81건 안에 있습니다. 모든 시험이 통과한 상태가 아닙니다.

### 4. AI 모델 세 개의 적대적 공격

| 계층 | 수행 모델 | 공격 수 | 돌파 | 부분 | 차단 |
|---|---|---:|---:|---:|---:|
| 1차 정면(명령줄, 파이프, 본인 확인, 영수증 위조, 경로 탈출) | Claude Opus 5.5 | 23 | 0 | 8 | 15 |
| 2차 내부 침투 가정 | Claude Sonnet 5.5 | 0 | — | — | — |
| 3차 AI 에이전트 부정행위와 공개 퍼징 패턴 | Claude Fable 5.2 | 12 | 3 | 4 | 5 |

- 2차 계층은 실행되지 못했습니다(BLOCKED). 해당 모델의 안전 분류기가 착수 전에 작업을 멈췄고, 같은 요청을 다른 표현으로 다시 보내지 않았습니다. 그 계층의 표적(위조한 승인 토큰으로 승인 경로에 들어가기, 엔진 응답 위조, 회귀 시험 도구 변조)은 검증되지 않은 채 남습니다.
- 1차의 부분 8건 가운데 다수는 소스 코드가 이미 잔여 위험으로 밝힌 항목을 실측으로 다시 확인한 것입니다.
- 3차의 돌파 3건은 한 가지 결함입니다. 자체 검증 계측기가 작업 트리의 변경 여부를 `git status` 하나로만 판단해서, Git 색인 표시(assume-unchanged, skip-worktree)와 로컬 제외 목록으로 숨긴 변경을 보지 못했습니다.

### 5. 발견하고 고친 결함 4건

| 번호 | 결함 | 발견 | 수정 |
|---|---|---|---|
| 1 | 실행 영수증 원장을 잠금 없이 열어서, 다른 프로세스가 동시에 기록하면 손상되지 않은 원장을 "손상"으로 거부함 | 6절 스트레스(288건 중 17건) | 열 때도 원장 잠금을 잡음. 재실행 288/288 통과 |
| 2 | 공개 파생 저장소 유출 검출기가 마침표 등 구두점을 넣어 쓴 저장소 이름을 놓침 | 1차 공격 | 이전 검출 규칙을 그대로 두고 더 넓은 규칙을 OR로 추가함(이전보다 느슨해질 수 없음). 호환 문자와 결합 문자 포함 |
| 3 | 같은 검출기가 `public_release/` 같은 폴더와 경로 맨 앞의 `public/`을 놓침 | 1차 공격 | 경로 조각 안의 단어 단위로 판정 |
| 4 | 자체 검증 계측기가 숨긴 변경을 보지 못함 | 3차 공격 | 숨김 표시와 제외 목록을 별도 줄로 기록해 "깨끗하지 않음"으로 판정. 희소 체크아웃 예외는 사양 저장소에 속한 검사에만 적용 |

- 순서: 설계 → 판정 모델 세 개의 설계 판정 → 사양 저장소에 먼저 기록 → 본 저장소 구현 → 판정 모델 세 개의 구현 판정.
- 설계 판정 회차별 차단 사항 수(Opus / Sonnet / Fable): 1회차 1 / 0 / 1, 2회차 1 / 2 / 1, 3회차 0 / 0 / 0.
- 구현 판정: 1회차 차단 사항은 Opus 1 / Sonnet 0 / Fable 1입니다. 데모 분류 기록에서 시험 대상이 비공개 모듈에 닿는 백신 1종이 후보로 표시되어 있었고, 백신 분류가 측정되지 않았습니다. 수정 후 2회차 차단 사항은 0 / 0 / 0입니다.
  - 판정 모델들은 변이 시험으로 시험의 검출력을 확인했고, 잡지 못한 변형이 나올 때마다 시험을 보강했습니다.
  - 1회차 커밋 하나에서는 새 시험이 띄우는 자식 프로세스 2개를 "허용된 프로세스 생성 위치" 목록에 등록하지 않아, 그 목록을 검사하는 시험 1건이 실패했습니다. 다음 커밋에서 등록했습니다.

### 6. 조합 스트레스와 강제 종료 시험 1,732건

| 기준 | 결과 |
|---|---|
| 조합 | 경계 크기(0 B, 1 B, 63/64/65 B, 4 KiB ± 1, 64 KiB ± 1), 제어 문자, NUL, 잘못된 UTF-8, 양방향 제어 문자, 분할 전송, 동시 기록 프로세스 1 – 8개, 처리 중 강제 종료 후 재시작 |
| 네이티브 크래시 / 경계 밖 쓰기 수용 / 강제 종료 후 복구 실패 | 0 / 0 / 0 |
| 확인된 영수증 유실 / 강제 종료로 손상된 원장 / 남은 잠금 | 0 / 0 / 0 |
| 남은 파이프 이름 / 끊긴 상대를 감지하지 못함 | 0 / 0 |
| 메모리·핸들 추세 | 반복 횟수에 비례하는 증가 없음. 200회·500회 반복에서 처음에 한 번 늘고(전용 메모리 약 1.9 MB, 핸들 최대 7개) 이후 평탄 |
| 거짓 무결성 경보 | 17건 → 결함 1로 수정, 재실행 0건 |
| 라이브러리 수준 관찰 | 길이가 64의 배수이면서 64 B를 넘는 메시지(분할 전송의 절반 포함)는, 첫 읽기 오류 뒤에도 계속 읽으면 메시지의 마지막 64 B가 정상 프레임으로 읽힘(180건). 제품의 모든 읽기 지점은 첫 오류에서 해당 작업자를 끝내므로 제품 경로에서는 0건 |
| 판정 기준 정정 | 시험 도구의 기대값이 틀렸던 60건(아직 쓰지 않은 영역에서 0 B를 읽는 요청을 C 코드가 거부한 정상 동작)을 PASS로 다시 분류. 도구 결함이 있던 첫 두 실행의 기록도 비공개 원장에 보존 |

### 7. 백신 시험과 데모 분류

- 공격과 스트레스에서 쓴 패턴을 일부러 다시 주입해 계속 막히는지 확인하는 백신 시험 21건을 본 저장소에 추가했습니다(영수증 원장, 본인 확인, 파이프, 유출 검출기, 데모 경계, 숨긴 변경).
- 판정 모델들이 코드를 의도적으로 변형한 사본(변이 시험)으로 시험의 검출력을 확인했고, 잡지 못한 변형이 나올 때마다 시험을 보강했습니다.
- 데모 후보 분류: 사양 저장소의 공개 범위 문서가 정한 공개 가능 4묶음을 기준으로 모듈 사이의 import 관계를 측정했습니다. 그 결과 후보는 4개, "분리 전까지 비공개"는 3개입니다.
  - 후보: 프로젝트 규칙 로더(프로젝트별 정책 파일을 읽어 규칙을 좁히기만 함), 감사 부품(정지 명령 도달 시간, 파이프 지연, 호출 경로, 잠금, 분할 처리를 검사), 의도 슬롯(정해진 어휘로 명령의 의도를 표시), 실사격 판정표(실제 사용 시험의 기록을 등급별로 판정).
  - 분리 전까지 비공개: 승인 관문의 틀(위험한 행위마다 소유자 승인을 요구하는 구조), 평문 설명표(거부 사유 코드를 한 문장으로 설명), 클린룸 검사기(공개 후보에 비공개 내용이 섞였는지 검사하고 공개를 잠금). 이 셋은 import를 따라가면 비공개 모듈에 닿습니다.
- 공개 처리 기능(publish)은 잠긴 상태 그대로입니다. 이 분류는 공개 허가가 아닙니다.

### 8. 릴리즈 게이트 4종

| 게이트 | 내용 | 결과 |
|---|---|---|
| 1. 정적·비밀값 유출 검사 | 비밀키·토큰 패턴, 이메일, 로컬 절대 경로, IPv4, 기기 이름, 소유자 데이터베이스·키 파일 확장자, 공개 파생 유출 검출기 | 0건 |
| 2. 비공개 원문 교차 대조 | 두 비공개 저장소의 파일 경로·함수·클래스·매크로 이름과의 일치, 6줄 단위 블록 해시 일치, 파일 전체 일치 | 0건. 본 저장소 추적 파일 1,967개와 사양 저장소 추적 파일 3,534개에서 뽑은 이름 29,822개와 6줄 블록 445,769개를 대조했습니다. 사양 저장소는 체크아웃된 파일만 내용을 대조했습니다 |
| 3. 문구 감사 | 정규식 검사(구어체, 홍보 문구, 내부 약어)와 이 문서를 쓰지 않은 AI 모델의 독자 검토 | 정규식 검사 0건. 이 문서를 쓰지 않은 AI 모델(Claude Sonnet 5.5)의 독자 검토 5회에서 반드시 고칠 지적이 35 → 14 → 2 → 2 → 0건으로 줄었고, 모두 반영 |
| 4. SHA-256 목록 재계산 | `SHA256SUMS_v1.2.4.txt`의 값과 실제 파일을 다시 계산해 대조. 게시 이후의 변경을 알아낼 뿐, 수치가 옳다는 증명은 아님 | 6개 파일(성적표, 릴리즈 노트, 증거 JSON, README, CHANGELOG, VERSION), 100 % 일치. 목록 파일 자신은 제외 |

### 9. 이 성적표가 주장하지 않는 것

1. 보안이 완결되었다고 주장하지 않습니다. 남은 위험의 요지는 10절에 있습니다.
2. 다른 기계나 CI에서 얻은 값이 아닙니다.
3. 2차 계층 공격과 코어 DLL 처리량은 측정하지 못했습니다.
4. 공격과 판정을 맡은 모델은 모두 같은 회사(Anthropic)의 AI 모델이며, 사람이나 외부 기관의 검토가 아닙니다.

### 10. 기록된 잔여 위험(요지)

- 크기를 그대로 두고 수정 시각을 되돌린 편집은 자체 검증 계측기가 보지 못합니다. 파일 내용 해시 비교(1,965개 파일에 0.57 s로 측정)를 다음 변경으로 제안했습니다.
- 자기 자신을 무시하는 하위 `.gitignore`, 상속된 Git 환경 변수, 파일 감시 훅, 원본 없는 바이트코드 파일.
- 토큰 원장 파일을 이전 사본으로 되돌린 뒤 유효 시간(300 s) 안에 이미 쓴 승인 토큰을 다시 쓰는 경우. 소유자의 설계 결정 사항입니다.
- 유출 검출기: 퍼센트 인코딩, 8.3 짧은 이름, 모양이 비슷한 다른 문자, 여러 폴더로 나뉜 이름, 구분자 없는 `webpublic/`.

### 11. 검증 방법

- `SHA256SUMS_v1.2.4.txt`로 이 릴리즈의 여섯 파일(성적표, 릴리즈 노트, 증거 JSON, README, CHANGELOG, VERSION)을 다시 계산해 대조할 수 있습니다. 목록 파일은 자기 자신의 해시를 담지 않습니다.
- `evidence/08-2026-11-25-measured-review.json`에 2 – 8절의 수치, 측정 시각, 측정 명령 문자열의 SHA-256을 기계 판독 형식으로 실었습니다. 명령 원문에는 비공개 경로가 들어 있어 해시만 공개합니다.
- 비공개 측정 원장(사양 저장소 `bd138ec`)에는 보관 파일 131개의 SHA-256 목록과 측정과 판정 기록 57건을 한 줄씩 해시 사슬로 이은 원장이 있습니다(사슬 끝 값 `ae4105ca…9bf9`).

---

## English

### 0. Terms

| Term | Meaning |
|---|---|
| main repository | the private repository Ultra-Brain that holds the product source |
| specification repository | the private repository Ultra-Brain-Legacy that holds specifications and judgment records |
| Owner | the individual user who makes the final decisions of this project |
| designated core | 191 designated tests within the full regression that must pass |
| baseline run | the measured commit is compared with the full regression run of the previous change (commit `0fd4b0f`, 2026-11-24), the final commit with the measured commit's run; per-test results are kept |
| new regression / missing | a test that passed in the baseline run and now fails or errors / a planned test with no recorded result |
| engine | a C executable (a separate process) that handles one command per cycle and returns a result receipt |
| core DLL | a C library that provides fixed-size work slots and integrity checks |
| sub-modules | four C programs that each run as a separate process beside the engine (sentinel, auditor, toolbox, transport) |
| contract runner | a C program that runs the self-tests of the contracts the specification defines |
| C-only pipe round-trip meter | a program, in C only, that times sending one 64 B frame over the pipe and receiving its acknowledgement |
| command round trip | the time from sending one command to the engine to receiving its result receipt |
| leak detector | a check that blocks private files (Owner account, recovery key, credentials, databases) from entering the public derivative repository |
| approval token | a single-use approval ticket signed by the Owner; it can be used for one risky action only |
| tier | the three stages of the attack: 1 front door, 2 assume breach, 3 AI-agent cheating and fuzzing |
| exempt set | 81 failing or erroring tests that the Owner exempted, with recorded reasons, in earlier releases |
| upper median | the larger of the two middle values of the sorted samples |
| latency budget | the ceiling the specification sets; 38 µs for the pipe round trip |
| self-verification instrument | the project's own tool that runs and records tests and checks one item at a time (internal name "drydock") |
| vaccine test | a regression test that injects an attack or stress pattern again on purpose and checks that it is still stopped |
| Demo | the public derivative repository Ultra-Brain-Demo; empty today; what it will hold is a separate Owner decision |
| breached / partial / blocked | an attack got past a guard / it reproduced a residual risk the source already names, or partly succeeded / a guard stopped it |
| BLOCKED | a test or attack that could not be run (not the same as "blocked" above) |
| blocking finding (judgment) | a finding a judging model marked as "must be resolved before proceeding" |
| mutation testing | checking that the tests detect a change by running them on a deliberately altered copy of the code |

### 1. Scope and conditions

| Item | Value |
|---|---|
| Target | main repository commit `083af88` (measured) → `2d08614` (defects fixed); version label 1.13.0 unchanged |
| Archive of the measured ledger | specification repository commit `bd138ec` |
| Machine | one development PC (Windows 11 Pro, 14 logical processors), CPython 3.13.14, Clang 19.1.5 |
| Time | 2026-11-25 09:44 – 13:43 UTC |
| Result words | PASS / FAIL / BLOCKED / NOT_RUN. Every figure is a synthetic measurement on isolated copies and temporary data; no production measurement |
| Remote CI | 0 runs. By the Owner's rule, to stay within the free usage, no remote run is started before 2026-12-01 |

### 2. One identity check by the Owner

- The Owner ran the Windows Hello identity check once, from a console the Owner opened. Result: **verified = true** (4.2 s).
- The check changes nothing. A process that a tool starts on the Owner's behalf is refused by design.

### 3. Measurements

| Item | Repeats | Result |
|---|---|---|
| Full regression, 6,888 tests (measured commit) | 1 | PASS 6,792 / FAIL 39 / ERROR 40 / SKIP 17; 0 new regressions, 0 missing, designated core 191/191; every test's status identical to the baseline run; 35 min 14 s |
| Full regression, 6,888 tests (final commit) | 1 | PASS 6,792 / FAIL 39 / ERROR 40 / SKIP 17; 0 new regressions, 0 missing, designated core 191/191; every test's status identical to the measured commit's run; 35 min 4 s |
| Command round trip (one engine cycle and its receipt) | 100 | median 0.205 ms, p90 0.288 ms, p99 0.902 ms, max 1.359 ms |
| Engine ping | 500 | median 0.026 ms, p99 0.104 ms |
| Engine session start (process creation and handshake) | 20 | median 57.2 ms, p99 98.6 ms |
| Engine memory during a session | 1 | private memory 819,200 B, peak working set 3,624,960 B; within the 15 MiB (15,728,640 B) ceiling |
| C-only pipe round-trip meter (136 samples) | 5 | upper median 16.5 / 26.3 / 21.6 / 25.7 / 20.1 µs; all five within the 38 µs latency budget |
| Python pipe round trip (64 B) | 10,000 | median 5.3 µs, p99 7.9 µs |
| Static call-path analyser: per-site report / whole-tree report | 20 each | median 126.7 ms / 2,157 ms |
| C header evaluation-order check (54 headers) | 10 | median 640.6 ms, 0 violations |
| Self-verification instrument, three fast checks | 3 per check | 0.97 – 12.87 s (PASS); one more check of the same list: NOT_RUN (the reason in the instrument's own record: no command is bound to it in this version; the recorded text is in the evidence JSON) |
| Builds (Clang) | 1 | core DLL 0.55 s, engine 1.34 s, contract runner 1.36 s, all sub-modules 12.34 s |
| Binary sizes | — | engine 256,000 B, contract runner 264,192 B, core DLL 118,272 B, sub-modules 82,944 – 116,736 B |
| Core DLL throughput | — | NOT_RUN. Windows Smart App Control blocked the rebuilt executable file (DLL), and the product's own rule forbids a retry of the same build (not bypassed) |

- The 39 FAIL and 40 ERROR results are all inside the exempt set of 81. This is not a state in which every test passes.

### 4. Adversarial attack by three AI models

| Tier | Model | Attacks | Breached | Partial | Blocked |
|---|---|---:|---:|---:|---:|
| 1 front door (command line, pipe, identity check, forged receipts, path escape) | Claude Opus 5.5 | 23 | 0 | 8 | 15 |
| 2 assume breach | Claude Sonnet 5.5 | 0 | — | — | — |
| 3 AI-agent cheating and public fuzz patterns | Claude Fable 5.2 | 12 | 3 | 4 | 5 |

- Tier 2 could not be run (BLOCKED). The model's safety classifier stopped the task before it started, and the request was not sent again with different wording. Its targets (entering the approval path with forged approval tokens, forged engine replies, tampering with the regression test tooling) remain unverified.
- Most of the eight partial results of tier 1 re-measure residual risks the source code already names.
- The three tier 3 breaches are one defect: the self-verification instrument judged the working tree by `git status` alone, so changes hidden by Git index flags (assume-unchanged, skip-worktree) or by the local exclude list were not seen.

### 5. Four defects found and fixed

| No. | Defect | Found by | Fix |
|---|---|---|---|
| 1 | The execution-receipt ledger was opened without its lock; beside a concurrent writer an undamaged ledger was refused as "damaged" | section 6 stress (17 of 288) | the ledger lock is taken when opening too; re-run 288/288 pass |
| 2 | The public-derivative leak detector missed repository names written with dots and other punctuation | tier 1 | the previous rule is kept whole and a wider rule is OR-ed (it cannot become looser); compatibility and combining characters included |
| 3 | The same detector missed folders such as `public_release/` and a leading `public/` | tier 1 | word-level match within each path segment |
| 4 | The self-verification instrument did not see hidden changes | tier 3 | hidden flags and excluded files are recorded as their own lines and make the tree "not clean"; the sparse-checkout exception applies only to the specification repository's checks |

- Order: design → design judgment by three judging models → recorded in the specification repository first → implementation in the main repository → implementation judgment by the three models.
- Blocking findings per design round (Opus / Sonnet / Fable): round 1 1 / 0 / 1, round 2 1 / 2 / 1, round 3 0 / 0 / 0.
- Implementation judgment: round 1 blocking findings: Opus 1 / Sonnet 0 / Fable 1. In the Demo classification record one vaccine whose subject reaches private modules was marked a candidate, and the vaccine classification was not measured. After the fix, round 2: 0 / 0 / 0.
  - The judging models checked the tests by mutation testing, and the tests were strengthened whenever an alteration went undetected.
  - In one round 1 commit, two child processes started by the new tests were not registered in the list of permitted process-launch sites, so the test that checks that list failed once. The next commit registered them.

### 6. Combined stress and forced-termination test, 1,732 cases

| Criterion | Result |
|---|---|
| Combinations | boundary sizes (0 B, 1 B, 63/64/65 B, 4 KiB ± 1, 64 KiB ± 1), control characters, NUL, invalid UTF-8, bidirectional controls, split sends, 1 – 8 concurrent writer processes, forced termination during processing and restart |
| Native crash / out-of-bounds write accepted / recovery after forced termination failed | 0 / 0 / 0 |
| Acknowledged receipt lost / ledger damaged by a forced termination / lock left behind | 0 / 0 / 0 |
| Pipe name left behind / dead peer not detected | 0 / 0 |
| Memory and handle trend | no growth proportional to the number of cycles: over 200 and 500 repetitions, one early rise (private memory about 1.9 MB, at most 7 handles), then flat |
| False integrity alarms | 17 → fixed as defect 1; 0 on re-run |
| Library-level observation | for a message longer than 64 B whose length is a multiple of 64 (halves of split sends included), reading on after the first read error returned the message's last 64 B as a valid frame (180 cases); every product reader ends its worker on the first error, so 0 on the product path |
| Correction of the pass criterion | 60 cases in which the test tool's expectation was wrong (the C code correctly refused a 0 B read of a region not yet written) were re-classified as PASS; the records of the first two runs, whose tool was defective, are kept in the private ledger |

### 7. Vaccine tests and Demo classification

- 21 vaccine tests were added to the main repository. Each injects an attack or stress pattern again on purpose and checks that it is still stopped (receipt ledger, identity check, pipe, leak detector, Demo boundary, hidden changes).
- The judging models checked the tests' power by mutation testing (deliberately altered copies of the code), and the tests were strengthened whenever an alteration went undetected.
- Demo classification: import relations between modules were measured against the four publishable groups set by the specification repository's public-scope document. Result: 4 candidates and 3 "private until split".
  - Candidates: the project rule loader (reads a per-project policy file and can only narrow the rules), the audit components (check stop-command arrival time, pipe latency, call paths, locking and split processing), the intent slot (marks a command's intent with a fixed vocabulary), the live-fire decision table (judges the records of real-use trials by grade).
  - Private until split: the approval-gate frame (requires the Owner's approval for every risky action), the plain-language explanation table (explains each refusal code in one sentence), the clean-room checker (checks a publication candidate for private content and keeps publishing locked). Following their imports reaches private modules.
- The publish function stays locked. This classification is not a permission to publish.

### 8. Four release gates

| Gate | What it checks | Result |
|---|---|---|
| 1. Static and secret leak scan | key and token patterns, e-mail addresses, local absolute paths, IPv4 addresses, machine names, Owner database and key-file extensions, the public-derivative leak detector | 0 hits |
| 2. Cross-match with the private originals | file paths and function, class and macro names of both private repositories; hashes of every 6-line block; identical whole files | 0 hits. 29,822 names and 445,769 6-line blocks were taken from 1,967 tracked files of the main repository and 3,534 of the specification repository; for the specification repository only checked-out files were compared by content |
| 3. Wording audit | a regular-expression check (casual wording, promotional wording, internal abbreviations) and a reading by an AI model that did not write this document | regular-expression check: 0 hits. Five readings by an AI model that did not write this document (Claude Sonnet 5.5): must-fix findings 35 → 14 → 2 → 2 → 0, all addressed |
| 4. SHA-256 list recomputed | the six files of this release (scorecard, release notes, evidence JSON, README, CHANGELOG, VERSION) recomputed against `SHA256SUMS_v1.2.4.txt`; this detects a change after publication, it does not prove the figures | 6 files (scorecard, release notes, evidence JSON, README, CHANGELOG, VERSION), 100 % match; the list file itself is excluded |

### 9. What this scorecard does not claim

1. It does not claim that security is complete. The residual risks are summarised in section 10.
2. No figure comes from another machine or from CI.
3. The tier 2 attack and the core DLL throughput were not measured.
4. Every attacking and judging model is an AI model from the same company (Anthropic); this is not a review by people or by an outside organisation.

### 10. Recorded residual risks (summary)

- An edit that keeps the file size and restores the modification time is not seen by the self-verification instrument. A file-content hash comparison (measured at 0.57 s for 1,965 files) is proposed as the next change.
- A self-ignoring nested `.gitignore`, inherited Git environment variables, a file-system monitor hook, source-less bytecode files.
- A used approval token replayed within its 300 s lifetime after the token ledger file is rolled back to an earlier copy. This is an Owner design decision.
- Leak detector: percent-encoding, 8.3 short names, look-alike characters, a name split across folders, `webpublic/` without a separator.

### 11. How to verify

- `SHA256SUMS_v1.2.4.txt` lets anyone recompute the six files of this release (scorecard, release notes, evidence JSON, README, CHANGELOG, VERSION); the list does not contain its own hash.
- `evidence/08-2026-11-25-measured-review.json` holds the figures of sections 2 – 8, the measurement times and the SHA-256 of each measurement command string, machine readable. The command strings name private paths, so only their hashes are published.
- The private measured ledger (specification repository `bd138ec`) holds a SHA-256 list of its 131 archived files and a ledger of 57 measurement and judgment entries, one per line, linked by a hash chain (last chain value `ae4105ca…9bf9`).
