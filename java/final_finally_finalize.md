# final, finally, finalize

### 1. final

final은 사용되는 문맥에 따라서 다르게 사용된다.

#### final 필드

final을 가장 많이 사용하는 곳은 `상수`를 선언할 때이다. final을 필드에 사용하면 그 필드는 **더 이상 수정이 불가능**하다는 의미를 갖는다.

- 변수에 적용 : 해당 **변수의 값**은 변경이 불가능해진다.

```java
final String str = "test";
str = "update test";	 // 컴파일 에러 : 수정 불가능
```

- 변수의 참조에 적용 : **참조 변수**가 힙 내의 다른 객체를 가리키도록 변경할 수 없다.

```java
final String str = new String();
str = new String();	// 컴파일 에러 : 수정 불가능
```

#### final 메서드


- 메서드에 적용 : 해당 메서드를 상속받는 하위 클래스에서 **오버라이딩 할 수 없다**. 즉, 상속받은 하위 클래스에서 메서드의 변경이 필요하지 않으면 final 키워드를 붙힌다.

```java
class Person {

    public final void test(){
        System.out.println("super class");
    }
}

class Student extends Person {
   
    // 컴파일 에러 : 오버라이딩 불가능
    @Override
    public final void test(){
        
    }
}
```

#### final 클래스

- 클래스에 적용 : 해당 클래스를 다른 클래스가 **상속받을 수 없다.** 즉, final 클래스의 하위 클래스를 정의할 수 없다.

```java
final class Person {

    public final void test(){
        System.out.println("super class");
    }
}

// 컴파일 에러 : 상속 불가능
class Student extends Person {
    
}
```

### 2. finally

finally는 `try-catch 블록` 뒤에 둘 수 있는 **선택적인 블록**인데 try-catch문이 끝나기 전에 **항상 실행되어야 하는** 로직이 있을 경우 finally 절에 두면된다. try-catch 블록과 함께 사용되며 예외가 발생하여도 항상 실행될 코드를 지정하기 위해서 사용된다. 보통 뒷 마무리 코드를 작성하는데 사용된다(DB 연결을 닫는 것 등).

```java
try {
      conn = getConnection();
} catch (Exception e){
      e.printStackTrace();
} finally {
      conn.close();
}
```

### 3. finalize()

finalize() 메서드는 `java garbage collector`가 **더 이상의 참조가 존재하지 않는 객체를 발견한 순간 호출**하는 메서드이다. 즉, 더 이상 사용되지 않는 객체가 있을 경우 메모리 낭비를 막기 위해서 garbage collector가 이 객체를 없애버리는데 이 때 해당 객체의 finalize 메서드를 호출해서 없앤다.

finalize() 메서드는 java.lang.Object 클래스로 부터 상속을 받아서 모든 클래스의 객체가 가지고 있는 메서드이다. 즉, 만약 객체가 삭제되기 직전에 실행되어야 하는 로직같은게 있으면 Object 클래스에 정의된 finalize() 메서드를 오버라이딩하여 재정의할 수 있다.

> 하지만 finalize() 메서드가 언제 호출될지 알 수 없기 때문에 finalize() 메서드를 오버라디이 했다고 해서 여기에 의존하여 코딩하는 방식의 코딩은 좋은 방법이 아닐 수 있다.

