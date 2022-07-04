## **동기화와 비동기화의 차이**

- 동기화(Syncronous)
    - 어느 메소드가 실행하는 동안 다른 메소드의 실행을 불가능하게 하는 것을 의미한다.
    - 즉, 접근하는 스레드들이 순차적으로 진행한다. (순서를 지킨다)
    - 시간적인 동기화가 필요한 곳에 많이 사용 (ex 현금 인출기)
    - 다음에 실행될 명령은 현재 실행 중인 명령 종료 시까지 대기한다. (대기 시간 버퍼링 발생)
- 비동기화(Asyncronous)
    - 현재 실행 중인 명령이 종료되지 않아도 다음 명령 실행 가능하다.
    - 수신하는 동안 다른 작업을 시행 가능하여 버퍼링이 적다.
    - ex)  Ajax, 스레드 생성시

## Java에서 동기화

- `synchronized` 키워드 사용
- 공유자원에 멀티 스레드 접근을 제한함
- 메소드와 블록 단위로 적용 가능하다. → 단, 메소드 전체를 lock하기보다는 블록 단위 사용하는 것이 좋다.

ex) 동기화/비동기화 예시

```java
class User {
  private int userNo = 0;

  // 임계 영역이 되어야하는 메소드
	// synchronized 키워드 적용할 경우와 안할 경우 -> 각각의 결과가 다름
  public synchronized void add(String name) {
    System.out.println(name + " : " + userNo++);
  }

}

class UserThread extends Thread {
  User user;

  UserThread(User user, String name) {
    super(name);
    this.user = user;
  }

  public void run() {
    try {
      for (int i = 0; i < 3; i++) {
        user.add(getName());    // 임계영역
        sleep(500);
      }
    } catch(InterruptedException e) {
      System.err.println(e.getMessage());
    }
  }
}

public class SyncThread {
  public static void name(String[] args) {
    User user = new User();

    // 3개의 스레드 객체 생성
    UserThread p1 = new UserThread(user, "A");
    UserThread p2 = new UserThread(user, "B");
    UserThread p3 = new UserThread(user, "C");

    // 스레드 스케줄링 : 우선순위 부여
    p1.setPriority(p1.MAX_PRIORITY);
    p2.setPriority(p2.NORM_PRIORITY);
    p3.setPriority(p3.MIN_PRIORITY);

    p1.start();
    p2.start();
    p3.start();
  }
}
```

```
// case1 ) synchronized 키워드 안한 경우 
// 결과 => 순서 뒤죽박죽
A : 0번째 사용
B : 1번째 사용
C : 2번째 사용
A : 3번째 사용
C : 5번째 사용
B : 4번째 사용
C : 7번째 사용
A : 6번째 사용
B : 8번째 사용

// case2 ) synchronized 키워드 적용한 경우
// 결과
A : 0번째 사용
B : 1번째 사용
C : 2번째 사용
A : 3번째 사용
B : 5번째 사용
C : 4번째 사용
A : 7번째 사용
B : 6번째 사용
C : 8번째 사용
```

**Reference**

[https://huisam.tistory.com/entry/Synchronized-Asynchronized](https://huisam.tistory.com/entry/Synchronized-Asynchronized)
