# Ultra Brain Showcase

쇼케이스 버전: `1.1.0` ([VERSION](VERSION)). 관련 저장소: [Ultra-Brain-Labs](https://github.com/CSY8515/Ultra-Brain-Labs) — C 런타임 없이 `KERNEL32.dll`만 쓰는 Windows CLI 레퍼런스(5,120 B 바이너리, SHA-256 `a535d15b…`)와 빌드·검증 가이드. 사령부 공개 저장소: [Ultra-Brain-Legacy](https://github.com/CSY8515/Ultra-Brain-Legacy) (v1.60).

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
