# 람다(Lambda)

### 람다 함수란?

람다 함수는 프로그래밍 언어에서 사용되는 개념으로 익명 함수(Anonymous functions)를 지칭하는 용어입니다. 현재 사용되고 있는 람다의 근간은 수학과 기초 컴퓨터과학 분야에서의 람다 대수이다. 람다 대수는 간단히 말하면 수학에서 사용하는 함수를 보다 단순하게 표현하는 방법이다.

### 람다의 특징

-  이름을 가질 필요가 없는 `익명 함수(Anonymous functions)`이다.
-    두 개 이상의 입력이 있는 함수는 최종적으로 1개의 입력만 받는 람다 대수로 `단순화`될 수 있다.

> 익명함수: 말 그대로 **이름이 없는 함수**이다. 익명함수들은 공통으로 `일급객체(First class citizen)`라는 특징을 가지고 있다. 이 일급객체란 일반적으로 **다른 객체들에 적용 가능한 연산을 모두 지원하는 객체**를 가르킨다. 함수를 `값`으로 사용할 수도 있으며 `파라미터로 전달` 및 `변수에 대입`하기와 같은 연산들이 가능하다.

### 람다의 장단점

#### 장점

- 코드의 간결성 : 람다를 사용하면 **불필요한 반복문의 삭제가 가능**하며, 복잡한 식을 단순하게 표현할 수 있다.
- 지연연산 수행 : 람다은 지연연산을 수행함으로써 **불필요한 연산을 최소화** 할 수 있다.
- 병렬처리 가능 : **멀티쓰레드를 활용**하여 병렬처리를 사용할 수 있다.

#### 단점

- 람다식의 호출이 까다롭다.
- 이론상 단순하게 모든 원소를 **전부 순회하는 경우 람다식이 조금 느릴 수 있다.**
- 불필요하게 너무 사용하게 되면 **오히려 가독성을 떨어뜨릴 수 있다.**

### 람다의 표현식

- 람다은 매개변수 `화살표(->)` 함수 몸체로 이용하여 사용할 수 있다.
- 함수 몸체가 **단일 실행문이면 괄호{}를 생략할 수 있다.**
- 함수 몸체가 **return문**으로만 구성되어 있는 경우 **괄호{}를 생략할 수 없다.**

```java
//정상적인 유형
() -> {}
() -> 1
() -> { return 1; }

(int x) -> x+1
(x) -> x+1
x -> x+1
(int x) -> { return x+1; }
x -> { return x+1; }

(int x, int y) -> x+y
(x, y) -> x+y
(x, y) -> { return x+y; }

(String lam) -> lam.length()
lam -> lam.length()
(Thread lamT) -> { lamT.start(); }
lamT -> { lamT.start(); }


//잘못된 유형 선언된 type과 선언되지 않은 type을 같이 사용 할 수 없다.
(x, int y) -> x+y
(x, final y) -> x+y 
```

### 람다식 예제

#### 기존 자바 문법

```java
new Thread(new Runnable() {
   @Override
   public void run() { 
      System.out.println("Hello thread"); 
   }
}).start();
```

#### 람다식 문법

```java
new Thread(()->{
      System.out.println("Hello thread");
}).start();
```

이와 같이 람다식을 사용하면 코드가 훨씬 간결해지고 가독성도 좋아진 것을 확인할 수 있다.

# 함수형 인터페이스

### @FunctionalInterface

- 일반적으로 구현해야 할 **추상 메서드가 하나만 정의**된 인터페이스를 가리킨다.
- 자바 컴파일러는 함수형 인터페이스에 **두 개 이상의 메서드가 선언되면 오류를 발생**시킨다.
- `람다`처럼 사용이 가능하다.

```
//구현해야 할 메소드가 한개이므로 Functional Interface이다.
@FunctionalInterface
public interface Math {
    public int Calc(int x, int y);
}

//구현해야 할 메소드가 두개이므로 Functional Interface가 아니다. (오류 사항)
@FunctionalInterface
public interface Math {
    public int Calc(int x, int y);
    public int Calc2(int x, int y);
}
```

### 함수형 인터페이스 람다 사용 예제

```java
@FunctionalInterface
interface Math{
    int calc(int x, int y);
}

public class Main {

    public static void main(String[] args) {
        Math plusLambda = (x, y) -> x + y;
        System.out.println(plusLambda.calc(2, 1));
        
        Math minusLambda = (x, y) -> x - y;
        System.out.println(minusLambda.calc(2, 1));
    }
}
```