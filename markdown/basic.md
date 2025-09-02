<<<<<<< HEAD

# SystemVerilog 기본 문법 정리 (2024.06.05 ~ 06.09)

## 1. DUT (Design Under Test)
### 1.1. Packet Structure
- 데이터 전송 단위인 패킷을 구조화하여 설계
- 일반적으로 `typedef struct` 형태로 정의하여 TB 및 DUT 간 전송

### 1.2. Reset Signal
- `reset_n` : Active-Low Reset 신호
- 비동기 리셋 처리 시 `@(negedge reset_n)` 방식 사용

## 2. Verification Environment in SystemVerilog
### 2.1. Verification
- S.V에서는 다양한 기능을 통해 모든 상황에 대한 완전한 검증 가능
- Assertion, Covergroup, Constrained Random, Functional Coverage 등 포함

### 2.2. Test 환경 구성
- 기본 구조:
  DUT → (sample) → TB
  TB  → (drive)  → DUT

- 구성 요소:
  - Program (Testbench): 시뮬레이션 시나리오 실행
  - Interface: 신호/타이밍 정의 및 DUT와 TB 연결
  - DUT: 실제 설계 대상 모듈

### 2.3. Interface 구성
- Clocking Block: TB의 샘플링/드라이빙 시점 정의
- Modport: DUT ↔ TB 간 인터페이스 접근 권한 정의

```systemverilog
interface my_if(input logic clk);
    logic a, b;
    modport TB (input a, output b);
    modport DUT (output a, input b);
endinterface
```

- Program Block
  - `initial`, `task`, `fork`, `join` 등 사용
  - `task`: 시간 지연 포함 가능, 반환값 없음
  - `automatic` ↔ `static`

## 3. SystemVerilog Language Basics
### 3.1. 2-State Data Types
- bit, byte, shortint, int, longint, real, shortreal, realtime

### 3.2. 4-State Data Types
- reg, logic(unsigned), integer, time

### 3.3. String Data Type
- `string s = "hello";`

### 3.4. Enumerated Data Types
```systemverilog
typedef enum {IDLE=0, RUN, STOP} state_e;
```

