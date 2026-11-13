# Ultra Brain Showcase

쇼케이스 버전: `1.2.2` ([VERSION](VERSION), [CHANGELOG](CHANGELOG.md)). 이 저장소는 성적표, 영수증, 검증 해시, 감정서만 싣습니다. 본체 소스는 싣지 않으며, 어떤 공인 기관의 인증도 아닙니다.

| 저장소 | 버전 | 내용 |
|---|---|---|
| Ultra-Brain (비공개 본체) | v1.8.3 | C-Seed 엔진, 코텍스, Stage 8 소지능 사령탑, 복합 명령 게이트(v1.8.2), 등록 문자 집합(v1.8.3) |
| [Ultra-Brain-Legacy](https://github.com/CSY8515/Ultra-Brain-Legacy) (사령부) | [v1.68](https://github.com/CSY8515/Ultra-Brain-Legacy/releases/tag/v1.68) | 사양서, 본체 **v1.8.2** 계약 헤더 두 개 탑재(v1.8.3 헤더는 미탑재), 제97조 제정, 러너 26/26·메시 33/33 |
| [Ultra-Brain-Labs](https://github.com/CSY8515/Ultra-Brain-Labs) | [v1.1.0](https://github.com/CSY8515/Ultra-Brain-Labs/releases/tag/v1.1.0) | `KERNEL32.dll`만 쓰는 Windows 도구 2종: 5,120 B CLI와 named pipe 에이전트 IPC |

## #6 복합 명령 게이트·등록 문자 집합과 8차 교차 감사 — v1.8.2 · v1.8.3 (2026-11-13)

[BENCHMARK_V1.8.3.md](BENCHMARK_V1.8.3.md) · [EVALUATION_STAGE8_V183.md](EVALUATION_STAGE8_V183.md) · 원자료 [evidence/06-v183-v168-benchmark.json](evidence/06-v183-v168-benchmark.json)

본체 Ultra Brain v1.8.2(코드 `b1b58fe`)와 v1.8.3(코드 `2e90677`), 사령부 v1.68(`5f74841`)의 성적표입니다. 본체가 7차 판정의 복합 명령 결함을 고치고 사령부의 제97조를 구현했으며, 독립 실행한 Claude Opus 5.5가 그 결과를 감사했습니다.

| 항목 | 값 | 출처 |
|---|---|---|
| 전수 회귀 6,888건 (버전당 단일 실행) | 두 버전 모두 **6,790 PASS / 41 FAIL / 40 ERROR / 17 SKIP**, 신규 회귀 0, 누락 0, 핵심 191/191 | 본체 `b1b58fe`, `2e90677`, 2026-11-13 |
| 실프로세스 판정 / 단위 테스트 | v1.8.2 11/11 PASS / 262/262 PASS · v1.8.3 11/11 PASS / 265/265 PASS | 본체 CI, 버전당 4회 모두 success |
| 컴파일러 | Clang 20.1.8 `-Werror`·MSVC `/W4 /WX` 경고 0, 자가 시험 18/18 | 같은 run |
| 러너 크기 | 278,016 B → **284,160 B** (한도 307,200 B) | 같은 run |
| 상주 데몬 30초 soak (8표본) | Job 최대 **1,220,608 ~ 1,306,624 B (1.16 ~ 1.25 MiB)**, 유휴 2초 구간 CPU 0.0초 | 같은 run |
| 결정론 | P6 기준 영수증 SHA-256 `b53b511e…` 불변, 사이클 경로 헤더 8개 불변 | 같은 run |
| 사령부 도킹 | 러너 **26/26**, 메시 **33/33**, 계약 헤더 blob이 본체 **v1.8.2**와 동일 | Legacy run 43423157347 |

- 81건의 FAIL/ERROR는 역사적 오너 면제 집합입니다. raw ALL PASS가 아닙니다.
- soak 값은 같은 바이너리에서도 실행마다 수만 B씩 다릅니다. 한 표본을 그 버전의 값으로 읽으면 안 됩니다.
- 로컬 개발기에서는 Smart App Control이 빌드의 실행을 막아, 엔진 위의 수치는 모두 CI 표본입니다.
- 사령부는 v1.8.2 헤더를 싣고 있습니다. v1.8.3 헤더의 재탑재는 오너 지시로 보류 중입니다.

**바꾼 것**

- **v1.8.2, 복합 명령 게이트.** 명령을 절로 나누고 절의 모든 토큰을 판정합니다. 레이더(토큰 30개, 패턴 9개, 드라이브 지정)는 나누기 전의 전체 텍스트에서 돕니다. 7차 판정이 든 `read status and format c:`, `check and wipe disk`, `show everything then erase disk`는 이제 오너를 기다립니다.
- **v1.8.3, 등록 문자 집합.** 한글 음절, 인쇄 가능 ASCII, 탭·줄바꿈 밖의 문자나 잘못된 UTF-8이 있으면 접수에서 명령 전체를 거부합니다. 전각 문자와 폭 없는 공백을 지우거나 접지 않습니다. 오너 토큰은 소모되지 않습니다.

**8차 판정 요약** (전문은 [EVALUATION_STAGE8_V183.md](EVALUATION_STAGE8_V183.md)):

| 오너의 질문 | Claude Opus 5.5 8차 판정 |
|---|---|
| 복합 명령 위장 침투가 완전히 차단됐는가 | **부분 성립.** 7차의 세 반례와 목록 단어 유형은 닫혔다. "완전히"는 성립하지 않는다 |
| 전각·유니코드 공백 우회가 완전히 차단됐는가 | **부분 성립. 사령탑 경로 안에서는 성립.** 질문의 전제가 7차 원문과 다르다 |
| 메타섹터가 결속된 소지능 커널은 인지 커널인가 | **불성립.** "규칙 기반 적응 커널 위의 결정론적 명령 파이프라인, 그 앞의 구문 필터" |

- 7차 결함 10건의 재판정: **해소 0건, 부분 해소 2건, 미해소 8건.** 두 릴리즈가 겨냥한 것은 복합 명령 하나였습니다.
- 8차가 새로 찾은 것:
  - 레이더 단어의 변형은 오너 없이 지나갑니다. `reformat`, `shutdown_now`, `power_off`, `re-boot`가 그렇습니다.
  - 접속어를 넣으면 판정이 약해지는 경우가 있습니다. `read status run payload`는 대기하고 `read status and run payload`는 통과합니다.
  - 관제 화면, 상주 데몬, dock 명령줄은 이 규칙들을 거치지 않습니다.
  - 대기 증적은 절을 8개까지만 보여 줍니다. 증적의 위험도는 첫 동사의 것입니다.
  - 거부된 명령에도 엔진이 먼저 로드되고 인사·종료 프레임이 오갑니다.
  - 무해한 입력이 지나치게 많이 대기합니다. `read status?`, `please read status`, `show ledger -v`가 그렇습니다.
- 등록 문자 집합에 대한 8차의 공격은 사령탑 경로 안에서 모두 실패했습니다.
- 질의 2의 전제는 틀렸습니다. 7차가 지적한 유니코드 공백은 명령 입구가 아니라 R0 증명기의 것이고, 그 결함은 지금도 남아 있습니다.
- 구현한 Claude Fable 5.2는 이 주장들을 같은 코드에서 재현했고, 자기 릴리즈 노트와 R&D 리포트의 문장 여덟 개를 과장으로 인정했습니다.
- 지시문의 "228,057건 검증"은 제품이 아니라 구현 전 설계 시제품의 검사 건수입니다. "1,232,896 B"는 여덟 표본 중 하나입니다.
- 이 결함들은 본체 v1.8.3에 그대로 있습니다. 의미 이해와 자연어 추론은 여전히 없습니다.

## #5 6차 판정 결함 수정과 7차 재판정 — v1.8.1 (2026-11-13)

[BENCHMARK_V1.8.1.md](BENCHMARK_V1.8.1.md) · [EVALUATION_STAGE8_V181.md](EVALUATION_STAGE8_V181.md) · 원자료 [evidence/05-v181-v167-benchmark.json](evidence/05-v181-v167-benchmark.json)

본체 Ultra Brain v1.8.1(코드 `3c51e15`), 사령부 v1.67(`e583aaf`), Labs v1.1.0(`a69a0cd`)의 성적표입니다. 6차 판정이 든 결함을 본체가 고쳤고, 독립 실행한 Claude Opus 5.5가 그 수정을 다시 감사했습니다.

| 항목 | 값 | 출처 |
|---|---|---|
| 전수 회귀 6,888건, 2차 실행 | **6,790 PASS / 41 FAIL / 40 ERROR / 17 SKIP**, 신규 회귀 0, 누락 0, 핵심 191/191 | 본체 `3c51e15`, 2026-11-13 |
| 전수 회귀, 1차 실행 | 6,789 / 42 / 40 / 17, 신규 실패 1건 | 같은 커밋 |
| 실프로세스 판정 / 단위 테스트 | 10/10 PASS / 238/238 PASS | 본체 CI run 43392553181 (첫 시도 success) |
| 컴파일러 | Clang 20.1.8 `-Werror`·MSVC `/W4 /WX` 경고 0, 자가 시험 17/17 | 같은 run |
| 상주 데몬 30초 soak | Job 최대 **1,286,144 B (1.23 MiB)**, 유휴 2초 구간 CPU 0.0초 | 같은 run |
| 결정론 | P6 기준 영수증 SHA-256 `b53b511e…` 불변, 사이클 경로 헤더 8개 불변 | 같은 run |
| 사령부 도킹 | 러너 **25/25**, 메시 **32/32**, 계약 헤더 blob이 본체 v1.8.1과 동일 | Legacy run 43405793943 |

- **회귀를 두 번 돌렸습니다.** 1차 실행의 실패 1건은 기존 제품 시험이 출력에서 포트 문자열 `9443`을 부분 문자열로 찾는데, 그 실행의 무작위 SHA-256 다이제스트(`4b969443792c…`)에 같은 글자가 들어 있어서 생겼습니다. 실행당 약 0.5 %의 확률입니다. 격리 재실행은 6/6 PASS였고 그 시험은 고치지 않았습니다. "신규 회귀 0"은 2차 실행의 결과입니다.
- 81건의 FAIL/ERROR는 역사적 오너 면제 집합입니다. raw ALL PASS가 아닙니다.
- 호스트 비용 표본은 v1.8.0보다 나빠졌습니다. 보고 단계 448.6 µs(이전 약 50 µs), 사이클 왕복 3.987 ms(이전 약 0.73 ms). CI 표본 1개이며 반복 측정은 하지 않았습니다.
- 로컬 개발기에서는 Smart App Control이 v1.8.1 빌드의 실행을 막아, 엔진 위의 수치는 모두 CI 표본입니다.

**v1.8.1이 바꾼 것**: 오너 토큰(오너 키의 HMAC-SHA256, 행위·대상 결속, 300초, 디스크 원장), 단어표 밖 동사의 오너 게이트, 숫자의 자리 결속과 비 ASCII·구분자 숫자 거부, 줄·공백 유무를 보존하는 R0 증명기, 넘겨받은 기록에서 계산하는 증명 비트.

**7차 재판정 요약** (전문은 [EVALUATION_STAGE8_V181.md](EVALUATION_STAGE8_V181.md)):

| 오너의 질문 | 6차 (v1.8.0) | 7차 (v1.8.1) |
|---|---|---|
| 환각 0 % 보고 폐회로가 성립했는가 | 부분 성립 | **부분 성립.** "고정 템플릿 아래의 숫자 자리 결속 필터"이며 폐회로가 아니다 |
| 제86조가 오너 주권에 안전하게 종속됐는가 | 부분 성립 | **부분 성립.** 토큰을 쓰는 쪽이 키를 쥐고 있고, 실제로 도는 실행기는 없다 |
| 자율 인지 OS 커널 범주인가 | 불성립 | **불성립.** "규칙 기반 적응 커널 위의 결정론적 명령 파이프라인" |

- 6차 결함 10건의 재판정: **해소 1건, 부분 해소 8건, 미해소 1건.** 두 건은 "약점의 자리만 옮겨졌다"는 판정입니다.
- 7차가 새로 찾은 것:
  - 토큰을 검증하는 쪽이 같은 키로 발급도 할 수 있습니다.
  - 증명 기록은 서명 없는 dict라 꾸밀 수 있습니다.
  - 원장을 지우거나 동시에 열면 토큰 재사용이 통과하고, 동시 사용 뒤에는 원장이 영구히 막힙니다.
  - `read status and format c:` 같은 복합 명령은 첫 동사만 보는 게이트를 지나갑니다.
  - R0 증명기는 헤더 이름 안의 `//`, 삼중자, 유니코드 공백을 놓칩니다.
- 구현한 Claude Fable 5.2는 이 주장들을 같은 코드에서 재현했고, v1.8.1 릴리즈 노트에서 과장으로 지적된 문장 여섯 개를 인정했습니다.
- Windows Hello 결속은 없습니다. 오너 토큰은 같은 Windows 사용자의 프로세스가 키를 풀었다는 것만 증명하고 사람을 증명하지 않습니다. 의미 이해와 자연어 추론은 여전히 없습니다.
- 이 결함들은 v1.8.1 시점의 기록입니다. 복합 명령 게이트의 수정과 그 재판정은 위 #6에 있습니다. 나머지는 v1.8.3에도 그대로 있습니다.

## #4 Stage 8 소지능 사령탑 — 실측 벤치마크와 6차 교차 감정 (2026-11-13)

[BENCHMARK_V1.8.0.md](BENCHMARK_V1.8.0.md) · [EVALUATION_STAGE8.md](EVALUATION_STAGE8.md) · 원자료 [evidence/04-v180-v165-benchmark.json](evidence/04-v180-v165-benchmark.json)

본체 Ultra Brain v1.8.0(코드 `da6debe`), 사령부 v1.65(`bcd8e1a`), Labs v1.1.0(`a69a0cd`)의 성적표입니다.

| 항목 | 값 | 출처 |
|---|---|---|
| 전수 회귀 6,888건 (단일 실행) | **6,790 PASS / 41 FAIL / 40 ERROR / 17 SKIP**, 신규 회귀 0, 누락 0, 핵심 191/191 | 본체 `da6debe`, 2026-11-13 |
| 실프로세스 판정 / 단위 테스트 | 10/10 PASS / 220/220 PASS | 본체 CI run 43381020952 |
| 컴파일러 | Clang 20.1.8 `-Werror`·MSVC `/W4 /WX` 경고 0, 자가 시험 17/17 | 같은 run |
| 상주 데몬 30초 soak | Job 최대 **1,273,856 B (1.21 MiB)**, 한도 15 MiB의 8.1 %. 유휴 2초 구간 CPU 0.0초 | 같은 run |
| 결정론 | P6 기준 영수증 SHA-256 `b53b511e…` 불변. 사이클당 28 프레임, QUERY당 55 프레임. 사이클 경로 헤더 8개가 v1.7.91 바이트와 동일 | 같은 run |
| 사령탑 실엔진 | `read status` → 보고 완료(28 프레임, 불일치 0). `delete system now`는 오너 nonce 없이 대기, 사이클 0 | 같은 run |
| 사령부 도킹 | 러너 **25/25**, 메시 **32/32**, 계약 헤더 blob 동일 | Legacy run 43387715941 |
| Labs 파이프 IPC | 1,000회 왕복 평균 **0.027 ~ 0.051 ms**, 첫 왕복 8 ~ 15 ms | Labs CI 세 run |
| 엔진 warm 왕복 | 중앙값 0.244 ms는 2026-11-11 코텍스 이전 빌드의 값. **v1.8.0은 미측정** | evidence/02 |

- 81건의 FAIL/ERROR는 역사적 오너 면제 집합입니다. raw ALL PASS가 아닙니다.
- 로컬 개발기에서는 Smart App Control이 v1.8.0 빌드의 실행을 막아, 엔진 위의 수치는 모두 CI 표본입니다.
- 오너 지시문의 수치 중 측정과 달랐던 것은 벤치마크 문서 §7·§8에 정정했습니다. 1,273,856 B는 1.27 MiB가 아니라 1.21 MiB입니다. 클라우드 LLM 대비 배수는 재지 않아 싣지 않았습니다. DPAPI는 하드웨어 키가 아니라 Windows 사용자 프로필 결속입니다. SourceSeal은 v1.8.0에서 검증하지 않았습니다. v1.8.0의 샌드박스 실행은 NOT_RUN입니다.

**교차 감정 요약** (전문은 [EVALUATION_STAGE8.md](EVALUATION_STAGE8.md)):

| 오너의 질문 | Claude Opus 5.5 6차 판정 |
|---|---|
| `si_report` 채움 규칙으로 환각 0 % 보고 폐회로가 성립했는가 | **부분 성립.** 정확한 이름은 "숫자 토큰 출처 필터". 값과 자리의 결속, 판정어, 전각 숫자·한국어 수사는 검사하지 않는다 |
| 제86조 자가 리팩토링이 제78조 오너 주권에 종속되어 안전하게 가동되는가 | **부분 성립.** 순서와 검사는 코드로 강제되나 결정과 nonce를 호출자가 만들 수 있다. 실행기가 없어 수행된 정돈은 0건 |
| "자가 정돈 사령탑을 갖춘 초경량 자율 인지 OS 커널"로 범주가 도약했는가 | **불성립.** 정확한 이름은 "규칙 기반 적응 커널 위에 얹은 결정론적 명령 파이프라인" |

- 5차 판정이 "없다"고 한 사령탑은 최소 형태로 생겼습니다. 의미 이해, 자연어 추론, LLM 연결, 서명 배포는 여전히 없습니다.
- 구현한 Claude Fable 5.2는 6차 판정의 반례 9건을 같은 코드에서 재현했고 전부 사실이었습니다. 릴리즈 노트의 "보고가 틀릴 수 있는 유일한 길은 잘못된 템플릿"이라는 문장은 과장이었다고 적었습니다.
- 이 결함들은 v1.8.0 시점의 기록입니다. v1.8.1의 수정과 그 재판정은 위 #5에 있습니다.

## #3 C-Seed-1 Cortex — 실측 벤치마크와 양대 AI 교차 감정 (2026-11-13)

[03_CORTEX_BENCHMARK_AND_CROSS_APPRAISAL.md](03_CORTEX_BENCHMARK_AND_CROSS_APPRAISAL.md) · 원자료 [evidence/03-v1790-v160-benchmark.json](evidence/03-v1790-v160-benchmark.json)

본체 Ultra Brain v1.7.90(코드 `52a8e04`)과 사령부 Ultra-Brain-Legacy v1.60(태그 `db8a33f`)의 성적표입니다. 소스는 싣지 않고 실측치·CI 결과·검증 해시·두 모델의 감정만 싣습니다.

- 실측·CI (출처는 문서 §1):
  - 상주 데몬 Job Object 최대 **1,253,376 B (1.20 MiB)**, 15 MiB 한도의 1/12.5. 30초 soak 동안 엔진 유휴 CPU **0.0초** (CI)
  - 전수 회귀 **6,888건 단일 실행, 신규 결함 0건** (6,790 PASS / 41 FAIL / 40 ERROR / 17 SKIP, 핵심 191/191). 81건은 역사적 면제이며 raw ALL PASS가 아닙니다
  - 빌드: 본체 Clang `-Werror`·MSVC `/W4 /WX` 경고 0, 자가 시험 16/16, 단위 183/183. 사령부 러너 **24/24**, 메시 **31/31**, 16B 정렬 오차 0
  - 자가 주조 24벡터 다이제스트 `0x28762C01` (Python 독립 계산 일치)
  - 반응 지연 중앙값 **0.244 ms**는 2026-11-11 코텍스 이전 빌드의 warm 왕복 실측입니다. **FAST 조언 경로의 시간과 콜드스타트 복원 시간은 미측정(NOT_RUN)**입니다
- Claude Fable 5.1 감정(현장 주조관): 학습 조언 결속은 오너 /4 봉인과 봉인 기억 우선이라는 두 게이트 아래에서만 작동하며, 250 ms 타이머 레이스는 테스트 helper에서 고쳤고 제품 코드는 바꾸지 않았습니다. 학습기 수학은 C11 정수·Q8뿐이지만 영속화·봉인·텔레메트리 대조는 Python 호스트가 맡습니다.
- Claude Opus 5.5 5차 판정(수석 감리관): 4차 판정의 "규칙 기반 최소 기계 지능" 하한선은 계속 충족하며, 새 이름은 **"엔진 내 누적 학습을 가진 규칙 기반 적응 커널"**입니다. 질적 도약이 아니라 같은 범주 안의 한 단계 전진으로 판정했습니다.
  - 4차의 다음 경계 5개 중 의미 특징·C 네이티브 학습은 **부분**, 내용 결속 승인·신뢰 배포·대뇌 연결은 **미구현**.
  - 정정: `delete temp`와 `delete system`은 둘 다 위험도 3이라 같은 학습 칸을 쓰고 목적어 해시만 다릅니다. "1.25 MiB"는 1.20 MiB. "0초 복원"과 "FAST 0.244 ms"는 측정값이 아닙니다.
  - 새로 지적한 결함 2건: 오너 봉인은 `cortex_apply` 스위치 1비트에만 걸리고 가중치 파일은 키 없는 SHA-256만으로 보호됩니다. 학습된 격리로 막힌 사이클도 거부로 학습되어 격리가 스스로 강화됩니다.
  - 오너가 앱에서 실제로 학습 조언을 켜는 경로(/4 봉인 → CTRL)는 시험이 없어 **UNKNOWN**입니다.
- 의미 이해·자연어 추론·지식은 여전히 없습니다. 어떤 공인 기관의 인증도 아닙니다.

## #2 C-Seed-0 vs Claude — 8개 항목 대조 (2026-11-11)

[02_C-SEED-0_VS_CLAUDE.md](02_C-SEED-0_VS_CLAUDE.md)

- C-Seed-0 실측(로컬 PC):
  - 같은 명령을 새 프로세스 100개에 넣어 다이제스트·영수증이 100/100 동일
  - 명령 → 영수증 왕복 중앙값 0.244 ms
  - 엔진 작업 집합 3.4 MiB
  - 규약 위반 시 거부·종료 중앙값 4.31 ms
- Claude 쪽은 미측정입니다(3.5 Sonnet 퇴역). 문서화된 사실과 가정을 밝힌 계산만 적었습니다.
- 자연어 추론과 지식에서는 Claude가 앞섭니다. C-Seed-0에는 그 능력이 없습니다.
- 아래 #1의 "로컬 호스트에서는 실행하지 않음"은 당시 기준입니다. 이후 같은 PC에서 엔진 실행이 확인됐습니다.
- 문서 하단에 Claude Opus 5.5의 감정을 네 차례 실었습니다(1차 정체성, 2차 영속 원장, 3차 기억→행동 폐회로, 4차 네 경계 구현 이후).
  - 3차 판정: 오너가 봉인한 경험이 같은 입력에 대한 다음 행동을 바꾸는 것이 실물 엔진과 CI에서 확인됐습니다(격리 / 빠른 경로). 1차 감정이 정한 첫 경계를 좁은 의미에서 통과한 "규칙 기반 적응 커널"입니다.
  - 4차 판정: 3차 판정이 정한 네 경계를 구현했습니다(일반화, 자기 교정, 정책 학습, 공식 승인). 각 통과 시험은 실물 엔진·CI에서 PASS했습니다. 1차 감정의 세 조건(적응·선택·일반화)을 모두 최소 형태로 충족하므로, 그 정의 안에서 "규칙 기반 최소 기계 지능"의 하한선을 넘었다고 판정했습니다.
  - 의미 이해, 스스로 규칙을 만드는 능력, 자연어 추론은 여전히 없습니다.

## #1 C-Seed-0 — 실물 실행 영수증 (2026-11-11)

| 항목 | 결과 | 근거 |
|---|---|---|
| 단일 바이너리 | 175,616 bytes, Clang 19.1.5 (`-Wall -Wextra -pedantic -Werror`), 경고 0 | SHA-256 `8bd1e8d41a30036df66f570221574fb398a92876b7428550eb4675b4d62f63be` |
| Windows CI | 단위 테스트 **108/108 PASS** (24 + 84) · 실물 프로세스 체크 **4/4 PASS** (`--selftest` 14/14, `--resilience`, `--benchmark`, `--guards`) | GitHub Actions `windows-latest`, 원본 저장소(비공개) run 42997831414, commit `10680d5` |
| Windows Sandbox 오프라인 | 네트워크 차단 상태에서 `--demo` · `--selftest` 14/14 · `--guards` **ALL PASS** | Owner 직접 확인 및 아래 녹화 (2026-11-11) |

- 로컬 호스트는 Smart App Control이 켜져 있어 서명 없는 EXE를 실행하지 않습니다. 실행 증거는 위 CI와 Sandbox입니다.
- CI는 같은 소스 커밋에서 바이너리를 새로 빌드해 실행합니다. Sandbox는 위 SHA-256의 바이너리를 실행합니다.

## TCC 자가 치유 시연 로그 (`--demo`, CI 캡처 원문)

데모의 크기·RAM·시간 수치는 모델에 정의된 고정 상수이며 실측값이 아닙니다.

```text
+----------------------------------------------------------------------+
| C-SEED-0 TCC SELF-HEALING MOTOR  -  DEMO (deterministic model)       |
+----------------------------------------------------------------------+
  NOTE: visual walk-through of the model in c_seed_motor_tcc.h.
        No real compiler runs; code is not actually synthesized.
        Size / RAM / time are FIXED header constants, not live
        measurements. Each step is what tcc_self_healing_compile()
        computes: a clean input needs 0 strikes; an injected fault
        is modelled as self-healing to success on strike 1.

  Scenario: clean candidate  (syntax_error_simulated = false)
    [1] synthesize candidate cell code ............. (model step)
    [2] strike 0 : no fault ...................... 
    [4] hard-limit gate : size<=300 KB  ram<=15 MB  time<=1000 ms
    ------------------------------------------------------------------
    RESULT : COMPILE OK             self-heal strikes = 0
    model binary size  : 148 KB  (fixed constant, not measured)
    model RAM peak     :   8 MB  (fixed constant, not measured)
    model compile time :  48 ms  (fixed constant, not measured)
    last error code    : 0
    entrypoint addr    : 0x00401000

  Scenario: injected syntax fault  (syntax_error_simulated = true)
    [1] synthesize candidate cell code ............. (model step)
    [2] strike 0 : FAULT ......................... model error 1001 (syntax mismatch)
    [3] self-correct + recompile ................. model retry -> strike 1
    [4] hard-limit gate : size<=300 KB  ram<=15 MB  time<=1000 ms
    ------------------------------------------------------------------
    RESULT : COMPILE OK             self-heal strikes = 1
    model binary size  : 148 KB  (fixed constant, not measured)
    model RAM peak     :   8 MB  (fixed constant, not measured)
    model compile time :  48 ms  (fixed constant, not measured)
    last error code    : 0
    entrypoint addr    : 0x00401000

+----------------------------------------------------------------------+
| DEMO COMPLETE  -  model self-heals on strike 1, bounded by 3 limits  |
+----------------------------------------------------------------------+
```

## 실행 영상

Windows Sandbox(네트워크 차단 설정) 안에서 C-Seed-0 바이너리가 실행되는 화면입니다.

- [c-seed-0-sandbox-1.mp4](media/c-seed-0-sandbox-1.mp4) — 24.6초, `--demo` 추가 전 빌드. 약 12초부터 콘솔에 `ALL 14/14 SUBSYSTEM CHECKS STRICTLY PASSED`, `MAIN P1 guards verified`, `OFFLINE RUN COMPLETE - ALL 14/14 + P1 GUARDS PASS`
- [c-seed-0-sandbox-2.mp4](media/c-seed-0-sandbox-2.mp4) — 38.7초, 현재 빌드. 약 16초부터 콘솔에 TCC 데모 완료 줄(`DEMO COMPLETE`), `--selftest` 14/14, `--guards`, `OFFLINE RUN COMPLETE - TCC DEMO + ALL 14/14 + P1 GUARDS PASS`
