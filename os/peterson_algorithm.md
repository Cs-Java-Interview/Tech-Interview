# Peterson Algorithm
두 프로세스를 위한 상호 배제 알고리즘이다.

> 필요한 공간들
* flag : 전역 배열로, index가 각 프로세스 번호에 대응한다.
* turn : 전역 변수로, 병행 수행에 의한 충돌을 해결해 준다. (차례 표시)

<br/>


> 방법
1. 자신의 플래그를 true로 변경한다.
2. turn을 다른 프로세스에 양보한다.
3. 다른 프로세스가 임계영역에 있고 다른 프로세스의 차례이기도 하다면,
   1. 아무것도 하지 않으면서
   2. 다른 프로세스가 임계영역 실행을 마치거나,
   3. 자신의 차례가 되기를(turn == 0) 기다린다.
4. 임계 영역을 실행한다.
5. 자신의 플래그를 false로 변경한다.


```
boolean falg[2];
int turn;
void P0() {
  while (true) {
    flag[0] = true;
    turn = 1;
    while (flag[1] && turn == 1)
      /* do nothing */
    /* critical section */
    flag[0] = false;
  }
}

void main() {
  flag[0] = false;
  flag[1] = false;
  parbegin(P0, P1);
}
```

> 장점
* 상호 배제를 보장한다.
* 어느 한 프로세스가 임계 영역을 독점하지 않는다.