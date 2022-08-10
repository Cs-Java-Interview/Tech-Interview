## **Singleton 패턴**

<aside>
  
    💡 객체를 하나만 생성 하도록 하며, 생성된 객체를 어디에서든지 참조할 수 있도록 하는 패턴

</aside>

- 생성(Creational) 패턴 종류 중 하나이다. (객체 생성에 관련된 패턴)
- 클래스에서 하나뿐인 인스턴스를 관리하고, 인스턴스로 추가로 만들지 못하도록 막아야 한다
- 어디에서든지 참조할 수 있도록(전역 접근 가능) 해야함
- 싱글턴은 게으른 방식(초기에 생성되는 것이 아닌 싱글턴으로 구현하려는 객체가 필요할 때 생성하는 방법)으로 구현할 수 도 있는데, 이 방식은 싱글턴패턴으로 적용된 인스턴스가 자원을 많이 잡아먹는다면 이런 기법이 유용함

## **Singleton 패턴** 사용하는 이유

<aside>
  
    💡 메모리에 한번만 할당된다는 장점이 있다.

</aside>

- 최초로 메모리 할당 후에는 이미 생성된 인스턴스를 활용하여, 메모리/속도 측면에서 효율적이다.
- 또한 데이터 공유가 쉽다. (싱글톤 인스턴스가 전역에서 접근가능하기 떄문에) 다만, 동시성 문제가 발생할 수 있다.


**예시**

```java
public class Printer {

  private Printer() { }   // Printer 생성자를 외부에서 사용 불가 -> 싱글톤

    public static Printer getPrinter(){   // 어디서든 외부에서 접근이 가능하도록!
    if (printer == null) {
      printer = new Printer();
    }
    return printer;
  }
}
```

## **Singleton 패턴의** 문제점

- 멀티 스레드 환경에서 동기화 처리를 하지 않았을 때, 인스턴스가 2개가 생성되는 문제도 발생할 수 있다.
- 테스트하기 어렵다 → 자원을 공유하고 있기에 테스트를 수행하려면 매번 인스턴스 상태를 초기화해주어야 한다.
- 클라이언트가 구체적인 클래스에 의존하게 된다. → SOLID 원칙 중 DIP를 위반

> ❗🔎 다중 스레드에서는 싱글톤 인스턴스에 동시에 여러 스레드가 접근하는 Race Condition 문제가 발생할 수 있다.
(Race Condition : 메모리와 같은 동일한 자원을 2개 이상의 스레드가 이용하려고 경합하는 현상)
> => 멀티스레딩 환경에서 발생할 수 있는 동시성 문제 해결을 위해 syncronized 키워드를 사용해야 한다.
> 

## 멀티 스레딩 문제 해결방법

1. 정적 변수에 인스턴스를 만들어 바로 초기화하는 방법 (Eager Initialization)
    - 멀티 스레드 환경에서 동기화 처리를 하지 않았을 때, 인스턴스가 2개가 생성되는 문제도 발생할 수 있는데, 이러한 문제를 해결하기 위한 방법
    - 인스턴스가 필요할 때 생성하지 말고 처음부터 만드는 방법
    - 어플리테이션 실행 중에 생성하고 관리하지 않고 JVM에서 클래스 로딩될 때 생성
    
    ```java
    public class Printer {
       // static 변수에 외부에 제공할 자기 자신의 인스턴스를 만들어 초기화
       private static Printer printer = new Printer();
       private Printer() { }
       // 자기 자신의 인스턴스를 외부에 제공
       public static Printer getPrinter(){
         return printer;
       }
       public void print(String str) {
         System.out.println(str);
       }
    }
    ```
    
2. 인스턴스를 만드는 메서드에 동기화하는 방법 (Thread-Safe Initialization)
    - `synchronized` 키워드를 활용하여 동기화하기! → getInstance()에 Lock을 하는 방식이라 속도가 느리다.
    
    ```java
    public class Printer {
       // 외부에 제공할 자기 자신의 인스턴스
       private static Printer printer = null;
       private int counter = 0;
       private Printer() { }
       // 인스턴스를 만드는 메서드 동기화 (임계 구역)
       public synchronized static Printer getPrinter(){
         if (printer == null) {
           printer = new Printer(); // Printer 인스턴스 생성
         }
         return printer;
       }
       public void print(String str) {
         // 오직 하나의 스레드만 접근을 허용함 (임계 구역)
         // 성능을 위해 필요한 부분만을 임계 구역으로 설정한다.
         synchronized(this) {
           counter++;
           System.out.println(str + counter);
         }
       }
    }
    ```
    

**Reference**

[https://tecoble.techcourse.co.kr/post/2020-11-07-singleton/](https://tecoble.techcourse.co.kr/post/2020-11-07-singleton/)

[https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html](https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html)
