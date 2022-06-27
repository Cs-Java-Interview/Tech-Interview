# 추상 클래스(Abstract class)

## 추상 메서드와 추상 클래스

### 추상 메서드(abstract method)

- 선언되어 있으나 구현되지 않은 메서드, `abstract`로 선언한다.

```java
public abstract String getName();
public abstract void setName(String name);
```

- 추상 메서드는 상속받는 서브 클래스에서 오버라이딩하여 구현해야 한다.

### 추상 클래스(abstract class)의 2종류

1. 추상 메서드를 하나라도 가진 클래스
	- 클래스 앞에 반드시 abstract라고 선언해야 한다.

```java
abstract class Shape { 
	public void paint() { draw(); }
	abstract public void draw(); // 추상 메서드
}
```

2. 추상 메서드가 하나도 없지만 abstract로 선언된 클래스

```java
abstract class MyComponent { // 추상 클래스 선언 String name;
	public void load(String name) {
		this.name = name; 
	}
}
```

### 추상 클래스의 목적 및 특성

- `상속`을 위한 슈퍼 클래스로 활용하는 것이다.
- 서브 클래스에서 **추상 메서드를 구현**하기 위함
- `다형성`의 실현
- 추상 클래스로 **객체를 생성할 수 없다**.

```java
Shape shape = new Shape(); // 컴파일 오류. 추상 클래스 Shape의 객체를 생성할 수 없다.
```

### 추상 클래스의 상속

상속에는 2가지의 경우가 있는데, 다음과 같다.

1. 추상 클래스의 단순 상속
	- 추상 클래스를 상속받아서 추상 메서드를 구현하지 않는다면 추상 클래스가 되어 abstract 키워드를 붙여야 한다.

```java
abstract class Shape { // 추상 클래스
	public Shape() { }
	public void paint() { draw(); }
	abstract public void draw(); // 추상 메소드
}
abstract class Line extends Shape { 
	public String toString() { return "Line"; }
	// 슈퍼 클래스의 추상 메서드를 구현하지 않았음
} 
```

2. 추상 클래스의 구현 상속
	- 서브 클래스에서 슈퍼 클래스의 **추상 메서드 구현(오버라이딩)**
	- 서브 클래스는 **추상 클래스가 아닌 경우**를 말한다.

# 인터페이스(Interface)

### 인터페이스의 정의

- 클래스와 클래스 사이의 상호 규격을 나타낸 것이다.
- 즉, 인터페이스는 필요한 메서드들의 이름, 매개변수에 대해서 서로 합의하는 것이다.
- 예를들면, 전자기기들은 turnOn(), turnOff() 메서드들이 대부분 존재한다.
- 인터페이스는 **몸체가 없는 추상 메서드만 정의**된다.

```java
public interface RemoteControl { // 인터페이스 선언
	public static final int  TIMEOUT = 10000;	// 상수 필드 public static final 생략 가능
	public abstract void sendCall();	// 추상 메서드 public abstract 생략 가능
	public abstract void receiverCall();	// 추상 메서드 public abstract 생략 가능
	public default void printLogo() {	// default 메서드 public 생략 가능
		System.out.println("** phone **);
	}
}
```

### 인터페이스 구성요소들의 특징

- 상수
	- **public 만 허용**된다. **public static final 생략 가능**하다.
- 추상 메서드
	- **public abstract 생략 가능**하다.
- default 메서드
	- 인터페이스에 코드가 작성된 메서드이다.
	- 인터페이스를 구현하는 클래스에 자동으로 상속된다.
	- **public 접근 지정만 허용**된다. 생략이 가능하다.
- private 메서드
	- 인터페이스 내에 메서드 코드가 작성되어야 한다.
	- 인터페이스 내에있는 다른 메서드에 의해서만 호출이 가능하다.
- static 메서드
	- public, private 모두 지정이 가능하며, **생략하면 public**이 된다.

### 인터페이스의 특징

- 인터페이스로 **객체 생성이 불가능**하다.

```java
PhoneInterface galaxy = new PhoneInterface();	// 컴파일 오류
```

- 인터페이스 타입의 **참조변수는 선언이 가능**하다.

```java
PhoneInterface galaxy = new SamsungGalaxy();
```

- 인터페이스를 상속받는 서브 클래스는 인터페이스의 **모든 추상메서드를 구현해야 한다.**
- 다른 인터페이스와의 **다중 상속이 가능**하다.
- 인터페이스를 상속받을때는 `extends` 키워드를 사용하고, 클래스에서 인터페이스를 상속받을 때는 `implements` 키워드를 사용한다.