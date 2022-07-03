# Java Annotation

## Annotation 이란?

* 자바 어노테이션이란 자바 코드에 메타 데이터를 제공하기 위해 사용된다.
* Java 5 이후부터 등장한 기술이다.
* AOP(Aspect Orientied Programming)을 편리하게 구성할 수 있다.

## Annotation 특징
* 컴파일러에게 코드 문법 에러를 체크하도록 정보를 제공

* 소프트웨어 개발 툴이 빌드나 배치 시 코드를 자동으로 생성할 수 있도록 정보를 제공
* 어노테이션을 만들 때 용도를 분명하게 해야 한다
  * 소스 상에서만 유지할 지
  * 컴파일러된 클래스에도 유지할 지
  * 런타임시에도 유지할 지 지정한다.

## Annotation 사용방법

* @ 문자와 어노테이션 이름으로 구성이 되며, 컴파일러는 @ 문자로 시작되면 어노테이션으로 판단한다. 
```java
@Entity(name = "table")
```
어노테이션은 값을 저장할 수 있는 엘리먼트를 가지며, 어노테이션 이름 다음 괄호안에 엘리먼트를 정의한다.

어노테이션은 클래스, 인터페이스, 메소드, 파라메터, 필드, 지역변수 위에 위치할 수 있다.


## Annotation 사용 예시

### Built-in Annotation

#### **@Override**
* 메소드가 오버라이드 됐는지 검증한다.
* 만약 부모 클래스 또는 구현해야할 인터페이스에서 해당 메소드를 찾을 수 없다면 컴파일 오류가 난다.

#### **@Deprecated**
* 클래스, 메소드, 필드가 deprecated 되었음을 알릴 때 사용한다.
* deprecated code를 사용하면 컴파일러에서는 warning을 표시한다.
* 어노테이션을 사용할 때, @deprecated JavaDoc symbol을 사용하여, 왜 deprecated 되었는지 대신에 무엇을 사용해야 하는지 작성하는 것이 좋다.

```java
@Deprecated
/**
  @deprecated Use MyNewComponent instead.
*/
public class MyComponent {
}
```

#### **@SuppressWarnings**
* 컴파일 경고를 무시하도록 한다.

#### **@SafeVarargs**
* 제너릭 같은 가변인자 매개변수를 사용할 때 경고를 무시한다. (자바7 이상)

#### **@FunctionalInterface**
* 람다 함수등을 위한 인터페이스를 지정합니다. 메소드가 없거나 두개 이상 되면 컴파일 오류가 발생. (자바 8이상)

### Meta Annotations
다른 어노테이션에 적용할 수 있는 어노테이션을 메타 어노테이션이라고 한다.

```java
 @Target(ElementType.TYPE)
 @Retention(RetentionPolicy.RUNTIME)
 @Documented
 @Component
 public @interface Service {
 
     // ...
 }
```

#### @Retention
* 어노테이션이 언제까지 유효한 지를 명시한다.

#### @Target
* 어노테이션을 JAVA의 어떤 엘레먼트에 적용할 지 명시한다.
* 총 11가지의 엘레먼트 타입이 있다.

  * TYPE : 모든 타입에 사용 가능( ex. class, interface, enum, annotation )
  * METHOD : 메서드에 사용 가능
  * FIELD : 필드에 사용 가능
  * CONSTRUCTOR : 생성자에 적용
  * ANNOTATION_TYPE : 다른 어노테이션에 적용하기 위해 사용되는 타입.( ex @Target, @Retetion )

#### @Inherited
* 자식 클래스에 부모 클래스의 어노테이션을 갖게 하기 위해(propagate) 사용하는 어노테이션
* 아래 코드에서 DerivedClass는 BaseClass에 선언된 @InheritedAnnotation이 적용된다.
```java
@Inherited
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface InheritedAnnotation {
}

@InheritedAnnotation
public class BaseClass {
}

public class DerivedClass extends BaseClass {
}
```

#### **@Documented**
* Javadocs에 어노테이션의 사용을 문서화 해주게 하는 어노테이션
* 아래 코드처럼 ExcelCell이라는 어노테이션어 @Documented를 선언하면, Javadocs에 Employee 클래스에서 ExcelCell이라는 어노테이션을 사용한다고 문서화해줌
```java
@Documented
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
public @interface ExcelCell {
    int value();
}

public class Employee {
    @ExcelCell(0)
    public String name;
}
```

![image](https://user-images.githubusercontent.com/36829127/177041358-895b6795-a875-4dd1-b3fe-791cefc7940d.png)

**    javadocs**

#### **@Repeatable**
* 어노테이션을 반복 사용할 때 사용하는 어노테이션이다.

1. Repeatable Annotation 타입을 선언(실 사용할 어노테이션)
  * `@Repeatable`에 Container annotation 전달
```java
@Repeatable(value = Colors.class) // 
@Retention(RetentionPolicy.RUNTIME)
public @interface Color {
  String value(); // 
}
```

2. 컨테이너 어노테이션 타입을 포함하여 선언(묶음 값을 관리하는 컨테이너 어노테이션)
  * 컨테이너 어노테이션은 내부 어노테이션을 반환하는 value() 메서드를 정의해야한다.
```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Colors {
  Color[] value();  
}
```

위와 같이 `@Repeatable` 어노테이션으로 구현하면 다음과 같이 어노테이션을 사용할 수 있다.
```java
@Color("green")
@Color("blue")
@Color("red")
public class RGBColor { ... }
```



참조 : 

https://goateedev.tistory.com/319

https://nesoy.github.io/articles/2018-04/Java-Annotation

https://glory-day.tistory.com/90

https://dahye-jeong.gitbook.io/java/java/advanced/meta-annotation/2021-06-13-repeatable-annotation
