<<<<<<< HEAD
## 1. VCS Flow Overview

### Synopsys 제공 RTL 시뮬레이터: VCS

#### 기본 흐름
1. **Design/Testbench 작성**
   - RTL 및 UVM 기반 testbench 구성: DUT, Interface, Driver, Monitor 등 포함

2. **Compile 단계**
   - 명령어:  
     ```bash
     vcs [design files] [testbench files] <옵션>
     ```
   - 목적: 시뮬레이션 바이너리(`simv`) 생성
   - 예시:  
     ```bash
     vcs -full64 -debug_pp -sverilog top.sv tb_top.sv
     ```

3. **Simulate 단계**
   - 명령어:  
     ```bash
     ./simv +<옵션>
     ```
   - 예시:  
     ```bash
     ./simv +UVM_TESTNAME=test +save
     ```

4. **Analyze 단계**
   - 커버리지 분석 및 디버깅 수행

#### VCS 내부 동작
- **Verilog Analyzer (`vlogan`)**
  - Verilog 파일을 논리 라이브러리로 파싱
  - 사용 옵션: `-y`, `-v`, `-f`, `+libext+.v` 등
- **VHDL Analyzer (`vhdlan`)**
  - VHDL 하향식 분석 및 configuration 단계 일부 elaboration 수행

#### 디버깅 관련 옵션
- `-debug_access`: 함수, 메모리, 스트렝스 추적
- `-debug_region`: 특정 지역만 디버깅
- **Reverse Debug**
  - Verdi > Preferences > Interactive Debug > Reverse Debug

---

## 2. Coverage
#### "현재 만든 TestBench가 어느정도의 영역을 체크하고 있는지"

### 커버리지 수집 및 분석
- 지원 메트릭: 조건, 분기, case, FSM 등
- **부분 커버리지 수집 가능**

### Verdi 커버리지 GUI 실행
```bash
verdi -cov -covdir ./simv.vdb
```

### URG를 이용한 커버리지 병합 예시
```bash
urg -metric line+cond+fsm+assert \
    -dir ./coverage/* \
    -dbname ./coverage/merge/rg1 \
    -noreport
```

---

## 3. Partition Compile
#### "컴파일 전체 한번에 몇일 걸릴 수도 있는데..."

### 기능 요약
- 3단계 컴파일 흐름 지원
- 파티션 단위로 병렬 컴파일 → 빌드 속도 향상
- 로그에서 파티션 메시지 확인 가능
- 자동 파티션 제어 가능

### 명령 예시
```bash
vcs -partcomp -pcmakeprof
```

### 출력 디렉토리 지정
- `-Mdir`: 기본 디렉토리 `csrc`

---

## 4. FGP (Fine Grained Parallelism)

### CPU 및 병렬처리 성능 확인
- CPU 정보 확인:
  ```bash
  cat /proc/cpuinfo
  grep "cpu cores" /proc/cpuinfo
  ```

- EPC 분석을 위한 실행:
  ```bash
  vcs <other-options>
  ./simv -Xdprof=timeline <other-options>
  ```

- **EPC (Event Per Cycle)** = CPU Time / Elapsed Time  
  → 높을수록 병렬처리 효율 우수

---

## 5. X-Prop

### RTL 설계의 X-propagation 검증

- **Merge Function 비교**
  - `tmerge`: 입력 중 a=1, b=1 → 결과 1
  - `xmerge`: 하나라도 x → 결과는 무조건 x
  - 디버깅 시 **xmerge 사용 권장**

---

## 6. Save & Restore

### 초기화 블록에서 설정
```systemverilog
if ($test$plusargs("save")) begin
   $save("test.chk");
end

$restart("test.chk");
```

### 실행 예시
- 저장 모드:
  ```bash
  ./simv +save
  ```
  → `$save` 호출 이후부터 실행됨

- 복원 모드:
  ```bash
  ./simv -r test.chk
  ```
