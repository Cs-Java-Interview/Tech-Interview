# Java의 제네릭과 C++의 템플릿의 차이

## 제네릭(Generic)

자바에서 어떤 자료구조를 만들어 배포하려고 한다.

String 타입도 지원하고 싶고 Integer 타입도 지원하고 싶고 많은 타입을 지원하고 싶다.

그렇다고 String, Integer 그외 등등 타입에 따라 각각 만드는 것은 비효율적이다. 

이런 문제를 해결하기 위해 JDK 1.5 이후 `제네릭(Generic)`을 사용한다.


즉, **제네릭(Generic)이란 클래스 내부에서 사용할 데이터 타입을 외부에서 사용자에 의해 지정하는 기법을 의미**한다.

특정(Specific) 타입을 미리 지정해주는 것이 아닌 필요에 의해 지정할 수 있도록 하는 일반(Generic) 타입인 것이다.


----

### 제네릭(Generic)의 장점

1. 제네릭을 사용하면 잘못된 타입이 들어올 수 있는 것을 컴파일 단계에서 방지할 수 있다.
2. 클래스 외부에서 타입을 지정해주기 때문에 따로 타입을 체크하고 변환해줄 필요가 없다.
3. 비슷한 기능을 지원하는 경우 코드의 재사용성이 높아진다.


### 제네릭(Generic) 사용방법

|타입|설명|
|---|---|
|\<T>|Type|
|\<E>|Element|
|\<K>|Key|
|\<V>|Value|
|\<N>|Number|

보통 제네릭은 다음과 같은 타입들이 많이 쓰인다.




#### 제네릭(Generic)의 선언 및 생성
자바에서 제네릭은 다음과 같은 방법으로 선언할 수 있다.


**제네릭 클래스**

```java



//클래스에 선언하는 방법
class MyArray<T> {
    T element;
    void setElement(T element) { this.element = element; }
    T getElement() { return element; }
}
```

* 위의 예제에서 사용된 'T'를 타입 변수(type variable)라고 하며, 임의의 참조형 타입을 의미

* 꼭 'T'뿐만 아니라 어떠한 문자를 사용해도 상관없으며, 여러 개의 타입 변수는 쉼표(,)로 구분하여 명시할 수 있음

* 타입 변수는 클래스에서뿐만 아니라 메소드의 매개변수나 반환값으로도 사용할 수 있음



```java
MyArray<Integer> myArr = new MyArray<Integer>();
MyArray<Integer> myArr = new MyArray<>(); // Java SE 7부터 가능함.
```
위에 선언한 제네릭 클래스를 생성할 때에는 타입 변수 자리에 사용할 실제 타입을 명시해야 한다.

또한, Java SE 7부터 인스턴스 생성 시 타입을 추정할 수 있는 경우에는 타입을 생략할 수 있음.



> 자바 제네릭에는 타입을 명시할 때 기본 타입(Primitive Type)를 사용할 수 없다.
> 
> 이때는 Integer과 같이 **래퍼 클래스(Wrapper class)** 를 사용해야 한다.


**제네릭 메소드**

```java
//메소드에 선언하는 방법
public class Box<T> {
    private T t;
    
    public T getT() {
        return t;
    }
    
    public void setT(T t) {
        this.t = t;
    }
}
```

```java
public class Util {
    
    public static<T> Box<T> boxing(T t) {
        Box<T> box = new Box<T>();
        box.setT(t);
        return box;
    } 
 }
```

```java
public class Main {
    public static void main(String[] args) {
        Box<Integer> box1 = Util.<Integer>boxing(100);
        
        Box<String> box2 = Util.boxing("암묵적호출");
    }
}
```
* 제네릭 메소드를 호출하기 위해선 두가지 방법이 있다.
  * Main 클래스에서처럼 호출할 때 타입을 지정하는 방법이 있다.
  * 암묵적 호출일 경우에는 매게타입을 보고 컴파일러가 타입을 추정한다.


**제한된 타입 파라미터**

```java
public <T extends Number> int compare(T t1, T t2){

    double v1 = t1.doubleValue();

    ....
}
```

* 타입 파라미터에 지정되는 구처적인 타입은 제한될 필요가 있다.
* 예를 들어, 숫자를 연산하는 제네릭 메소드는 매개값으로 number 타입 또는 하위 클래스 타입(Byte, Short, Long, Double)의 인스턴스만 가져와야 한다.
* 타입 파라미터 뒤에 extends 키워드를 붙이고 상위 타입을 명시한다. 상위 타입은 클래스 뿐 아니라 인터페이스도 가능하다.


**와일드카드 타입(<?>, <? extends ..>, <? super ...>)** 

* 제네릭타입<?> : 제한 없음
  * 모든 클래스나 인터페이스 타입이 올 수 있다.

* 제네릭타입<? extends 상위타입> : 상위 클래스 제한
* 제네릭타입<? super 하위타입> : 하위 클래스 제한


https://coding-factory.tistory.com/573

## 제네릭과 C++의 템플릿의 차이

Java의 Generic은 타입 제거라는 개념에 근거한다.
이 기법은 소스코드를 Java 가상 머신(JVM)이 인식하는 바이트코드로 변환할 때 인자로 주어진 타입을 제거하는 기술이다.
Java Generic이 있다고 해서 크게 달라지기보다 뭔가 더 예쁘게 작성할 수 있게 해준다. 따라서 이를 문법적 양념(syntactic sugar)라고 부른다.

C++의 Template은 좀 더 우아한 형태의 매크로로써 상황이 다르다. 컴파일러는 인자로 주어진 각각의 타입에 대해 별도의 템플릿 코드를 생성한다.
(ex. Myclass<Foo>, Myclass<Bar>가 서로 static 변수를 공유하지 않는다.)
반면에 Java static 변수는 Myclass로 만든 모든 객체가 공유한다.


- C++ Template에는 int와 같은 **기본 타입**을 인자로 넘길 수 있다. Java Generic은 불가능하다. 모든 타입은 Object를 상속해야 하며 따라서 int 대신 Integer를 사용해야 한다.
- Java의 경우, Generic 타입 인자를 **특정한 타입**이 되도록 제한할 수 있다. 가령 CardDeck을 Generic 클래스로 정의할 때, 그 인자로는 CardGame의 하위 클래스만 사용되도록 제한하는 것이 가능하다.
- C++ Template는 인자로 주어진 타입으로부터 객체를 만들어 낼 수 있다. Java에서는 불가능하다.
- Java에서 Generic Type 인자는 **static** 메소드나 변수를 선언하는 데 사용될 수 없다. MyClass<Foo>와 MyClass<Bar>가 이 메서드와 변수를 공유할 것이기 때문이다.
C++에서는 이 두 클래스는 다른 클래스이므로 템플릿 타입 인자를 static 메서드나 변수를 선언하는 데 사용할 수 있다.
- Java에서 MyClass로 만든 모든 객체는 제네릭 타입 인자가 무엇이냐에 관계없이 전부 동등한 타입이다.
실행시간에 타입 인자 정보는 삭제된다. 반면, C++에서는 다른 템플릿 타입 인자를 사용해 만든 객체는 서로 다른 타입의 객체이다.
