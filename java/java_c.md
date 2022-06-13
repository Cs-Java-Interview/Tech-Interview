# Java와 C 차이점

1. Java는 **객체지향 언어** , C는 **절차지향 언어**이다.
2. 속도는 C가 상대적으로 빠르고 바이너리 크기가 작다.
3. C는 OS 개발의 목적으로 만들어진 언어이다.
4. Java는 C와는 다르게 JV위에서 실행되므로 OS 독립적으로 실행이 가능하다. **(Write Once, Run Anywhere)**

# Java와 CPP 차이점

## 1. 상속
1. C++는 `다중 상속`을 지원하고 Java는 그렇지 않다. `다중 상속`이라 함은, 하나의 클래스가 두개 이상의 클래스를 상속하는 것을 지칭한다.
2. C++는 `friend` 키워드를 지원하고 Java는 그렇지 않다. friend 키워드를 붙여 클래스 혹은 함수를 선언해 놓으면 그 클래스의 private, protected area에 접근할 수 있다. 이는 **객체의 은닉성**을 깨는 행위므로 지양한다.
3. Java는 `Interface`를 지원하고 C++는 그렇지 않다. Interface는 상수와 추상메소드로만 구성되어 있다.


> 단, Java 8 버전 이후에서는 interface에서도 default method와 static method가 구현 가능하다.
> 
> https://jeong-pro.tistory.com/209


## 2. 메모리 처리
1. Java는 객체를 메모리의 Heap영역에만 할당할 수 있으나, C++의 경우 Heap과 Stack 영역 모두에서 할당 가능하다.
2. Java는 메모리 해체가 자동적으로 이루어지지만 **(GC에 의해)**, C++는 개발자가 직접 해야한다. **(free함수 및 delete 함수)**


## 3.문법 및 기능
1. C++는 `연산자 오버로딩`을 지원하지만 Java는 그렇지 않다. 연산자 오버로딩이란 연산자를 재정의 하는 기능이다.
2. Java는 `익명 클래스`를 지원하지만 C++는 그렇지 않다.
3. Java는 `동적바인딩`을 채택하지만 C++는 `정적바인딩`을 택하고 있다. C++에서도 **virtual**키워드를 통해 동적바인딩이 가능하다.


> **동적바인딩** 
> 
> * 다형성을 사용하여 메소드를 호출할 때, 발생하는 현상이다.
> * **실행 시간**(Runtime) 즉, 파일을 실행하는 시점에 성격이 결정된다.
> * 실제 참조하는 객체는 서브 클래스이니 **서브 클래스의 메소드**를 호출한다.
> 
> **정적 바인딩**
> 
> * **컴파일(Compile) 시간**에 성격이 결정된다.
> * 변수의 타입이 수퍼 클래스이니 수퍼 클래스의 메소드를 호출한다.
> 
> https://computasha.github.io/CS-dynamic-binding/

