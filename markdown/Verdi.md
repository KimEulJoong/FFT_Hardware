<<<<<<< HEAD
# Verdi 

Verdi는 Synopsys사의 디버깅/검증 GUI 툴로, RTL 디버깅, 파형 분석, 상태 기계 시각화, 신호 추적 등의 다양한 기능을 제공합니다.

---

## 1. Data Preparation (KDB & FSDB)

### 1.1 KDB 생성 및 활용
- KDB는 설계 데이터베이스로, Verdi에서 빠르게 로딩 및 탐색 가능
- 생성 명령어:
  ```bash
  $VERDI_HOME/bin/vericom -kdb <design_files>
  ```
- 예제 경로: `$VERDI_HOME/demo/Verilog`

- 저장된 세션 불러오기:
  ```bash
  verdi -dbdir simv.daidir -play verdi.cmd
  ```

### 1.2 FSDB 설정 (파형 저장)

- FSDB : verdi 에서는 DB에 WaveForm을 저장해서 사용자에게 제공한다. 무겁기 때문에 다양한 유틸리티 시스템을 이용하여 편의성을 제공하고자 한다.

- 파형 저장 초기화 예:
  ```systemverilog
  initial begin
    $fsdbDumpfile("./fsdb_dump/rtl.fsdb");
    $fsdbDumpvars(0, tb_top, "+all");
  end
  ```

- **주요 시스템 태스크**
  - `$fsdbDumpfile("파일경로")` – 덤프 파일 생성
  - `$fsdbDumpvars(<level>, <module>, "+all")` – 변수 저장
  - `$fsdbDumpon / $fsdbDumpoff` – 런타임 중 특정 시점만 저장

### 1.3 FSDB 유틸리티
- `fsdbdebug`: FSDB 구조 확인용
- `fsdbreport`: FSDB 요약 리포트 생성
- `fsb2vcd`, `vfast`, `fsdb2saif`: 형식 변환 유틸리티

---

## 2. Preference & GUI 설정

### 2.1 환경 설정 파일
- 전역 설정:  
  ```bash
  $HOME/novas.rc
  <NOVAS_HOME>/etc/novas.rc
  ```

### 2.2 사용자 설정 기능
- 파라미터 매크로 설정
- 마우스/단축키/윈도우 UI 요소 사용자 정의 가능

---

## 3. nTrace – 신호 추적 도구

### 3.1 기능
- 특정 신호의 드라이버, 영향을 받는 경로 자동 추적
- 소스 코드와 연결된 경로 브라우징 가능

### 3.2 활용 예
- 신호 선택 후 우클릭 → nTrace 실행
- 트레이스 결과에 따라 "Active Source Code" 자동 하이라이트

---

## 4. nSchema – 회로도(Schematic) 뷰어

### 4.1 기능
- 모듈 간 연결을 시각적으로 표현
- 신호/포트/하위 모듈 확인 가능

### 4.2 활용 팁
- 단축키 `x`: 신호 중심으로 회로 자동 줌인
- 회로도 내에서 직접 신호 트레이싱 가능
- Edit 기능으로 간단한 회로 수정 가능 (단, 시뮬레이션용은 아님)

---

## 5. nWave – 파형 뷰어

### 5.1 기본 사용
- 파형 보기 실행:
  ```bash
  verdi -ssf fsdb_dump/rtl.fsdb
  ```
- 시뮬레이션 종료 후 자동 로딩도 가능

### 5.2 주요 기능
- 신호 추가/제거, 줌인/줌아웃, 파형 비교
- 파형 강제(force) 및 조작 가능
- 신호의 논리 연산 표현 및 필터링
- 파형 간 동기화(Sync) 기능

---

## 6. nState – FSM 분석기

### 6.1 기능
- 상태 기계(State Machine) 자동 인식 및 시각화
- 상태 전이 분석 가능

### 6.2 사용 절차
- 메뉴: Tools > nState
- `OneSearch` 기능을 통해 특정 상태 탐색 가능
- 상태 전이 타이밍, 조건 등 상세 정보 제공

---

## 7. TFV – Temporal Flow View

### 7.1 개요
- 시간 흐름에 따른 신호의 원인-결과 추적 가능

### 7.2 주요 기능
- Root Cause 분석: X, Z 발생 원인을 단계별 추적
- TFV View와 nWave, nSchema 연계 가능
- Trace Signal to Driver, to Source Path 기능으로 다중 시점 연결

### 7.3 활용 예
- X 또는 오류 값이 발생한 신호를 선택
- TFV Trace 실행하여 영향 원인 분석

---

## 참고 명령어 요약

| 기능 | 명령어 예시 |
|------|-------------|
| Verdi 실행 | `verdi -ssf dump.fsdb` |
| KDB 생성 | `$VERDI_HOME/bin/vericom -kdb design.sv` |
| 저장된 세션 실행 | `verdi -dbdir simv.daidir -play verdi.cmd` |
| 커버리지 보기 | `verdi -cov -covdir simv.vdb` |
| 상태 기계 보기 | Verdi GUI → Tools > nState |
| 회로도 보기 | Verdi GUI → Tools > nSchema |
| TFV 실행 | Verdi GUI → Tools > Temporal Flow View |

---

## 마무리

