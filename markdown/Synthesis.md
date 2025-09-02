<<<<<<< HEAD
# Design Compiler Synthesis 정리 문서

본 문서는 Synopsys Design Compiler(DC) 기반의 합성 과정에서 사용되는 주요 개념 및 SDC 명령어를 실무 중심으로 상세히 정리한 내용입니다.

---

## 기본 Synthesis Flow

1. **RTL 설계 작성**
2. **제약 조건(SDC) 설정**
3. **합성 실행**
   - `compile_ultra -no_autoungroup`
4. **보고서 확인 및 최적화 반복**
   - `report_timing`, `report_area`, `report_power` 등

---

##  주요 타이밍 개념

| 항목 | 설명 |
|------|------|
| **Setup Time** | 클럭 엣지 전에 신호가 안정적으로 도달해야 하는 최소 시간 |
| **Hold Time** | 클럭 엣지 후 신호가 유지되어야 하는 최소 시간 |
| **Recovery/Removal Time** | 비동기 신호(reset 등)의 안정성 보장 시간 |
| **Clock Skew** | 클럭 네트워크 내 가장 빠르고 느린 도착 시간 차 |
| **Critical Path** | 전체 회로에서 가장 큰 지연 경로로 Setup violation 발생 가능성이 높음 |

---

## 클럭 관련 명령어

### `create_clock`
```tcl
create_clock "CLK1" -period 10 -waveform {0 5.0}
```
- 100 MHz 클럭 생성, 0~5ns 구간은 High

### `set_clock_uncertainty`
```tcl
set_clock_uncertainty -setup 0.65 [get_clocks CLK1]
set_clock_uncertainty -hold 0.45 [get_clocks CLK1]
```
- Setup/Hold에 대한 클럭 여유시간 마진 설정

### `set_clock_transition`
```tcl
set_clock_transition 0.64 -fall [get_clocks CLK1]
```
- 클럭 fall edge의 트랜지션 시간 설정

---

## 일반 경로 제약

### `set_max_transition`
```tcl
set_max_transition 0.3 -clock_path [all_clocks]
```
- 최대 트랜지션 시간 제한 (0.3ns 이내)

### `set_max_fanout`
```tcl
set_max_fanout 64 [current_design]
```
- 로직 셀의 최대 fanout 개수 설정 → 게이트 드라이브 성능 제어

---

## 입출력 타이밍 제약

### `set_input_delay`
```tcl
set_input_delay -max 1.35 -clock {CLK1}
```
- 입력 신호가 클럭 기준으로 도달해야 하는 시간

### `set_output_delay`
```tcl
set_output_delay -max 1.0 -clock {CLK1}
```
- 출력 신호가 클럭 기준으로 나가야 하는 시간

---

## 특수 경로 제약

### `set_multicycle_path`
```tcl
set_multicycle_path 2 -from A -to B
```
- 데이터가 2개 클럭 사이클에 걸쳐 전송될 경우 설정  
- Setup/Hold 체크 기준을 변경하여 타이밍 완화

### `set_false_path`
```tcl
set_false_path -from A -to B
```
- 분석에서 제외할 경로 정의 (예: 테스트 로직, 리셋 회로 등)

---

## 실무 활용 팁

- 모든 제약 조건은 `.sdc` 파일로 작성하고 `read_sdc`로 불러옵니다.
- Fanout, transition, uncertainty 설정은 후공정(P&R)을 고려해야 합니다.
- False path와 Multicycle path는 기능 및 타이밍 분석 검토 후 사용해야 합니다.
- `report_timing -from [get_pins A] -to [get_pins B]` 명령으로 특정 경로 확인 가능
=======
# Design Compiler Synthesis 정리 문서

본 문서는 Synopsys Design Compiler(DC) 기반의 합성 과정에서 사용되는 주요 개념 및 SDC 명령어를 실무 중심으로 상세히 정리한 내용입니다.

---

## 기본 Synthesis Flow

1. **RTL 설계 작성**
2. **제약 조건(SDC) 설정**
3. **합성 실행**
   - `compile_ultra -no_autoungroup`
4. **보고서 확인 및 최적화 반복**
   - `report_timing`, `report_area`, `report_power` 등

---

##  주요 타이밍 개념

| 항목 | 설명 |
|------|------|
| **Setup Time** | 클럭 엣지 전에 신호가 안정적으로 도달해야 하는 최소 시간 |
| **Hold Time** | 클럭 엣지 후 신호가 유지되어야 하는 최소 시간 |
| **Recovery/Removal Time** | 비동기 신호(reset 등)의 안정성 보장 시간 |
| **Clock Skew** | 클럭 네트워크 내 가장 빠르고 느린 도착 시간 차 |
| **Critical Path** | 전체 회로에서 가장 큰 지연 경로로 Setup violation 발생 가능성이 높음 |

---

## 클럭 관련 명령어

### `create_clock`
```tcl
create_clock "CLK1" -period 10 -waveform {0 5.0}
```
- 100 MHz 클럭 생성, 0~5ns 구간은 High

### `set_clock_uncertainty`
```tcl
set_clock_uncertainty -setup 0.65 [get_clocks CLK1]
set_clock_uncertainty -hold 0.45 [get_clocks CLK1]
```
- Setup/Hold에 대한 클럭 여유시간 마진 설정

### `set_clock_transition`
```tcl
set_clock_transition 0.64 -fall [get_clocks CLK1]
```
- 클럭 fall edge의 트랜지션 시간 설정

---

## 일반 경로 제약

### `set_max_transition`
```tcl
set_max_transition 0.3 -clock_path [all_clocks]
```
- 최대 트랜지션 시간 제한 (0.3ns 이내)

### `set_max_fanout`
```tcl
set_max_fanout 64 [current_design]
```
- 로직 셀의 최대 fanout 개수 설정 → 게이트 드라이브 성능 제어

---

## 입출력 타이밍 제약

### `set_input_delay`
```tcl
set_input_delay -max 1.35 -clock {CLK1}
```
- 입력 신호가 클럭 기준으로 도달해야 하는 시간

### `set_output_delay`
```tcl
set_output_delay -max 1.0 -clock {CLK1}
```
- 출력 신호가 클럭 기준으로 나가야 하는 시간

---

## 특수 경로 제약

### `set_multicycle_path`
```tcl
set_multicycle_path 2 -from A -to B
```
- 데이터가 2개 클럭 사이클에 걸쳐 전송될 경우 설정  
- Setup/Hold 체크 기준을 변경하여 타이밍 완화

### `set_false_path`
```tcl
set_false_path -from A -to B
```
- 분석에서 제외할 경로 정의 (예: 테스트 로직, 리셋 회로 등)

---

## 실무 활용 팁

- 모든 제약 조건은 `.sdc` 파일로 작성하고 `read_sdc`로 불러옵니다.
- Fanout, transition, uncertainty 설정은 후공정(P&R)을 고려해야 합니다.
- False path와 Multicycle path는 기능 및 타이밍 분석 검토 후 사용해야 합니다.
- `report_timing -from [get_pins A] -to [get_pins B]` 명령으로 특정 경로 확인 가능
>>>>>>> 51f10cc5d1487f53b7a7be2da46da77c4d7439f5
