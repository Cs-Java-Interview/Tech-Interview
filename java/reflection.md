## 리플렉션을 사용하는 이유

Java는 컴파일 시점에 타입을 결정하는 정적인 언어이다.

- 정적 언어: 컴파일 시점에 타입을 결정 ex) Java, C, C++ 등
- 동적 언어: 런타임 시점에 타입을 결정 ex) Javascript, Python, Ruby 등

**정적 언어의 한계**

- Java는 컴파일 시점에 타입을 결정하는 정적인 언어라, 런타임 시점에 타입을 결정할 수 없다.
- 이로 인해, 컴파일 단계에서 모든 타입이 결정되어 있어야 한다.
- 즉, 코드에 타입에 대한 모든 내용이 작성되어 있어야 한다. (런타임때 동적으로 결정될 수 없으므로)
- 이는 구체화에 의존한다는 의미이고, 이로 인해 변경에 취약해진다.

이러한 정적 언어의 한계를 극복하기 위해, 런타임 시점에 타입을 결정할 수 있는 리플렉션을 사용한다.

## 리플렉션이란

- 컴파일 시간(Compile Time)에 특정 클래스의 정보을 알지 못하더라도, 
실행 시간(Run Time)에 동적으로 특정 클래스의 정보를 알아낼 수 있는 프로그래밍 기법이다.
- 리플렉션은 구체적인 클래스 타입을 알지 못하더라도 해당 클래스의 메소드, 타입, 변수들에 접근할 수 있도록 해주는 자바 API
- 이미 로딩이 완료된 클래스에서 **또 다른 클래스를 동적으로 로딩(Dynamic Loading)**
하여 생성자(Constructor), 멤버 필드(Member Variables) 그리고 멤버 메서드(Member Method) 등을 사용할 수 있도록 한다.
- 리플렉션을 통해 접근할 수 있는 클래스의 정보들
    - **Class** 클래스
    - **Constructor** 생성자
    - **Method** 메서드
    - **Field** 멤버 필드

## 리플랙션 메소드

메소드들을 통해서 런타임에 해당 클래스 혹은 메소드의 행위를 접근(확인/수정/실행)할 수 있다.

```java
package test;

public class Child { // 클래스 찾기
    public String cstr1 = "1"; // 필드

    public Child() {
    }

    private Child(String str) {
        cstr1 = str;
    }

    public int methodTest(int n) { // 메소드
        System.out.println("methodTest : " + n);
        return n;
    }
}
```