### 3.5. Fixed-size Arrays
```systemverilog
bit [31:0] c[2][3] = `{'{3,7,1}, '{5,1,9}};
$display("%0d", $dimensions(c)); // 출력: 3
```

### 3.6. Data Arrays
- Dynamic Arrays, Queues, Associative Arrays (first, next, prev, last)

### 3.7. Array Methods
- find, [$], item, addr

### 3.8. struct, rand, $urandom, typedef

### 3.9. Operators
- inside, iff

### 3.10. Procedures
- task: 시간 지연 및 입출력 포함 가능
- function: 시간 지연 불가, 값 반환

### 3.11. DPI-C
- C 함수와 S.V 함수 간 연결 가능
- sv 에서 import "DPI-C" 해주고 
  - vcs -full64 -severilog <sv파일> <c파일> 해주면 완료!

## 4. Concurrency (병렬 처리)
### 4.1. Thread
- fork 블록 내에서 병렬 실행
- 동시성 확보, 실행 순서 주의

### 4.2. Fork Variants
- fork - join: 모든 스레드 완료 후 진행
- fork - join_any: 하나라도 완료되면 진행
- fork - join_none: 백그라운드 실행, 이후 wait fork로 제어 가능   

## 5. Class   
### 5.1. OOP : Object Oriented Programming
- class: members = properties + methods
  - 예시: class packet(packet이라는 이름의 class 선언) -> packet pkt(pkt라는 이름의 packet)
- new(): 초기화를 통해 memory를 만들어주기
  - 예시: pkt = new();
- access: pkt.sa = 3;  packet class 안에 있는 sa(property)에 3 설정
- this : 현재 객체 자신을 참조하는 포인터 (vs super)
- Data hiding: class 안에 선언되어 있는 properties를 program와 같은 외부에서 직접적으로 수정할 수 없다
- local vs protected
- Handle Assignment : pkt1 = pkt2 -> pkt1이라는 핸들을 똑 떼다 pkt2가 붙어있는 memory에 같이 붙이는 것, 기존 pkt1이 붙어있던 memory는 garbage가 된다
  - pkt1_copy = pkt1.copy();
- typedef 기존타입 새이름;
  - 기본 타입에 이름 부여
  - struct, enum, class 정의
- extern: 선언과 정의 분리, class 밖에서 정의된 method를 class 내부로 가져와 사용
- :: -> scope resolution operator, 클래스 이름이나 패키지 이름의 스코프를 명시하여 외부 정의된 멤버/함수를 참조할때 사용
- package: 컨테이너
  - import package::*  //package 불러와서 전부(*) 사용하겠다

## 6. Randomization
### 6.1. Difficult to reach Corner-case Verification
- Randomization: randomize, rand, randc
  - rand(주사위 던지기), randc(카드뽑기)로 선언 후 randomize로 호출
  - constraint: 제약 조건, == 사용
    - constraint_mode(0|1): 1이 default, 0으로 해서 통제가능
    - dist: sa dist {[5:7]:=30, 9:=20}; //5,6,7 have weight of 30 each, 9 has weight of 20
    - solve, lnline, soft
  - inside: set membership
- randomize: pre_randomize, post_randomize
  - pre_randomize : rand_mode(0|1)
- std::randomize()
- seed change: %simv <other_opts> +ntb_random_seed=123
  - $get_initial_random_seed();를 통해 특정 포인트에서 로그를 가져오고
    - %simv <other_opts> -l simv.log : 해당 로그를 이용해 집중적으로 관리 가능

## 7. Class Inheritance
### 7.1. Inheritance
- 원본 base class 를 이용해 내가 원하는 내용을 추가 할 수 있다. 
  - class (내꺼) extends (원본)
  - this.(내꺼) = super.(원본);
  - handle assign : base = extend : 원본 -> extend로 붙음

### 7.2. Polymorphism: 다형성
- base에 있는 functiond은 virtual 형태로, properties는 protected
  - extends는 super를 통해 assign 
- argument 형태가 일치해야 한다

## 8. Inter - Thread Communications
### 8.1. Event
- event , triggered
  1. event A : input A 처럼 선언
  2. fork-join : 스레드 생성
  3. task1 : 안에서 wait(A1.triggered); 
  4. task2 : 해당 task 작업 끝나면 task1으로

### 8.2. Resource Sharing ITC : Semaphores
- put, get, try_get
- 전부 비어있을 때 
  - task 에서 get을 실행 시 pending
  - function 에서 try_get 실행 시 0 뱉고 진행

| 항목            | task                          | function                         |
|-----------------|-------------------------------|----------------------------------|
| 시간 흐름       | O (시간 지연 가능)            | X (시간 지연 불가능)            |
| 반환값         | X (return value 없음)         | O (1개 반환값 필수)             |
| 호출 방식       | blocking                      | non-blocking (combinational)    |
| 목적            | 순차적 동작 수행              | 계산/연산 결과 반환             |
| 사용 위치       | initial, always, program 등   | 표현식(expression) 내 사용 가능 |
| 실행 시간       | 여러 시뮬레이션 사이클 가능   | 단일 시뮬레이션 사이클 내 수행  |

- semaphore는 사용 전에 new로 초기화 진행 필수

### 8.3. Mailbox
- put, get, peek
- try_put : return value는 넣은 개수
- Message 개수 할당

### 8.4. Coverage
- bin(test case)을 어떻게 잡느냐 즉, coverpoint를 어떻게 설정하느냐에 따라 coverage
- covergroup, coverpoint
- covergroup @() 이벤트 확인해서 coverpoint 설정해주고
  - option.auto_bin_max 가 default 64이며 넘으면 반드시 설정
- coverpoint -> bin -> wildcard, with
=======

# SystemVerilog 기본 문법 정리 (2024.06.05 ~ 06.09)

## 1. DUT (Design Under Test)
### 1.1. Packet Structure
- 데이터 전송 단위인 패킷을 구조화하여 설계
- 일반적으로 `typedef struct` 형태로 정의하여 TB 및 DUT 간 전송

### 1.2. Reset Signal
- `reset_n` : Active-Low Reset 신호
- 비동기 리셋 처리 시 `@(negedge reset_n)` 방식 사용

## 2. Verification Environment in SystemVerilog
### 2.1. Verification
- S.V에서는 다양한 기능을 통해 모든 상황에 대한 완전한 검증 가능
- Assertion, Covergroup, Constrained Random, Functional Coverage 등 포함

### 2.2. Test 환경 구성
- 기본 구조:
  DUT → (sample) → TB
  TB  → (drive)  → DUT

- 구성 요소:
  - Program (Testbench): 시뮬레이션 시나리오 실행
  - Interface: 신호/타이밍 정의 및 DUT와 TB 연결
  - DUT: 실제 설계 대상 모듈

### 2.3. Interface 구성
- Clocking Block: TB의 샘플링/드라이빙 시점 정의
- Modport: DUT ↔ TB 간 인터페이스 접근 권한 정의

```systemverilog
interface my_if(input logic clk);
    logic a, b;
    modport TB (input a, output b);
    modport DUT (output a, input b);
