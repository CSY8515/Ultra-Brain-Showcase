# Ultra Brain Showcase

관련 저장소: [Ultra-Brain-Labs](https://github.com/CSY8515/Ultra-Brain-Labs) — C 런타임 없이 `KERNEL32.dll`만 쓰는 Windows CLI 레퍼런스(5,120 B 바이너리)와 빌드·검증 가이드.

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
