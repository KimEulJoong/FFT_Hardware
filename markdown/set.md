<<<<<<< HEAD
# MobaXterm + Vi Editor + Verilog 작업 환경

## Vi Editor 기본 사용법 및 명령어

---

## 초기 세팅

1. 사용자 설정 파일 가져오기:
    - `.alias`: 사용자 정의 명령어 설정
    - `.cshrc`: 셸 초기화 설정

2. 파일 생성 및 코딩:
    ```bash
    vi <filename>.v
    # 또는
    gvim <filename>.v
    ```

3. 컴파일:
    ```bash
    vcs -full64 -kdb -debug_access+all -f <filelist>
    ```
    - `-full64`: 64비트 모드
    - `-kdb`: 디버그 정보 포함
    - `-debug_access+all`: 모든 신호 디버깅 가능
    - `-f <filelist>`: 파일 리스트를 통한 일괄 컴파일

4. 시뮬레이션 실행 파일:
    - `simv` : 생성된 실행 바이너리
    - `csrc/` : C 기반 내부 컴파일 디렉토리
    - `simv.daidir/` : 디버깅 정보 디렉토리

5. 시뮬레이션 실행:
    ```bash
    ./simv -verdi &
    ```

6. 파일리스트 관리:
    - `cnt_filelist` 내부에 모듈 파일을 `./모듈명` 형태로 등록하여 일괄 컴파일

7. 실행 스크립트 사용:
    - `run` 파일 생성 후 실행
    ```bash
    chmod +x run
    ./run
    ```

---

## 시뮬레이션 추가 팁

1. **Verdi 재시작 없이 수정 반영**:
    - Verdi > Simulate > *Rebuild and Restart* > *VCS to Rebuild*

2. **초기 딜레이 설정**:
    - Testbench에서 `#100;` 등으로 초기 상태 안정화

---

## Verilog 이론 체크포인트

1. **동기(Sync) / 비동기(Async)** 여부 확인:
    - 이에 따라 블록(`@posedge`, `@negedge`) 사용과
      블로킹(`=`) / 논블로킹(`<=`) 결정

2. **데이터 타입 구분**:
    - **2-state** 타입: `bit`, `logic`
    - **4-state** 타입: `reg`, `wire` (`0`, `1`, `x`, `z` 포함)

---

=======
# MobaXterm + Vi Editor + Verilog 작업 환경

## Vi Editor 기본 사용법 및 명령어

---

## 초기 세팅

1. 사용자 설정 파일 가져오기:
    - `.alias`: 사용자 정의 명령어 설정
    - `.cshrc`: 셸 초기화 설정

2. 파일 생성 및 코딩:
    ```bash
    vi <filename>.v
    # 또는
    gvim <filename>.v
    ```

3. 컴파일:
    ```bash
    vcs -full64 -kdb -debug_access+all -f <filelist>
    ```
    - `-full64`: 64비트 모드
    - `-kdb`: 디버그 정보 포함
    - `-debug_access+all`: 모든 신호 디버깅 가능
    - `-f <filelist>`: 파일 리스트를 통한 일괄 컴파일

4. 시뮬레이션 실행 파일:
    - `simv` : 생성된 실행 바이너리
    - `csrc/` : C 기반 내부 컴파일 디렉토리
    - `simv.daidir/` : 디버깅 정보 디렉토리

5. 시뮬레이션 실행:
    ```bash
    ./simv -verdi &
    ```

6. 파일리스트 관리:
    - `cnt_filelist` 내부에 모듈 파일을 `./모듈명` 형태로 등록하여 일괄 컴파일

7. 실행 스크립트 사용:
    - `run` 파일 생성 후 실행
    ```bash
    chmod +x run
    ./run
    ```

---

## 시뮬레이션 추가 팁

1. **Verdi 재시작 없이 수정 반영**:
    - Verdi > Simulate > *Rebuild and Restart* > *VCS to Rebuild*

2. **초기 딜레이 설정**:
    - Testbench에서 `#100;` 등으로 초기 상태 안정화

---

## Verilog 이론 체크포인트

1. **동기(Sync) / 비동기(Async)** 여부 확인:
    - 이에 따라 블록(`@posedge`, `@negedge`) 사용과
      블로킹(`=`) / 논블로킹(`<=`) 결정

2. **데이터 타입 구분**:
    - **2-state** 타입: `bit`, `logic`
    - **4-state** 타입: `reg`, `wire` (`0`, `1`, `x`, `z` 포함)

---

>>>>>>> 51f10cc5d1487f53b7a7be2da46da77c4d7439f5