endinterface
```

- Program Block
  - `initial`, `task`, `fork`, `join` 등 사용
  - `task`: 시간 지연 포함 가능, 반환값 없음
  - `automatic` ↔ `static`

## 3. SystemVerilog Language Basics
### 3.1. 2-State Data Types
- bit, byte, shortint, int, longint, real, shortreal, realtime

### 3.2. 4-State Data Types
- reg, logic(unsigned), integer, time

### 3.3. String Data Type
- `string s = "hello";`

### 3.4. Enumerated Data Types
```systemverilog
typedef enum {IDLE=0, RUN, STOP} state_e;
```

### 3.5. Fixed-size Arrays
```systemverilog
bit [31:0] c[2][3] = `{'{3,7,1}, '{5,1,9}};
$display("%0d", $dimensions(c)); // 출력: 3
```

### 3.6. Data Arrays
- Dynamic Arrays, Queues, Associative Arrays (first, next, prev, last)

### 3.7. Array Methods
- find, [$], item, addr

### 3.8. struct, rand, $urandom, typedef

### 3.9. Operators
- inside, iff

### 3.10. Procedures
- task: 시간 지연 및 입출력 포함 가능
- function: 시간 지연 불가, 값 반환

### 3.11. DPI-C
- C 함수와 S.V 함수 간 연결 가능
- sv 에서 import "DPI-C" 해주고 
  - vcs -full64 -severilog <sv파일> <c파일> 해주면 완료!

## 4. Concurrency (병렬 처리)
### 4.1. Thread
- fork 블록 내에서 병렬 실행
- 동시성 확보, 실행 순서 주의

### 4.2. Fork Variants
- fork - join: 모든 스레드 완료 후 진행
- fork - join_any: 하나라도 완료되면 진행
- fork - join_none: 백그라운드 실행, 이후 wait fork로 제어 가능   

## 5. Class   
### 5.1. OOP : Object Oriented Programming
- class: members = properties + methods
  - 예시: class packet(packet이라는 이름의 class 선언) -> packet pkt(pkt라는 이름의 packet)
- new(): 초기화를 통해 memory를 만들어주기
  - 예시: pkt = new();
- access: pkt.sa = 3;  packet class 안에 있는 sa(property)에 3 설정
- this : 현재 객체 자신을 참조하는 포인터 (vs super)
- Data hiding: class 안에 선언되어 있는 properties를 program와 같은 외부에서 직접적으로 수정할 수 없다
- local vs protected
- Handle Assignment : pkt1 = pkt2 -> pkt1이라는 핸들을 똑 떼다 pkt2가 붙어있는 memory에 같이 붙이는 것, 기존 pkt1이 붙어있던 memory는 garbage가 된다
  - pkt1_copy = pkt1.copy();
