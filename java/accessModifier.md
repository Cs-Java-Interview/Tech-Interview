**제어자(modifier)란** 

- 클래스와 클래스 멤버의 선언 시 사용하여 부가적인 의미를 부여하는 키워드를 뜻함
- 접근 제어자(access modifier)와 기타 제어자(final, abstract, static)가 있다.

## 접근 제어자의 종류

1. private : 해당 클래스에서만 접근이 가능하다.
2. default(기본 설정값) : 해당 패키지 내에서만 접근이 가능하다.
3. protected : 동일 패키지에 있거나, 해당 클래스를 상속받은 클래스에서만 접근이 가능하다.
4. public : 어떤 클래스에서라도 접근이 가능

```java
// 1. private : 해당 클래스에서만 접근이 가능하다.
public class SameClass {
    private String var = "같은 클래스만 허용"; // private 필드
    private String getVar() {                  // private 메소드
        return this.var;
    }
}

// 2. default(기본 설정값) : 해당 패키지 내에서만 접근이 가능하다.
package test;
public class SameClass {
    String var = "다른 패키지는 접근 불가"; // default 필드

    public static void main(String[] args) {
        SamePackage sp = new SamePackage();
        System.out.println(sp.sameVar);     // 같은 패키지는 허용
    }
}

// 3. protected : 동일 패키지에 있거나, 해당 클래스를 상속받은 클래스에서만 접근이 가능하다.
package test;

public class SameClass {
    String var = "다른 패키지는 접근 불가"; // default 필드
    public static void main(String[] args) {
        SamePackage sp = new SamePackage();
        System.out.println(sp.sameVar);     // 같은 패키지는 허용
    }
}

// 4. public : 어떤 클래스에서라도 접근이 가능
public class Everywhere {

    public String var = "누구든지 허용"; // public 필드

    public String getVar() {             // public 메소드
        return this.var;
    }
}
```

**접근 제어자를 사용하는 이유?**

: 클래스의 내부에 선언된 데이터를 보호하기 위해서

데이터를 외부에서 함부로 변경하지 못하도록 외부로부터의 접근을 제한하기 위해 사용한다. 이를 객체지향에선 캡슐화(encapsulation)라 한다.



**Reference**

[http://www.tcpschool.com/java/java_modifier_accessModifier](http://www.tcpschool.com/java/java_modifier_accessModifier)
