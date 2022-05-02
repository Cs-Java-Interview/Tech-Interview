## 동기와 비동기

### 동기(Synchronous: 동시에 일어나는)

`동기`는 말 그대로 <U>동시에 일어난다</U>는 뜻이다. 요청과 그 결과가 동시에 일어난다는 약속이라고 할 수 있는데, 즉 요청을 하면 시간이 얼마나 걸리던지 간에 요청한 자리에서 바로 결과가 주어져야 한다.

### 비동기(Asynchronous: 동시에 일어나지 않는)

`비동기`는 동시에 일어나지 않는다는 뜻이다. 요청에 따른 결과가 동시에 일어나지 않을 것이라는 약속이라고 할 수 있다.

### 동기와 비동기의 차이

`동기`와 `비동기`의 차이점을 비유적으로 표현하면 다음과 같다.

> 현재 해야할 일이 빨래, 설거지, 청소가 있다고 가정한다. 이 일들을 `동기적`으로 처리한다면 내가 정해둔 순서대로 빨래를 한 다음 설거지를 하고 청소를 할 것이다. 그런데 이 일을 `비동기적`으로 처리한다면 빨래 업체에게 빨래를 시키고, 설거지 대행 업체게 설거지를 시키고, 청소 대행 업체에 청소를 시킨다. 이 때, 어느 작업이 먼저 완료될지는 알 수 없다. 그리고 일을 맡긴 다음에는 나는 다른 업무를 수행할 수 있게된다.

일반적으로 프로그래밍에서 `동기`와 `비동기`의 차이는 메서드를 실행시킴과 동시에 반환값이 기대되는 경우에 `동기`라고 표현하고, 그렇지 않은 경우에 대해서는 `비동기`라고 표현한다. 동시에 라는것은 실행되었을 때 값이 반환되기 전까지, 즉 메서드가 종료되기 전까지는 `blocking` 되어있다는 것을 의미한다. `비동기`의 경우, `blocking` 되지 않고 이벤트 큐에 넣거나 백그라운드 스레드에게 해당 작업을 위임하고 바로 다음코드를 실행하기 때문에 기대되는 값이 바로 다음코드를 실행하기 때문에 기대되는 값이 바로 반환되지 않는다.

> `블록(blocking)`과 `논블록(nonblocking)`의 차이를 간략하게 설명하자면, 학생이 시험지를 선생에게 건넨 후 가만히 앉아 채점이 끝나고 시험지를 돌려받기만을 기다리고 있다면 학생을 `블록(blocking)`상태라고 할 수 있다. 하지만 학생이 시험지를 건넨 후 선생에게 채점이 완료되었다는 전송을 받기전까지 다른 과목을 공부한다거나 게임을 한다거나 다른 일을 하게 된다면 학생은 `논블록(nonblocking)`상태라고 할 수 있다.