- typedef 기존타입 새이름;
  - 기본 타입에 이름 부여
  - struct, enum, class 정의
- extern: 선언과 정의 분리, class 밖에서 정의된 method를 class 내부로 가져와 사용
- :: -> scope resolution operator, 클래스 이름이나 패키지 이름의 스코프를 명시하여 외부 정의된 멤버/함수를 참조할때 사용
- package: 컨테이너
  - import package::*  //package 불러와서 전부(*) 사용하겠다

## 6. Randomization
### 6.1. Difficult to reach Corner-case Verification
- Randomization: randomize, rand, randc
  - rand(주사위 던지기), randc(카드뽑기)로 선언 후 randomize로 호출
  - constraint: 제약 조건, == 사용
    - constraint_mode(0|1): 1이 default, 0으로 해서 통제가능
    - dist: sa dist {[5:7]:=30, 9:=20}; //5,6,7 have weight of 30 each, 9 has weight of 20
    - solve, lnline, soft
  - inside: set membership
- randomize: pre_randomize, post_randomize
  - pre_randomize : rand_mode(0|1)
- std::randomize()
- seed change: %simv <other_opts> +ntb_random_seed=123
  - $get_initial_random_seed();를 통해 특정 포인트에서 로그를 가져오고
    - %simv <other_opts> -l simv.log : 해당 로그를 이용해 집중적으로 관리 가능

## 7. Class Inheritance
### 7.1. Inheritance
- 원본 base class 를 이용해 내가 원하는 내용을 추가 할 수 있다. 
  - class (내꺼) extends (원본)
  - this.(내꺼) = super.(원본);
  - handle assign : base = extend : 원본 -> extend로 붙음

### 7.2. Polymorphism: 다형성
- base에 있는 functiond은 virtual 형태로, properties는 protected
  - extends는 super를 통해 assign 
- argument 형태가 일치해야 한다

## 8. Inter - Thread Communications
### 8.1. Event
- event , triggered
  1. event A : input A 처럼 선언
  2. fork-join : 스레드 생성
  3. task1 : 안에서 wait(A1.triggered); 
  4. task2 : 해당 task 작업 끝나면 task1으로

### 8.2. Resource Sharing ITC : Semaphores
- put, get, try_get
- 전부 비어있을 때 
  - task 에서 get을 실행 시 pending
  - function 에서 try_get 실행 시 0 뱉고 진행

| 항목            | task                          | function                         |
|-----------------|-------------------------------|----------------------------------|
| 시간 흐름       | O (시간 지연 가능)            | X (시간 지연 불가능)            |
| 반환값         | X (return value 없음)         | O (1개 반환값 필수)             |
| 호출 방식       | blocking                      | non-blocking (combinational)    |
| 목적            | 순차적 동작 수행              | 계산/연산 결과 반환             |
| 사용 위치       | initial, always, program 등   | 표현식(expression) 내 사용 가능 |
| 실행 시간       | 여러 시뮬레이션 사이클 가능   | 단일 시뮬레이션 사이클 내 수행  |

- semaphore는 사용 전에 new로 초기화 진행 필수

### 8.3. Mailbox
- put, get, peek
- try_put : return value는 넣은 개수
- Message 개수 할당

### 8.4. Coverage
- bin(test case)을 어떻게 잡느냐 즉, coverpoint를 어떻게 설정하느냐에 따라 coverage
- covergroup, coverpoint
- covergroup @() 이벤트 확인해서 coverpoint 설정해주고
  - option.auto_bin_max 가 default 64이며 넘으면 반드시 설정
- coverpoint -> bin -> wildcard, with
>>>>>>> 51f10cc5d1487f53b7a7be2da46da77c4d7439f5
- $get_coverage