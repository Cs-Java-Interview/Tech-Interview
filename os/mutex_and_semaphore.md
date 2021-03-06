# 뮤텍스와 세마포어의 차이
뮤텍스와 세마포어는 대표적인 병행성을 위한 기법이다.

## 세마포어
* 프로세스 간 시그널을 주고받기 위해 사용되는 정수 값.
* 세 가지 원자적 연산만을 지원한다.
  * 초기화 : 음이 아닌 값으로 초기화된다.
  * 값 증가(semSignal)
    * 값이 0이거나 음수이면 블록된 프로세스들을 깨운다.
  * 값 감소(semWait)
    * 값이 양수이면 값을 감소시키고 semWait을 호출한 프로세스는 계속 실행된다.
    * 값이 0 또는 음수이면 값을 감소시키고 semWait을 호출한 프로세스는 블락된다.
* 범용 세마포어와 이진 세마포어로 나눌 수 있다.
* 강성 세마포어와 약성 세마포어로 구분되기도 한다.
* 임계 영역에 여러 프로세스가 들어갈 수 있는 경우(공유변수가 여러 개)에도 상호 배제가 보장된다.

## 뮤텍스
* 객체를 얻거나 반납할 때 사용하는 프로그래밍 플래그.
* 0과 1만을 값으로 가진다.
* 임계 영역에 단 하나의 프로세스만 들어갈 수 있다.

## 뮤텍스와 세마포어의 차이
뮤텍스와 세마포어의 차이를 논할 때 세마포어는 이진 세마포어이다. [자세히](./semaphore.md)

* 뮤텍스는 락을 설정한 프로세스만 락을 해제할 수 있다.
* 이진 세마포어의 경우 락을 설정한 프로세스와 락을 해제하는 프로세스가 서로 다를 수 있다.