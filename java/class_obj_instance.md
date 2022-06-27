## 클래스, 객체, 인스턴스의 차이

- **객체(Object)란**
    - 자신 고유의 속성을 가지는 물리적, 추상적인 모든 대상을 의미
    - 소프트웨어 세계 안에서 구현할 대상

- **클래스(Class)란**
    - 객체를 생성하기 위한 **설계도**
    - 연관되어 있는 변수와 메서드의 집합
    - 필드(Field), 생성자(Constructor), 메소드(Method)로 구성
    
- **인스턴스(Instance)란**
    - 클래스를 통해서 구현된 구체적인 실체
    - 실제로 **메모리로 할당**된 상태

- 객체와 인스턴스 차이?
    - 객체와 인스턴스는 혼용해서 표현할 수 있음
    - 클래스의 타입으로 선언되었을 때 객체라고 부르고, 
    그 객체가 메모리에 할당되어 실제 사용될 때 인스턴스라고 부른다.
    - 객체를 ‘**클래스의 인스턴스**’라고도 부른다.


```java
public class Dog
{
    // Variables
    String name;
    int age;
 
    // Constructor
    public Dog(String name, int age) {
        this.name = name;
        this.age = age;
    }
 
    // method
    public String getName(){    
				return name;
		}
		public int getAge(){    
				return age;
		}
 
    public static void main(String[] args)
    {
        Dog myDog = new Dog("dog_name", 5); // 클래스로 구현된 실체 (객체, 인스턴스)
    }
}
```
