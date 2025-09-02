# .md
.md 파일은 각각의 내용 정리 파일

## Verilog

### 1. 개요
- 국내 반도체 팹리스 현황, 삼성 파운드리의 4나노 수율 안정화 및 중국 AI 고객 유치 상황 소개
- Back-end (Design House), Front-end (Fabless) 설계 흐름

### 2. Verilog 핵심 개념
- **모듈 예제:** `ham_7_4_enc` Hamming Encoder 설계
- **initial vs always**
  - `initial`: 시뮬레이션 한 번만 실행
  - `always`: 반복 실행
- **논리 종류**
  - 조합논리 (Combinational Logic)
  - 순차논리 (Sequential Logic)
- **연산자**
  - 산술, 관계, 동등, 논리, 비트, 쉬프트 연산자 등
- **Reg vs Wire**
  - `wire`: 연속 할당
  - `reg`: 저장 기능 포함 (절차적 할당)
- **Blocking vs Non-blocking**
  - `=`: Blocking, 순차적
  - `<=`: Non-blocking, 병렬적
- **Latch vs Flip-Flop**
  - Latch 문제 방지를 위한 `else` 조건 필수
- **설계 예시**
  - 카운터, 쉬프트 레지스터 등

---

## SystemVerilog

### 1. DUT 및 테스트 환경
- Router DUT의 입력/출력 구조
- Verification 환경: Program Block, Interface, Clocking Block 등

### 2. 언어 기초
- **데이터 타입**
  - 2-State: `bit`
  - 4-State: `reg`, `logic`
  - 동적 배열, 연관 배열, 큐 등
- **연산자**
  - `inside`, `iff`, `===` 등

### 3. 병행성 (Concurrency)
- `fork...join` 등 병렬 처리 기법
- Thread 동기화, watchdog timer 구현 예시

### 4. 클래스 및 OOP
- `class`, `object`, `this`, 상속, 다형성
- 패키지 사용법 및 예제

### 5. 무작위화 (Randomization)
- `rand`, `randc`, `constraint`
- `randomize()` 사용 및 runtime 제어

### 6. 상속 및 스레드 간 통신
- `mailbox`, `semaphore`, `event` 활용 예

### 7. Functional Coverage
- 상태 전이 커버리지, 조건 커버리지 등
- 자동 bin 생성 및 측정 예제 포함

---

## Synthesis (Design Compiler)

### 1. 기본 합성 흐름
- RTL → Gate-level → Netlist

### 2. Timing 요소
- Setup/Hold, Recovery/Removal, Clock Skew, Critical Path

### 3. Timing 제약 명령어
- `create_clock`, `set_input_delay`, `set_output_delay`
- `set_clock_uncertainty`, `set_max_fanout`, `set_false_path`
- `set_multicycle_path` 등

### 4. 주요 개념
- Clock Transition, Max Transition Time
- Constraint 설정 예시 포함

---

## VCS

### 1. VCS Flow
- **3단계 Flow:** `vlogan → elaboration → simv`
- Legacy 코드 포함 가능

### 2. 커버리지
- 조건, 분기, FSM, Assertion 커버리지 지원
- URG로 커버리지 결과 병합 및 보고

### 3. Partition Compile
- 병렬 컴파일 지원 (3-step, profiling 지원)

### 4. FGP (Fine Grained Parallelism)
- 멀티코어 병렬 실행 지원
- EPC 계산, 성능 측정

### 5. X-Prop
- RTL 레벨 X 전파 이슈 검증

### 6. Save & Restore
- `$save`, `$restart` 명령어를 통한 상태 저장 및 복구

---

## VERDI

### 1. 데이터 준비 (FSDB, KDB)
- `$fsdbDumpfile`, `$fsdbDumpvars` 활용
- `vericom`으로 KDB 생성

### 2. GUI 기능
- 설정파일: `novas.rc`
- 매크로 주석, GUI 설정

### 3. 주요 기능
- **nTrace**: 신호 추적
- **nSchema**: 회로도 기반 설계 보기
- **nWave**: 파형 보기 및 조작
- **nState**: FSM 상태 흐름도
- **TFV**: Temporal Flow View, 원인 분석

### 4. 디버깅 활용
- Signal tracing, signal manipulation, forced signal 등
