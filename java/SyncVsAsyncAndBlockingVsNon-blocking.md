# sync(동기)vs async(비동기) / blocking vs non-blocking

**updated 2023.03.13**

## 정의

### Synchronous(동기) Vs Asynchronous(비동기)

- 동기와 비동기는 순서와 예측가능한 결과에 포인트가 있다.
- [동기와 비동기는 프로세스의 수행 순서 보장에 대한 매커니즘이다. 즉 처리해야 하는 작업들을 어떠한 흐름으로 처리할 것이냐에 관점이다.](https://jaehoney.tistory.com/242)

#### Synchronous(동기)

- 같을 동(同), 기약할 기(期) = 같은 상태를 기약(약속)함
- <img src="https://evan-moon.github.io/static/edf9c97710f3443a11147da87d5eb3e8/c08c5/synchronized-swimming.jpg"/>
- 동기 작업의 특징을 단어의 뜻과 연결해서 풀어보자면,
  - 스레드가 작업을 시작 할 때, 작업의 순서가 보장되어, 결과를 예측 할 수 있다. => 작업을 마치면 약속(예상)했던 상태로 업데이트 됨.
- [함수는 다른 함수의 리턴값을 고려해서 동작한다.](https://jaehoney.tistory.com/242)
- 동기 방식으로 프로그래밍 하면, 늘 동일한 결과를 보장한다.(고 생각함.)

#### Asynchronous(비동기)

- 아닐 비(非), 같을 동(同), 기약할 기(期) = 같은 상태를 기약(약속)하지 아니함
- 반대로 비동기 작업의 특징은
  - 스레드가 작업을 시작 할 때, 작업의 순서와 결과를 예측할 수 없어, 동일한 결과 값을 보장하지 못함.
- [함수는 다른 함수의 리턴 값을 고려하지 않고 동작한다.](https://jaehoney.tistory.com/242)
- 비동기 방식으로 프로그래밍 하면, 항상 동일한 결과가 나올 것이라 장담할 수 없음.

- 대표적인 [비동기 방식의 프로그래밍 구현](https://youtu.be/EJNBLD3X2yg?t=370)은 두가지가 있다.
  - multi-threads
  - non-block I/O

### blocking vs non-blocking

- 블록킹과 논블록킹은 작업을 다루는 스레드의 자율성([제어권](https://youtu.be/oEIoqGd-Sns?t=640))이 포인트다.

#### blocking

- 직역하자면 차단하기.
- 농구에서, 상대 선수의 진행을 신체적 접촉에 의해 방해하는 일 => 진행을 막는 것
- 스레드 기준으로 풀어보자면, 자신의 작업을 진행하다가, 다른 스레드 혹은 프로세스(ex. file system)에게 특정 작업을 부탁하면, 그 작업이 진행되는 동안 해당 스레드의 자율성이 막히는 상태가 되는 것. = 진행이 막힌채로 있는 것. 대기하는 것.

#### non-blocking

- 직역 : 비 차단.
- [다른 주체의 작업에 관련 없이 자신의 작업을 하는 것을 의미한다.](https://jaehoney.tistory.com/242)
- 스레드 기준으로 풀어보자면, 자신의 작업을 진행하다가, 다른 스레드 혹은 프로세스(ex. file system)에게 특정 작업을 부탁하고, 다시 자신의 작업을 막힘없이 진행하는 것.

## 관계 정의

- 유명한 표 from IBM
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F9947B63359A87B3933"/>

- ['시간'이라는 자원을 가장 효율적으로 사용한 경우는 무엇일까요?](https://haneepark.github.io/2021/07/18/blocking-nonblocking-sync-async/)
  <img src="https://haneepark.github.io/images/blocing-nonblocking-sync-async.png"/>

### 관계 조합별로 알아보자.

#### Synchronous & blocking

- **작업의 순서가 정해져있어 결과를 예측가능한 & 차단되는 상태**
  - 예) 한 사람이 자료를 스캔하고 나서 PPT를 작성할 때.
  - Java의 hashTable을 조작할 때 와 비슷할 것 같음.
  - 높은 동시성 보다 스레드 세이프 해야하는 작업시 유용 할 것 같음.

#### Synchronous & non-blocking

- **작업의 순서가 정해져있어 결과를 예측가능한 & 막힘없는 상태**
  - 예) 한 사람이 스캐너를 작동시켜 놓고나서 (스캔이 되는 동안), PPT를 작성할 때. (스캔이 다 되었나 중간 중간 확인, 또는 알림음을 들을 수 있음)

#### Asynchronous & blocking

- **작업의 순서와 결과를 예측할 수 없는 & 차단되는 상태**
  - 예) 둘 이상의 사람이 업무를 나눠 하되 각자 스캔하고 나서(대기 후) PPT작성 하는 경우.

#### Asynchronous & non-blocking

- **작업의 순서와 결과를 예측할 수 없는 & 막힘없는 상태**
  - 예) 둘 이상 사람이 업무를 하되, 자신의 담당업무가 아니면 다른 사람에게 맡기고 자신의 일을 계속하는 경우. (단 맡긴 업무가 완료 되면 알림 받음)
  - Node.js의 이벤트 루프가 그러한것 같음.
