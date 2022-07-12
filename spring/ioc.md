## IoC(Inversion of Control, 제어의 역전)란

- 객체의 생성과 객체 생명 관리, 흐름 제어를 제 3자에게 위임하는 프로그래밍 모델
- 이 방식은 대부분의 프레임워크에서 사용하는 방법으로, 개발자는 필요한 부분을 개발해서 끼워 넣기의 형태로 개발하고 실행하게 된다.
- 이렇게 조립된 코드의 최종 호출은 개발자에 의해서 제어되는 것이 아니라 프레임워크의 내부에서 결정된 대로 이뤄지게 되는데, 이러한 현상을 "제어의 역전"이라고 표현한다.

## Spring에서의 IoC

- Spring 프레임워크에서 지원하는 Ioc Container는 자바 객체(빈)의 생성과 생명주기, 의존관계를 관리한다. 이를 코드 대신 컨테이너가 오브젝트에 대한 제어권을 갖고 있다고 해서 IoC라고 부른다. 그래서 스프링 컨테이너를 IoC 컨테이너라고도한다.

ex1) 개발자가 직접 객체를 생성하여 코드를 제어하는 경우

```java
// A객체는 B객체에게 의존한다
public class A {

    private B b;

    public A()
        b = new B();
    }
}
```

ex2) 컨테이너에 의해서 생성한 객체를 사용만 하는 경우 (제어의 역전)

```java
public class A {

		// 스프링 컨테이너에 있는 객체 B를 주입
    @Autowired
    private B b;

}
```

두번째는 B 객체를 직접 생성하지 않고, 스프링 컨테이너 속 Bean을 주입받아서 사용한다.

<aside>

  
    💡 이렇게 개발자가 직접 객체를 관리하지 않고 
    스프링 컨테이너에서 직접(제어) 객체를 생성하여 해당 객체에 주입 시켜준 것이므로 제어의 역전이라 한다.

  
</aside>

## IoC 사용하는 이유?

- 객체 지향 원칙(DIP, OCP)를 잘 지키기 위해서 ([참고 링크](https://github.com/Cs-Java-Interview/Tech-Interview/blob/main/spring/container.md#%EC%8A%A4%ED%94%84%EB%A7%81-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0?))
- 객체를 관리해주는 프레임워크와 내가 구현 하고자 하는 부분으로 역할과 관심을 분리해 응집도를 높이고 결합도를 낮추며, 이에 따라 변경에 유연한 코드를 작성 할 수 있는 구조가 될 수 있기 때문에 제어를 역전한 것이다.

## IOC와 DL, DI

IoC는 객체의 생성과 객체 생명 관리, 흐름 제어를 제 3자에게 위임하는 것을 의미하고,

IoC를 구현하는 방법에는 DL과 DI이 있는데, 주로 DI을 사용한다.

- **DL(Dependency Lookup)**
    - 의존성 검색을 의미한다.
    - Bean에 접근하기 위해 컨테이너가 제공하는 API를 이용하여 Bean을 Lookup하는 것이다.
    - getBean() 메소드를 통해 스프링 컨테이너에 담겨있는 구현 클래스를 직접 가져오는 방식
    - 
        
        ```java
        public class UserDao {
            private ConnectionMaker connectionMaker;
            
            public UserDao() {
                AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(DaoFacory.class);
                this.connectionMaker = context.getBean("connectionMaker", ConnectionMaker.class);
            }
        }
        ```
        
- **DI(Dependency Injection)**
    - 의존성 주입을 의미한다.
    - 각 클래스간의 의존성을 자신이 아닌 외부(컨테이너)에서 주입하는 개념이다.
    - 일반적으로 Bean 설정 파일을 바탕으로 의존관계를 확인하여 주입하게 된다.
    이는 객체 레퍼런스를 컨테이너로 부터 주입받아서, 실행 시에 동적으로 의존관계가 생성된다. 따라서 컨테이너가 흐름의 주체가 되어 어플리케이션 코드에 의존 관계를 주입하게 되는 것이다.
        
        ```java
        public class UserDao {
            private ConnectionMaker connectionMaker;
        
            public UserDao(ConnectionMaker connectionMaker) {
                this.connectionMaker = connectionMaker;
            }
        }
        ```
        

>   IoC와 DI의 차이?
> 
> - IoC는 객체의 흐름, 생명주기 관리 등을 제 3자에게 위임하는 방식이다. 스프링 외에도 다른 프레임워크 및 디자인 패턴과 같이 범용적인 곳에서 사용하는 개념이다. (어떤 객체의 사용 여부가 타 객체에 의해 수동적으로 결정되어 사용되는 방식도 IoC라고 볼 수 있다.)
> - 하지만 DI는 인터페이스를 통해 다이나믹하게 객체를 주입을 하여 유연한 프로그래밍을 가능하게 하는 패턴으로 좀 더 구체적인 의미.


## 라이브러리와 프레임워크의 차이

- **IoC 적용 여부의 차이**
- 라이브러리
    - IoC 적용X
    - 개발자가 애플리케이션 흐름을 직접 제어한다. 단지 동작하는 중에 필요한 기능이 있을 때 능동적으로 라이브러리를 사용할 뿐이다.
    - ex) React, JQuery
- 프레임워크
    - IoC 적용
    - 애플리케이션 코드가 프레임워크에 의해 사용된다. 보통 프레임워크 위에 개발한 클래스를 등록해두고, 프레임워크가 흐름을 주도하는 중에 개발자가 만든 애플리케이션 코드를 사용하도록 만드는 방식이다.
    - ex) Spring Framework, Django
