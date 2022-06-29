# Java의 직렬화와 역직렬화

## 직렬화와 역직렬화란

### 직렬화(Serialization)란

객체를 직렬화하여 전송 가능한 형태로 만드는 것을 의미한다. 객체들의 데이터를 연속적인 데이터로 변경하여 Stream을 통해 데이터를 읽도록 해준다.

객체들을 통째로 파일로 저장하거나 전송하고 싶을 때 사용된다.


### 역직렬화(Deserialization)란

직렬화된 파일들을 역으로 직렬화하여 다시 객체의 형태로 만드는 것을 의미한다. 저장된 파일을 읽거나 전송된 스트림 데이터를 읽어 원래 객체의 형태로 복원한다.


## 직렬화가 필요한 이유

개발 언어에는 값 형식 데이터와 참조 형식 데이터 두가지가 있다. 두 가지 데이터 중 디스크에 저장하거나 통신할 때는 값 형식 데이터만 사용할 수 있다 
참조 형식 데이터는 실제 데이터 값이 아닌 힙에 할당된 메모리 번지 주소를 갖고 있기 때문이다.

객체 A를 만들어 파일에 저장한다고 했을 때 객체 A의 주소값을 저장하게 된다. 
이후 프로그램을 종료 후 다시 프로그램을 켜서 해당 주소값을 가져온다고 하더라도, 프로그램이 종료됐을 때 기존에 할당됐던 메모리는 해제되고 없기 때문에 기존 A객체를 가져올 수 없다. 

네트워크 통신에서도 각 PC마다 사용하고 있는 메모리 공간 주소는 전혀 다르기 때문에 객체의 주소값은 무의미하게 된다.

직렬화를 하게 되면 값이 가지는 데이터를 전부 모아 값 형식 데이터로 변환해준다. 직렬화된 데이터는 언어에 따라 텍스트 또는 바이너리 등의 형태가 되는데, 이러한 형태가 저장하거나 통신할 때 파싱이 가능한 유의미한 데이터가 된다.

즉, 직렬화를 하는 이유는 사용하고 있는 데이터를 파일 저장 혹은 데이터 동신에서 파싱할 수 있는 유의미한 데이터를 만들기 위함이다.


### 직렬화의 특징

* 프로그램이 종료되어도 객체의 데이터는 파일로 변환하여 저장되어 있기 때문에 언제든지 불러서 다시 객체로 변환할 수 있으며 외부로 보내서 데이터를 공유할 수 있음
* Java의 기본 라이브러리를 사용하지 않더라고 여러 형태(CSV, JSON, 일반 파일 등)로 수행이 가능
* Java에서 제공하는 직렬화 기능은 오직 Java 프로그램끼리만 공유가 가능한 데이터이며 코드 수정을 하거나 자바 버전이 달라 클래스 속성이 바뀌게 되면 사용할 수 없음
* Java의 JVM에서 자동으로 처리해주기 때문에 수신부와 송신부의 운영체제가 달라도 아무런 상관이 없음


### 직렬화 하는 방법
* java.io.Serializable 인터페이스를 구현한다.

```java
class Person implements Serializable {
	String name;
	int age;
	
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}

	@Override
	public String toString() {
		return "Person [name=" + name + ", age=" + age + "]";
	}
}
```

Serializable 인터페이스는 사실 구현할 부분이 없고 **현재 클래스의 객체가 직렬화가 제공되어야 함만을 JVM에게 알려주는** 역할을 한다.

```java
public class Main {
	public static void main(String[] args) throws IOException {
		Person person = new Person("Libi", 26);
		
		try(ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("Serialization.txt"))){
			out.writeObject(person);
		} catch (Exception e) {
		}
	}
}
```

Person 클래스의 객체를 직렬화를 통해 파일로 변환하여 저장해보자.

객체 직렬화는 직렬화하고자 하는 객체에 직렬화를 수행해주는 **ObjectOutputStream**, 직렬화한 내용을 저장할 .txt 파일을 생성해주는 **FileOutputStream**을 통해 수행된다.


### 역직렬화 방법
```java
public class Main {
	public static void main(String[] args) throws IOException {
		Person person = null;
		
		try(ObjectInputStream in = new ObjectInputStream(new FileInputStream("Serialization.txt"))){
			person = (Person) in.readObject();
		} catch (Exception e) {
		}
		
		System.out.println(person.toString());
	}
}
```

객체 역직렬화는 불러온 파일에 역직렬화를 수행하여 객체로 복수시켜주는 ObjectInputStream과 .txt 파일을 불러오는 FileInputStream을 통해 수행된다.

### 추가사항

특정 정보를 저장하고 싶지 않다면 멤버에 **transient** 또는 **static** 키워드를 붙이면 제외할 수 있다.

```java
class Person implements Serializable {
	static String name;
	transient int age;
	
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}

	@Override
	public String toString() {
		return "Person [name=" + name + ", age=" + age + "]";
	}
}
```

### 역직렬화의 주의사항
역직렬화를 수행하기 위해선 직렬화한 클래스와 역직렬화를 수행하는 클래스 속성이 일치해야 한다.
그렇다면 어떻게 Java가 두 클래스의 속성이 일치하는 지 판단할 수 있을까?

이는 **serialVersionUID** 필드를 통해 판단한다. 이는 Serializable 인터페이스에 구현된 것으로, 직렬화 클래스를 선언한 클래스를 컴파일 할 때 컴파일러가 클래스 구조 정보를 해시값으로 변환한 값을 자동으로 넣어준다.

Default 값으로 내부적으로 클래스의 구조 정보를 이용하여 자동으로 생성된 해시 값이 할당된다. 때문에 클래스의 멤버 변수가 추가되거나 삭제되면 serialVersionUID가 달라진다.

```java
public class Member implements Serializable {

    private static final long serialVersionUID = 1L;
```
이 값을 통해 두 클래스의 속성이 같은 지 판단한다.

### 직렬화 특징
1. 객체 직렬화는 타입에 대해 상당히 엄격하다.
  *  String → StringBuilder, int → long 등 잘못된 타입으로 받아들이면 Exception이 발생한다.
2. 간단한 객체의 내용도 직렬화를 수행하면 데이터뿐만 아니라 다른 구조 데이터 등의 정보도 포함되기 때문에 데이터의 크기가 많이 커지는 단점이 있다.
  * 따라서 웬만하면 내용 변경이 없을만한 클래스에만 직렬화를 사용하는 것이 좋으며, 장기 보관용으로는 적합하지 않다.
  * 또한 데이터 크기가 많이 차이 나기 때문에 긴 만료 시간을 가지는 데이터는 Json 과 같은 포맷을 이용하는 것이 좋다.
3. 이러한 객체 직렬화는 Java에서 서블릿 세션, 캐시, 자바 RMI 등에서 사용된다.

