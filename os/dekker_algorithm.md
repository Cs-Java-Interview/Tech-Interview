# Dekker Algorithm
두 프로세스를 위한 상호 배제 알고리즘으로, Dekker에 의해 설계되고 Dijkstra가 보고하였다.
여러 시도를 거쳐 해결안을 단계별로 개발하였는데, 여기서는 그 단계별 해결안을 설명하고자 한다.

## 첫 번째 시도
* 하드웨어 내에서의 기본적 배제 기법 이용
* 한 메모리 위치에 대해 한 번에 하나의 접근만 허용해야 한다.

> 방법
1. `turn` 이라는 전역 메모리 변수를 이용
2. `turn` 값이 임계 영역을 수행하려는 프로세스 번호(PID)와 같을 경우에만 임계영역 수행 가능
3. 대기 중 `turn` 값을 계속 읽어야 한다. - busy waiting / spin waiting 방식
```
/* PROCESS 0 */               /* PROCESS 1 */

while (turn != 0)             while (turn != 1)
  /* do nothing */              /* do nothing */
/* critical section */        /* critical section */
turn = 1;                     turn = 0;
```

<br/>

> 단점
* 프로세스들이 임계영역을 번갈아가며 사용한다.
* 따라서 전체 속도가 두 프로세스 중 느린 프로세스의 속도에 맞춰진다.
* 한 프로세스가 임계영역 안에서 실패하거나, 심지어 임계영역 밖에서 실패할 경우에도 다른 프로세스는 영구히 기다려야 한다.

<br/>

## 두 번째 시도
> 첫 번째 시도의 문제점
* 두 프로세스의 상태 정보만 필요함에도 임계영역에 들어갈 프로세스의 번호를 저장해야 한다.
* 다른 프로세스가 실패할 경우에도 자신의 임게영역에 접근할 수 있도록 자신만의 키를 가져야 한다.


> 방법
1. boolean vector flag를 정의한다.
2. flag[0]은 PROCESS 0에, flag[1]은 PROCESS 1에 대응한다.
3. 다른 프로세스의 플래그 값이 false가 될 때까지 자신의 임계 영역에 진입할 수 없다.
4. 임계 영역에 진입하려는 프로세스는 다른 프로세스의 플래그 값을 읽어야 한다. - busy waiting / spin waiting 방식

```
/* PROCESS 0 */               /* PROCESS 1 */

while (flag[1])               while (flag[0])
  /* do nothing */              /* do nothing */
flag[0] = true;               flag[1] = true;
/* critical section */        /* critical section */
flag[0] = false;              flag[1] = false;
```

<br/>

> 단점
* 상호배제를 보장하지 못한다.
* 전체 실행 속도가 여전히 프로세스의 상대적인 속도에 영향을 받는다.
* 임계 영역 안에서 실패하는 프로세스가 있거나,
* 플래그를 true로 바꾼 후 임계영역에 들어가기 직전에 실패하는 경우 다른 프로세스가 영구히 블록된다.

<br/>

## 세 번째 시도
> 두 번째 시도의 문제점
* 한 프로세스가 다른 프로세스의 상태를 검사하고 자신의 임계영역에 들어가기 전, 다른 프로세스가 자신의 상태를 바꿀 수 있기 때문에 문제가 발생한다.

> 방법
1. 자신의 플래그를 true로 바꾼 후 다른 프로세스의 플래그 값을 검사한다.
2. 다른 프로세스의 플래그 값이 false가 될 때까지 자신의 임계 영역에 진입할 수 없다.
3. 임계 영역에 진입하려는 프로세스는 다른 프로세스의 플래그 값을 읽어야 한다. - busy waiting / spin waiting 방식

```
/* PROCESS 0 */               /* PROCESS 1 */

flag[0] = true;               flag[1] = true;
while (flag[1])               while (flag[0])
  /* do nothing */              /* do nothing */
/* critical section */        /* critical section */
flag[0] = false;              flag[1] = false;
```
<br/>

> 단점
* 한 프로세스가 플래그 값 설정 코드나 임계영역 안에서 실패하는 경우 다른 프로세스가 블록된다.
* 하지만 그 이외의 영역에서 실패할 경우 다른 프로세스가 블록되지 않는다.
* 두 프로세스가 while 문을 실행하기 전 자신들의 플래그를 각각 true로 설정하게 되면 교착상태(deadlock)가 야기된다.

<br/>

## 네 번째 시도
> 세 번째 시도의 문제점
* 다른 프로세스의 상태를 알지 못한 상태에서 자신의 상태를 설정하기 때문에,
* 각 프로세스는 서로가 자신의 임계 영역에 진입하려고 하기 때문에 교착 상태가 발생했다.

> 방식
1. 다른 프로세스가 임계 영역을 실행중일 경우 자신의 플래그 값을 false로 변경한다.
```
/* PROCESS 0 */               /* PROCESS 1 */

flag[0] = true;               flag[1] = true;
while (flag[1]) {             while (flag[0]) {
  flag[0] = false;              flag[1] = false;
  /* delay */                   /* delay */
  flag[0] = true;               flag[1] = true;
}                             }
/* critical section */        /* critical section */
flag[0] = false;              flag[1] = false;
```

<br/>

> 단점
* 두 프로세스가 서로 한 줄씩 비슷한 속도로 실행하는 경우 라이브락(livelock)이 발생할 수 있다. (상호우대)
* 이 경우, 두 프로세스의 상대적 속도에 변화가 있을 경우 반복 사이클이 깨져 한 프로세스가 임계영역에 들어갈 수 있기 때문에 교착상태가 아니며, 이를 라이브락이라 부른다.

<br/>

## 올바른 시도, Dekker 알고리즘
> 발상
* 상호 우대 문제를 피하기 위해 두 프로세스 간의 수행 순서를 정해야 한다.
* `turn` 변수를 '어느 프로세스가 임계 영역에 들어갈 차례인지 알려주는 용도'로 사용한다.

> 방식
1. `flag` 배열과 `turn` 변수를 이용한다.
2. 자신의 플래그를 true로 변경한다.
3. 다른 프로세스의 플래그가 true라면,
   1. `turn`이 1인지 검사한다.(다른 프로세스의 차례인지 검사한다.)
   2. 다른 프로세스의 차례라면, 자신의 플래그를 false로 변경한다.
   3. 자신에게 차례가 돌아올 때까지 기다린다.
   4. 자신의 차례인 경우 자신의 플래그를 true로 변경한다.
4. 임계영역을 실행한다.
5. 차례를 넘긴다.
6. 자신의 플래그를 false로 변경한다.
```
boolean flag[2];
int turn;
void P0() {
  while (true) {
    flag[0] = true;
    while (flag[1]) {
      if(turn == 1) {
        flag[[0] = false;
        while(turn == 1)
          /* do nothing */
        flag[0] = true;
      }
    }
    /* critical section */
    turn = 1;
    flag[0] = false;
    /* remainder */
  }
}
```

## 마치며

Dekker 알고리즘은 상호 배제 문제를 해결했지만,
이해하기가 어렵고 정확성을 증명하기가 까다로운 복잡한 프로그램이다. 
[Peterson 알고리즘](./peterson_algorithm.md)은 더 간단하면서도 우아한 해결책을 제안한다.