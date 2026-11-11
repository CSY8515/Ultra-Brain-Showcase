# Ultra Brain Showcase

## #1 C-Seed-0 — 실물 실행 영수증 (2026-11-11)

| 항목 | 결과 | 근거 |
|---|---|---|
| 단일 바이너리 | 175,616 bytes, Clang 19.1.5 (`-Wall -Wextra -pedantic -Werror`), 경고 0 | SHA-256 `8bd1e8d41a30036df66f570221574fb398a92876b7428550eb4675b4d62f63be` |
| Windows CI | 단위 테스트 **108/108 PASS** (24 + 84) · 실물 프로세스 체크 **4/4 PASS** (`--selftest` 14/14, `--resilience`, `--benchmark`, `--guards`) | GitHub Actions `windows-latest`, 원본 저장소(비공개) run 42997831414, commit `10680d5` |
| Windows Sandbox 오프라인 | 네트워크 차단 상태에서 `--demo` · `--selftest` 14/14 · `--guards` **ALL PASS** | Owner 직접 확인 (2026-11-11) |

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

15초 오프라인 실행 영상: 추가 예정
