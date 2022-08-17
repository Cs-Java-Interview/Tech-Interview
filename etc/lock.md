## 락(Lock)이란

Lock을 획득한다는 것은 해당 자원을 사용한다는 것을 의미한다. 한 프로세스가 자원에 Lock을 걸면, 다른 프로세스는 해당 자원을 사용할 수 없다.

예를 들어 티켓팅 서비스에서, 한꺼번에 여러 사용자들의 요청이 오더라도 동시에 1개의 요청만 처리되어야 된다. 즉 요청이 순차적으로 처리되어야 한다.

이렇게 다수의 사용자가 동시에 동일한 요청을 하는 서비스의 경우, 공유자원에 대한 동기화 처리가 필요하다. 이때 Lock을 사용하여 동기화처리를 한다.


## 스핀락이란?

<aside>
  
    ✏️ 다른 스레드가 Lock을 소유하고 있다면, 그 Lock이 반환될 때까지 계속 루프를 돌면서 기다리는 동기화 방식이다.

</aside>

- Critical Section에 진입이 안될 경우, Context Switching을 하지 않고 루프를 돌면서 대기한다.
- Critical Section의 Lock과 Unlock과정이 짧은 경우, Context Switching하는 비용이 크기 때문에 잠시 대기 후 Critical Section에 진입하는 게 효율적인 경우 사용한다.
- for나 while 루프를 돌면서 락을 검사하는 방식으로 대기한다.
- 단점
    - Lock을 얻지 못하고 무한 루프를 돌면서 시간 낭비가 생길 수 있다.
    - 이를 B**usy waiting**이라 하며, CPU를 계속 사용하면서 루프가 돌기 때문에 다른 스레드가 CPU를 사용하지 못해 비효율적이다. (특히 싱글 코어인 경우 비효율적이다.)

## 분산락

<aside>
  
    ✏️ 여러 서버를 운영하는 분산 환경에서 사용하는 동기화 방식으로, 데이터베이스 단위에서 처리하는 동기화 방식이다. (Lock 체크를 매번 클라이언트가 하지 않아도 된다.)

</aside>

- 사용
    1. RDBMS에서 제공하는 락을 사용하는 방식
        - MySQL 데이터베이스에 락을 관리하는 테이블을 만들어 동기화하는 방식 → [참고 링크](https://techblog.woowahan.com/2631/)
    2. Redis를 이용한 방식 → [참고 링크](https://devroach.tistory.com/82)
    3. Zookeeper를 이용한 방식

### Reference

- [http://itnovice1.blogspot.com/2019/09/spin-lock.html](http://itnovice1.blogspot.com/2019/09/spin-lock.html)
- [https://happyer16.tistory.com/entry/레디스와-분산락](https://happyer16.tistory.com/entry/%EB%A0%88%EB%94%94%EC%8A%A4%EC%99%80-%EB%B6%84%EC%82%B0%EB%9D%BD)
- [https://kkambi.tistory.com/196](https://kkambi.tistory.com/196)