```java
// Class 찾기 (클래스 이름으로부터 클래스 찾기)
Class clazz = Class.forName("test.Child");
System.out.println(clazz.getName()); // 출력 >> test.Child

// Constructor 찾기
Constructor constructor = clazz.getDeclaredConstructor();
System.out.println(constructor.getName()); // 출력 >> test.Child

// Method 찾기
Method method1 = clazz.getDeclaredMethod("methodTest", int.class);
System.out.println(method1); // 출력 >> public int test.Child.methodTest(int)

// Field 찾기
Field field = clazz.getDeclaredField("cstr1");
System.out.println(field); // 출력 >> public java.lang.String test.Child.cstr1
```
[더 많은 리플렉션 문법들](https://kdg-is.tistory.com/entry/JAVA-%EB%A6%AC%ED%94%8C%EB%A0%89%EC%85%98-Reflection%EC%9D%B4%EB%9E%80)

## **리플렉션의 사용 예시**

코드를 작성할 시점에는 어떤 타입의 클래스를 사용할 지 모르는 경우에 리플렉션을 사용한다.

(아래 예시처럼 개발자가 직접 사용하는 경우는 드물다 → 후에 설명)

```java
public class Music {
		...
    private String singer;
    public String getSinger() {
        return singer;
    }
		...
}
public class Main{
    public static void main(String[] agrs) {
        Object music = new Music("IU", "YOU AND ME")
        music.getSinger() // 이 코드를 사용 할 수 없다.
    }
}
```

Music을 Object로 받으면 Music 클래스에 선언된 getter함수들을 사용 할 수 없다.

이렇게 (Object) music이 구체적인 클래스 타입을 알지 못할 때 Music의 메소드를 사용가능 하도록 해주는 기능이 Reflection이다.

```java
// 리플렉션 사용
public class Main {
    public static void main(String[] args) throws NoSuchMethodException, InvocationTargetException, IllegalAccessException {
        Object music = new Music("IU", "YOU AND ME");

        //== JAVA Reflection==//
        Class c = Music.class;
        Method getTitle = c.getMethod("getTitle");
        String title = (String) getTitle.invoke(music, null);

        System.out.println("title = " + title);
    }
}
```

위 예시처럼 Java Reflection을 개발자가 코딩을 하면서 직접 사용하는 일은 극히 드물다. 자신이 작성한 코드의 구체적인 클래스를 모를 일이 거의 없기 때문이다.

리플렉션은 주로 스프링 프레임워크, ORM 기술인 하이버네이트, jackson 라이브러리 등에 사용된다.

스프링 프레임워크의 경우, 프레임워크 입장에서는 개발자가 어떤 클래스를 만들었는 지 모른다. (클래스 내부의 정보를 모름)

이럴 때 동적으로 해결해주기 위해서 리플렉션을 사용한다.

### 리플렉션의 실제 사용 예시

- **Spring Framework에서의 리플렉션 사용**
    - Spring Container의 BeanFactory에서 활용한다.
    - Bean은 애플리케이션이 실행한 후 런타임에 객체가 호출될 때 동적으로 객체의 인스턴스를 생성하는데 이때 Spring Container의 BeanFactory에서 리플렉션을 사용한다.
    - 그외에도 스프링 DI, 어노테이션 등에도 사용된다.
- Spring Data JPA**에서의 리플렉션 사용**
    - 동적으로 객체 생성 시 Reflection API를 활용한다.
    - **Entity에 기본 생성자가 필요한 이유?**
        
        Reflection API로 가져올 수 없는 정보 중 하나가 생성자의 인자 정보이다. 그래서 기본 생성자가 반드시 있어야 객체를 생성할 수 있는 것이다. 기본 생성자로 객체를 생성만 하면 필드 값 등은 Reflection API로 넣어줄 수 있다. 
        
        만약 기본 생성자가 없다면 java Reflection은 해당 객체를 생성 할 수 없기 때문에 JPA의 Entity에는 기본 생성자가 꼭 필요하다.
        

<aside>
  
    💡 **리플렉션이 가능한 이유?**
    자바 클래스 파일(.class)은 static 영역(=method, class영역)에 저장되기에 
    클래스명만 안다면, static 영역에서 해당 클래스의 정보를 찾아서 가져온다.
  
</aside>

## 리플렉션의 주의사항

- 지나친 사용은 성능 이슈를 야기할 수 있다. 반드시 필요한 경우에만 사용할 것
    - 이미 인스턴스를 만들었음에도 불구하고 굳이 필드와 리플렉션을 이용해서 접근하거나 사용할 경우
- 컴파일 타임에 확인되지 않고 런타임 시에만 발생하는 문제를 만들 가능성이 있다. (컴파일 시 타입 체킹할 수 없으므로 런타임시 잘못된 파라미터로 인해 런타임 에러가 발생하기 쉽다.)
- 외부에서 접근할 수 없는 private 멤버 변수에도 접근할 수 있다.

**Reference**

[자바의 리플렉션](https://brunch.co.kr/@kd4/8#comment)

[자바의 리플렉션2](https://velog.io/@mincho920/Java-Reflection-%EB%A6%AC%ED%94%8C%EB%A0%89%EC%85%98)

[더 많은 리플렉션 문법들](https://kdg-is.tistory.com/entry/JAVA-%EB%A6%AC%ED%94%8C%EB%A0%89%EC%85%98-Reflection%EC%9D%B4%EB%9E%80)

[JPA에 기본생성자가 필요한 이유-리플렉션](https://velog.io/@yebali/Spring-JPA에-기본-생성자가-필요한-이유)

[자바에서 static 영역](https://it-mesung.tistory.com/86)

[리플렉션 주의사항](https://kmongcom.wordpress.com/2014/03/15/%EC%9E%90%EB%B0%94-%EB%A6%AC%ED%94%8C%EB%A0%89%EC%85%98%EC%97%90-%EB%8C%80%ED%95%9C-%EC%98%A4%ED%95%B4%EC%99%80-%EC%A7%84%EC%8B%A4/)
