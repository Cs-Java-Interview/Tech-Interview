### 오버로딩(Overloading)

- **메서드의 이름은 같고** **매개변수의 갯수나 타입이 다른 함수**를 정의하는 것을 의미한다.
- 리턴값만을 다르게 갖는 오버로딩은 작성 할 수 없다.

```java
class MethodOverloadingEx {

    static int add(int a, int b)
    {
      return a + b;
    }
 
    static int add(int a, int b, int c)
    {
        return a + b + c;
    }
 
    public static void main(String args[])
    {
        System.out.println(add(4, 6));
        System.out.println(add(4, 6, 7));
    }
}
```

### 오버라이딩(Overriding)

- **상위 클래스의 메서드를 하위 클래스가 재정의**하는 것이다.
- **메서드의 이름은 물론 파라미터의 갯수나 타입도 동일**해야 한다.
- 주로 상위 클래스의 동작을 상속받은 하위 클래스에서 변경하기 위해 사용된다.

```java
class Animal {
 
    void eat()
    {
        System.out.println("eating.");
    }
}
 
class Dog extends Animal {
 
    void eat()
    {
        System.out.println("Dog is eating.");
    }
}
 
class MethodOverridingEx {
 
    public static void main(String args[])
    {
        Dog d1 = new Dog();
        Animal a1 = new Animal();
 
        d1.eat(); // "Dog is eating."
        a1.eat(); // "eating."
 
        Animal animal = new Dog();
        animal.eat(); // "Dog is eating."
    }
}
```

- 정리
    - 오버로딩(Overloading)은 기존에 없던 새로운 메서드를 정의하는 것이고, (메소드 이름만 같음)
    - 오버라이딩(Overriding)은 상속 받은 메서드의 내용만 변경 하는 것이다.
    - 오버로딩과 오버라이딩으로 객체의 다형성을 구현할 수 있다.