=======
## 1. VCS Flow Overview

### Synopsys 제공 RTL 시뮬레이터: VCS

#### 기본 흐름
1. **Design/Testbench 작성**
   - RTL 및 UVM 기반 testbench 구성: DUT, Interface, Driver, Monitor 등 포함

2. **Compile 단계**
   - 명령어:  
     ```bash
     vcs [design files] [testbench files] <옵션>
     ```
   - 목적: 시뮬레이션 바이너리(`simv`) 생성
   - 예시:  
     ```bash
     vcs -full64 -debug_pp -sverilog top.sv tb_top.sv
     ```

3. **Simulate 단계**
   - 명령어:  
     ```bash
     ./simv +<옵션>
     ```
   - 예시:  
     ```bash
     ./simv +UVM_TESTNAME=test +save
     ```

4. **Analyze 단계**
   - 커버리지 분석 및 디버깅 수행

#### VCS 내부 동작
- **Verilog Analyzer (`vlogan`)**
  - Verilog 파일을 논리 라이브러리로 파싱
  - 사용 옵션: `-y`, `-v`, `-f`, `+libext+.v` 등
- **VHDL Analyzer (`vhdlan`)**
  - VHDL 하향식 분석 및 configuration 단계 일부 elaboration 수행

#### 디버깅 관련 옵션
- `-debug_access`: 함수, 메모리, 스트렝스 추적
- `-debug_region`: 특정 지역만 디버깅
- **Reverse Debug**
  - Verdi > Preferences > Interactive Debug > Reverse Debug

---

## 2. Coverage

### 커버리지 수집 및 분석
- 지원 메트릭: 조건, 분기, case, FSM 등
- **부분 커버리지 수집 가능**

### Verdi 커버리지 GUI 실행
```bash
verdi -cov -covdir ./simv.vdb
```

### URG를 이용한 커버리지 병합 예시
```bash
urg -metric line+cond+fsm+assert \
    -dir ./coverage/* \
    -dbname ./coverage/merge/rg1 \
    -noreport
```

---

## 3. Partition Compile

### 기능 요약
- 3단계 컴파일 흐름 지원
- 파티션 단위로 병렬 컴파일 → 빌드 속도 향상
- 로그에서 파티션 메시지 확인 가능
- 자동 파티션 제어 가능

### 명령 예시
```bash
vcs -partcomp -pcmakeprof
```

### 출력 디렉토리 지정
- `-Mdir`: 기본 디렉토리 `csrc`

---

## 4. FGP (Fine Grained Parallelism)

### CPU 및 병렬처리 성능 확인
- CPU 정보 확인:
  ```bash
  cat /proc/cpuinfo
  grep "cpu cores" /proc/cpuinfo
  ```

- EPC 분석을 위한 실행:
  ```bash
  vcs <other-options>
  ./simv -Xdprof=timeline <other-options>
  ```

- **EPC (Event Per Cycle)** = CPU Time / Elapsed Time  
  → 높을수록 병렬처리 효율 우수

---

## 5. X-Prop

### RTL 설계의 X-propagation 검증

- **Merge Function 비교**
  - `tmerge`: 입력 중 a=1, b=1 → 결과 1
  - `xmerge`: 하나라도 x → 결과는 무조건 x
  - 디버깅 시 **xmerge 사용 권장**

---

## 6. Save & Restore

### 초기화 블록에서 설정
```systemverilog
if ($test$plusargs("save")) begin
   $save("test.chk");
end

$restart("test.chk");
```

### 실행 예시
- 저장 모드:
  ```bash
  ./simv +save
  ```
  → `$save` 호출 이후부터 실행됨

- 복원 모드:
  ```bash
  ./simv -r test.chk
  ```
>>>>>>> 51f10cc5d1487f53b7a7be2da46da77c4d7439f5
  → `$save ~ $restart` 사이만 실행됨