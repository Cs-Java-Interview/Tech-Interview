# Java Version

### Java Version의 선택

최신 Java 버전은 6개월마다 업데이트가 된다.

수 많은 버전이 출시됨에 따라서 기본적으로 다음과 같은 시나리오가 존재한다.

- 기업의 기존 프로젝트에서는 Java 8을 사용해야 하는 경우가 많다.
- 일부 레거시 프로젝트는 Java 1.5 또는 1.6에서 중단되기도 했다.
- 최신 IDE, 프레임워크 및 빌드 도구를 사용하는 경우 `Java 11(LTS)` 또는 최신 `Java 17(LTS)`를 망설임 없이 사용할 수 있다.
- 안드로이드 개발의 경우 자바 버전은 기본적으로 자바 7에 고정되어있고, 특정한 자바 8 기능들을 이용할 수 있다. 또는 코틀린 프로그래밍 언어를 사용하는 것으로 전환한다.

### 특정 Java 버전을 학습해야 하는가?

결론부터 말하면 **특정 Java 버전만을 학습할 필요가 없다.**

Python 2에서 3과 같이 릴리스 사이에 심각한 문제가 있는 것과는 달리 Java는 `하위 호환성`이 매우 높기 때문이다.

- 즉, Java 5 또는 8 프로그램이 Java 8~17 가상머신에서 실행되도록 보장된다.
- 이것을 `하위 호환성`이라고 한다.
- 반대로 Java 8 JVM에서는 사용할 수 없는 Java 17 기능을 의존한다면 컴파일이 되지 않는다.

따라서 Java 8의 내용들로 토대를 쌓고 Java 9~17에 추가된 기능에 대해서 알아보고 필요에 따라서 버전을 변화하면서 적용하면 된다.

### OpenJDK 빌드(오라클) 및 OracleJDK 빌드

자바를 소스에서 빌드하는 벤더 중 하나가 오라클이라는 회사이다.

#### Oracle JDK와 Open JDK의 차이점

- `Oracle JDK`는 **상용(유료)**이지만, `Open JDK`는 **오픈소스**이다.
- Oracle JDK의 라이센스는 Oracle BCL(Binary Code License) Agreement이지만, Open JDK의 라이센스는 Oracle GPL v2이다.
- Oracle JDK는 **LTS(장기 지원)** 업데이트 지원을 받을 수 있지만, Open JDK는 **LTS없이 6개월마다 새로운 버전이 배포**된다.
- Oracle JDK는 Open JDK보다 **CPU 사용량과 메모리 사용량이 적고, 응답시간이 높다.**

Java 8 이전의 Open JDK 빌드와 Oracle JDK 빌드 사이에는 실제 소스 차이가 존재했는데, 최신의 두 버전은 본질적으로 동일하며 약간의 차이만 존재한다.

## Java 8~17 특징

### Java 8

- Lambda
- Stream
- Interface default method
- Optional
- new Date and Time API(LocalDateTime, ...)

### Java 9

- 모듈 시스템 등장(jigsaw)
- 컬렉션

```java
List<String> list = List.of("one", "two", "three");
Set<String> set = Set.of("one", "two", "three");
Map<Integer, String> map = Map.of(1, "one", 2, "two");
```

- 스트림
- Optional : ifPresentOrElse 추가 기능
- 인터페이스에 private method 사용 가능
- 기타 추가

### Java 10

- `var 키워드` -> 타입 추론

```java
// Prev Java 10
String username = "Kim";

// With Java 10
var username = "Kim";
```

- 병렬 처리 가비지 컬렉션 도입으로 인한 성능 향상
- JVM 힙 영역을 시스템 메모리가 아닌 다른 종류의 메모리에도 할당 가능

### Java 11

- Oracle JDK와 Open JDK의 **통합**
- Oracle JDK가 구독형 유료 모델로 전환
- 서드파티 JDK로의 이전 필요
- Lambda 지역변수 사용법 변경 : lambda 표현식에 **var** 사용 가능

```java
(var x, var y) -> x + y
```

- 기타 추가

### Java 12~16

12~16은 [Oracle API Docs](https://docs.oracle.com/en/java/) 참고

### Java 17

Java 17은 Java 11 이후의 새로운 Java LTS(장기 지원) 릴리즈이다.

- Pattern Matching for switch (Preview) : 객체를 전달하여 기능을 전환하고 특정 유형을 확인할 수 있다. - 예고

```java
public String test(Object obj) {
    return switch(obj) {
    case Integer i -> "An integer";
    case String s -> "A string";
    case Cat c -> "A Cat";
    default -> "I don't know what it is";
    };
}
```

- Sealed Classes (Finalized) : 상속 가능한 클래스를 지정할 수 있는 봉인 클래스가 제공

```java
public abstract sealed class Shape
    permits Circle, Rectangle, Square {...}
```

- Foreign Function & Memory API (Incubator) : Java Native Interface(JNI)를 대체한다. 기본 함수를 호출하고 JVM 외부의 메모리에 액세스할 수 있다.
- Deprecating the Security Manager : 자바 1.0 이후로 보안 관리자가 존재해왔었지만 현재는 더 이상 사용되지 않으며, 향후 버전에서 제거될 예정이다.

