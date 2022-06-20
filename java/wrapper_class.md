기본 타입의 데이터를 객체로 취급해야 하는 경우가 있다. 예를 들어, 메소드의 인수로 객체 타입만이 요구되면, 기본 타입의 데이터를 그대로 사용할 수는 없다. 이때 기본 타입의 데이터를 먼저 객체로 변환한 후 작업을 수행해야 한다.****

### 래퍼클래스

- 래퍼클래스란, 기본 타입에 해당하는 데이터를 객체(참조 자료형)로 포장해주는 클래스이다.****
- **사용 이유 ?**
    - 기본 자료형의 값을 단순히 값으로만 사용하지 않고 **참조형 자료형처럼 (객체 형태로) 사용하기 위하여**
    - **객체 또는 클래스가 제공하는 메소드 사용**
    - **기본 자료형의 값을 숫자, 문자로의 형변환 할 경우**
    - 그 외에도 null 값을 이용하기 위해, 제너릭을 사용하기 위해
- 래퍼 클래스는 각각의 타입에 해당하는 데이터를 인수로 전달받아, 해당 값을 가지는 객체로 만들어 준다. 이러한 래퍼 클래스는 모두 java.lang 패키지에 포함되어 제공된다.
    
    ex) `Integer num1 = new Integer(1);`
    
- 래퍼 클래스(Wrapper class)는 산술 연산을 위해 정의된 클래스가 아니므로, 인스턴스에 저장된 값을 변경할 수 없다. 단지, 값을 참조하기 위해 새로운 인스턴스를 생성하고, 생성된 인스턴스의 값만을 참조할 수 있다.
- 기본 타입과 래퍼 타입
    
    
    | 기본 타입 | 래퍼 클래스 |
    | --- | --- |
    | byte | Byte |
    | short | Short |
    | int | Integer |
    | long | Long |
    | float | Float |
    | double | Double |
    | char | Character |
    | boolean | Boolean |
- 래퍼 타입의 상속 계층도 : 모든 래퍼 클래스의 상위클래스는 Object이기에 Object의 메소드를 상속받는다. (ex. equals(), toString(), 등)
    
    ![image](https://user-images.githubusercontent.com/77563814/174629163-b9959e78-96fc-4b32-84c2-21ef640c5cb3.png)
    

**박싱(Boxing), 언박싱(Unboxing)**

- ![image](https://user-images.githubusercontent.com/77563814/174629273-b061f45f-091b-434b-af8e-cd7ef06856a1.png) 

- **박싱(Boxing)** : 기본 타입의 데이터를 래퍼 클래스의 인스턴스로 변환하는 과정
- **언박싱(UnBoxing)** : 래퍼 클래스의 인스턴스에 저장된 값을 기본 타입의 데이터로 꺼내는 과정

**오토 박싱(AutoBoxing)과 오토 언박싱(AutoUnBoxing)**

JDK 1.5부터는 박싱과 언박싱이 필요한 상황에서 자바 컴파일러가 이를 자동으로 처리해 준다. 이렇게 자동화된 박싱과 언박싱을 오토 박싱(AutoBoxing)과 오토 언박싱(AutoUnBoxing)이라고 한다.

```java
Integer num = new Integer(17); // 박싱
int n = num.intValue();        // 언박싱
```

**래퍼 클래스에서의 값 비교**

- `==`(동등 연산자) 사용시 : 두 인스턴스의 **주소값**을 비교함
- `equals()` 메소드 사용 시 : 두 인스턴스의 **저장된 값**을 비교함
- 따라서 인스턴스에 저장된 값의 동등 여부를 판단하려면 `equals()` 메소드를 사용해야한다.

```java
Integer num1 = new Integer(1);
Integer num2 = new Integer(1);

System.out.println(num1 == num2); // false (주소값이 다르므로)
System.out.println(num1.equals(num2)); // true (저장된 값이 같으므로)
```

**Reference**

[http://www.tcpschool.com/java/java_api_wrapper](http://www.tcpschool.com/java/java_api_wrapper)

[https://www.w3resource.com/java-tutorial/java-wrapper-classes.php](https://www.w3resource.com/java-tutorial/java-wrapper-classes.php)
