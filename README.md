# FFT_Hardware
-----

## 파일 구조
- **VIVADO_RTL** : 최종 프로젝트 결과
- **module0_golden** : module0 완성본
- **module1_golden** : module1 완성본
- **etc** : 완성본 기타 모음
- **tb** : 사용한 testbench
- **markdown** : fft 관련 공부한 정리 내용

---
### 7/25 진행사항
- step0_0 simulation, verification
- bfly0_1 design
- twf_m0 rom, multiplyer design(verification X)
- CBFP design(verification X)
---
### 7/26 진행사항
- step0_1 disign complete, (rough verification)
---
### 7/27 진행사항 및 유의사항
- step간 merge시에 butterfly output이 그 step이 내보내는 butterfly control signal(다음 단의 din_valid)보다 한 클럭 delay 되어있음에 유의  
  즉, butterfly control signal을 1clk delay 시켜서 사용해야 함
- butterfly_2 design complete(rough verification)
- cbfp revision(error 아직 있음)
---
### 7/28 진행사항
- step0, step1 오류 확인 및 수정
- butterfly_2 design complete, cbfp re-design(block unit operation is not applied)
- cbfp revision
---
### 7/29 진행사항
- step0, step1 오류 확인 및 수정완료(main brench 내 모든 module 최신화->shift reg 변경이 그 사유)
- cbfp design complete, merge with former module
- step0~step2 merge complete(timing problem is not yet solved)
- step1_0 design(verification error detected)
- **necessray verification** -> module0 top(with golden)
---
### 8/03 프로젝트 완료 소감
- RTL deisgn, verification은 만족스러웠으나 연속적인 동작에서의 요구사항을 만족하지 못함
- FPGA에서 제대로된 값이 나오지 않는 이유는 최신화가 되지 않았기 때문?
- Synthesis는 마지막까지 제대로된 netlist가 산출되지 않았음
------

# FFT-DIF Fixed Point Design Project

본 프로젝트는 **Radix-2 DIF FFT 알고리즘**을 기반으로, Floating Point 설계를 Fixed Point로 변환하고  
**CBFP(Custom Block Floating Point)** 기법을 적용하여 성능을 개선한 결과물입니다.  
RTL 설계, Simulation, Synthesis, FPGA 구현까지 **Front-end Design Flow 전반**을 경험하며 학습한 내용을 정리한 문서입니다.  

---

## 프로젝트 개요
- **알고리즘 구조**
  - 3-stage Radix-2^2 Butterfly 기반
  - Module0: 512 → 64  
  - Module1: 64 → 8  
  - Module2: 8 → 1
- **목표**
  - Floating Point FFT → Fixed Point 변환
  - SQNR 개선 및 HW 자원 최적화
- **사용 툴**
  - 언어: SystemVerilog  
  - Simulation: Synopsys VCS  
  - Synthesis: Synopsys Design Compiler  
  - FPGA: Xilinx Vivado

---

## 1. Floating vs Fixed Point FFT
- **Floating Point FFT**
  - 고정밀도, 구현 용이
  - HW 리소스 소모 큼
- **Fixed Point FFT**
  - 자원 효율적
  - 비트 관리 필요 (rounding, saturation, CBFP)
- **비교 결과**
  - Cosine 입력: Floating과 Fixed 출력 유사
  - Fixed에서는 SQNR 관리 필요

---

## 2. CBFP (Custom Block Floating Point) 적용
- **SQNR 개선**
  - 미적용: 평균 27.37 dB, 최저 0 dB
  - 적용: 평균 41.49 dB, 최저 6 dB 이상
- **효과**
  - 변동폭 감소
  - Fixed Point에서도 Floating에 근접한 성능 확보

---

## 3. RTL 설계
- **구현 모듈**
  - Shift Register
  - Butterfly00 / 01 / 02
  - Step00, Step1_0, Step1_1, Step1_2
  - CBFP Module
- **Top 구조**
  - FFT_TOP → Module0 → Module1 → Module2

---

## 4. Simulation & Synthesis
- **Simulation**
  - VCS 기반 Cosine/Random 입력 테스트
  - Module 단위 및 전체 FFT 결과 검증
- **Synthesis**
  - Synopsys DC 기반 합성
  - Area Report 및 Netlist 확인
  - 합성 누락 모듈 추적 (din 포트 연결 오류 확인)

---

## 5. FPGA 구현
- **구조**
  - cos_gen, clk_wiz, FFT 모듈
- **Bitstream 생성**
  - Vivado 기반 FPGA 다운로드
- **Timing Report**
  - 512포인트 데이터가 32clk 동안 16개씩 출력
  - 8clk 간격의 텀 존재
  - 일부 구간에서 peak 경향성 확인

---

## 6. Troubleshooting & 고찰
- **합성 후 Area mismatch**
  - 일부 모듈 합성 누락 → Netlist 추적 및 포트 연결 점검
- **Shift Register 구조 변경**
  - 2차원 → 1차원 배열 변경 → 성능 차이 미미
- **출력 오차 분석**
  - 최소 64 단위 값 차이, 일부 구간 두 배 차이
  - 실수/허수 비대칭 발생
  - 원인: CBFP 처리 문제로 판단

---

## 결론
- Fixed Point FFT는 HW 자원 효율성이 높으나, 비트 관리 필수
- CBFP 기법 적용 시 SQNR이 크게 개선됨
- RTL → FPGA까지 전 과정을 수행하며 **합성, 타이밍, 디버깅 능력**을 강화
- 학습·발표와 실무형 설계를 동시에 경험하며 **Front-end 설계 역량** 확보

---

**팀원**: 김용호 · 김을중 · 김한벗 · 정현태