=======
# Verdi 

Verdi는 Synopsys사의 디버깅/검증 GUI 툴로, RTL 디버깅, 파형 분석, 상태 기계 시각화, 신호 추적 등의 다양한 기능을 제공합니다.

---

## 1. Data Preparation (KDB & FSDB)

### 1.1 KDB 생성 및 활용
- KDB는 설계 데이터베이스로, Verdi에서 빠르게 로딩 및 탐색 가능
- 생성 명령어:
  ```bash
  $VERDI_HOME/bin/vericom -kdb <design_files>
  ```
- 예제 경로: `$VERDI_HOME/demo/Verilog`

- 저장된 세션 불러오기:
  ```bash
  verdi -dbdir simv.daidir -play verdi.cmd
  ```

### 1.2 FSDB 설정 (파형 저장)

- 파형 저장 초기화 예:
  ```systemverilog
  initial begin
    $fsdbDumpfile("./fsdb_dump/rtl.fsdb");
    $fsdbDumpvars(0, tb_top, "+all");
  end
  ```

- **주요 시스템 태스크**
  - `$fsdbDumpfile("파일경로")` – 덤프 파일 생성
  - `$fsdbDumpvars(<level>, <module>, "+all")` – 변수 저장
  - `$fsdbDumpon / $fsdbDumpoff` – 런타임 중 특정 시점만 저장

### 1.3 FSDB 유틸리티
- `fsdbdebug`: FSDB 구조 확인용
- `fsdbreport`: FSDB 요약 리포트 생성
- `fsb2vcd`, `vfast`, `fsdb2saif`: 형식 변환 유틸리티

---

## 2. Preference & GUI 설정

### 2.1 환경 설정 파일
- 전역 설정:  
  ```bash
  $HOME/novas.rc
  <NOVAS_HOME>/etc/novas.rc
  ```

### 2.2 사용자 설정 기능
- 파라미터 매크로 설정
- 마우스/단축키/윈도우 UI 요소 사용자 정의 가능

---

## 3. nTrace – 신호 추적 도구

### 3.1 기능
- 특정 신호의 드라이버, 영향을 받는 경로 자동 추적
- 소스 코드와 연결된 경로 브라우징 가능

### 3.2 활용 예
- 신호 선택 후 우클릭 → nTrace 실행
- 트레이스 결과에 따라 "Active Source Code" 자동 하이라이트

---

## 4. nSchema – 회로도(Schematic) 뷰어

### 4.1 기능
- 모듈 간 연결을 시각적으로 표현
- 신호/포트/하위 모듈 확인 가능

### 4.2 활용 팁
- 단축키 `x`: 신호 중심으로 회로 자동 줌인
- 회로도 내에서 직접 신호 트레이싱 가능
- Edit 기능으로 간단한 회로 수정 가능 (단, 시뮬레이션용은 아님)

---

## 5. nWave – 파형 뷰어

### 5.1 기본 사용
- 파형 보기 실행:
  ```bash
  verdi -ssf fsdb_dump/rtl.fsdb
  ```
- 시뮬레이션 종료 후 자동 로딩도 가능

### 5.2 주요 기능
- 신호 추가/제거, 줌인/줌아웃, 파형 비교
- 파형 강제(force) 및 조작 가능
- 신호의 논리 연산 표현 및 필터링
- 파형 간 동기화(Sync) 기능

---

## 6. nState – FSM 분석기

### 6.1 기능
- 상태 기계(State Machine) 자동 인식 및 시각화
- 상태 전이 분석 가능

### 6.2 사용 절차
- 메뉴: Tools > nState
- `OneSearch` 기능을 통해 특정 상태 탐색 가능
- 상태 전이 타이밍, 조건 등 상세 정보 제공

---

## 7. TFV – Temporal Flow View

### 7.1 개요
- 시간 흐름에 따른 신호의 원인-결과 추적 가능

### 7.2 주요 기능
- Root Cause 분석: X, Z 발생 원인을 단계별 추적
- TFV View와 nWave, nSchema 연계 가능
- Trace Signal to Driver, to Source Path 기능으로 다중 시점 연결

### 7.3 활용 예
- X 또는 오류 값이 발생한 신호를 선택
- TFV Trace 실행하여 영향 원인 분석

---

## 참고 명령어 요약

| 기능 | 명령어 예시 |
|------|-------------|
| Verdi 실행 | `verdi -ssf dump.fsdb` |
| KDB 생성 | `$VERDI_HOME/bin/vericom -kdb design.sv` |
| 저장된 세션 실행 | `verdi -dbdir simv.daidir -play verdi.cmd` |
| 커버리지 보기 | `verdi -cov -covdir simv.vdb` |
| 상태 기계 보기 | Verdi GUI → Tools > nState |
| 회로도 보기 | Verdi GUI → Tools > nSchema |
| TFV 실행 | Verdi GUI → Tools > Temporal Flow View |

---

## 마무리

>>>>>>> 51f10cc5d1487f53b7a7be2da46da77c4d7439f5
Verdi는 단순 파형 뷰어를 넘어서, 설계 디버깅 전체 과정(회로 분석, 상태 추적, 원인 분석 등)을 시각적이고 효율적으로 수행할 수 있도록 지원하는 통합 도구입니다.