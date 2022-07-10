## 스프링 컨테이너를 사용하는 이유?

: 결론 먼저 말하자면, 객체지향 설계의 5원칙 중 **DIP, OCP**를 지키기 위해서이다.

- 만약 순수 자바로만 코드를 짜다보면, 역할과 구현을 충실하게 분리하여서 코딩하더라도 OCP, DIP를 위배하게 된다.
- 아무리 인터페이스를 분리하고, 그 인터페이스에 해당하는 구현체들을 만드는 방식으로 프로그램을 구상하더라도, 결국 해당 클래스 내부에는 구체적인 구현체를 선언하고 할당해야하는 과정이 존재한다.
- 즉 구현체를 바꿀 때마다, 프로그래머가 해당 클래스 내부의 코드들을 일일히 수정해야 한다는 것을 의미한다.

⇒ 이는 개방폐쇄원칙으로 불리는 OCP 원칙과 의존관계 주입이라고 불리는 DIP 원칙을 위배한다.

예시)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4fe86781-0991-472f-af79-c0954a081b10/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bb95d534-edea-4ccb-8bde-2e61aced9e2c/Untitled.png)

*왼쪽은 기대했던 의존관계, 오른쪽은 자바코드로만 구현할 경우 실제 의존관계*

```java
public class OrderServiceImpl implements OrderService{

    private final MemberRepository memberRepository = new MemoryMemberRepository();
		
    //private final DiscountPolicy discountPolicy = new FixDiscountPolicy();
    private final DiscountPolicy discountPolicy = new RateDiscountPolicy();
	...
}
```

위의 예시처럼 자바 코드로만 개발할 경우, 구현체를 선언하고 의존관계를 설정하게 될 것이다.

1. 구현체가 아닌 추상체에 의존하라는 DIP 원칙 ❌위반!
    
    OrderServiceImple은 DiscountPolicy 뿐만 아니라 
    그 구현체인 FixDiscountPolicy 혹은 RateDiscountPolicy를 의존하고 있다.
    
2. 확장에는 열려있고 변경에는 닫혀있어야 한다는 OCP ❌위반!
    
    기존 코드인 OrderSerivceImpl에서의 코드 변경 없이는 확장할 수 없다.
    

이를 해결하기 위해 구현체가 아닌 인터페이스에만 의존할 수 있도록 아래와 같이 설계가 변경되어야한다. 

```java
public class OrderServiceImpl implements OrderService{

    private final MemberRepository memberRepository = new MemoryMemberRepository();

    //private final DiscountPolicy discountPolicy = new RateDiscountPolicy();
    private DiscountPolicy discountPolicy;
  	...
}
```

현재는 구현체없이 인터페이스만 있으므로 널포인트 예외가 발생한다. 이 문제를 해결하기 위해서는 다른 누군가가 `OrderServiceImpl`에 DiscountPolicy 구현 객체를 대신 생성하고 주입해 주어야합니다.

따라서 구현 객체를 생성하고 연결하는 담당하는 Appconfig 클래스를 만든다.

`OrderServiceImpl`입장에서는 생성자를 통해 어떤 구현체가 들어올지 알 수 없고, 오직 외부 AppConfig에서 결정된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d820705e-7e64-4113-92e7-aeac3c684745/Untitled.png)

이를 통해서 OCP, DIP 모두 만족하게 된다.

1. 구현체가 아닌 추상체에 의존하라는 DIP  ⭕
    
    `OrderServiceImpl`은 인터페이스인 `DiscountPolicy`만 의존하고 있다.
    
2. 확장에는 열려있고 변경에는 닫혀있어야 한다는 OCP ⭕
    
    기능을 변경 확장할 때 기존 클라이언트 코드를 변경할 필요가 없다.
    
    
  
----
 ### **Bean(빈)**

- 스프링이 IoC 방식으로 관리하는 오브젝트, 쉽게 말해 **자바 객체들**
- 스프링이 직접 제어권을 갖고 생성과 제어를 담당하는 오브젝트
- 각 메서드에 @Bean을 붙이면 스프링 컨테이너에 자동으로 등록된다.
- 자바에서 new를 통해 생성되는 객체를 의미하는 것이 아니라 컨테이너에서 생성하고 관리하는 객체를 의미한다.

### 스프링 컨테이너

- 자바 객체(Bean)들을 담고있는 공간
- 등록된 스프링 빈을 생성하고 의존관계를 주입하고 생명주기를 관리해준다.
- 빈의 생성부터 소멸까지를 개발자 대신 관리해주는 곳
- **Bean Factory(빈 팩토리)**
    - 스프링의 IoC를 담당하는 핵심 컨테이너
    - 빈의 등록, 생성, 조회, 그 외 부가적인 빈을 관리하는 기능
- **Application Context(애플리케이션 컨텍스트)**
    - 빈 팩토리를 확장한 IoC 컨테이너
    - 스프링이 제공하는 각종 부가 서비스 추가
    - 애플리케이션 컨텍스트가 구현해야 하는 기본 인터페이스 지칭

### **BeanFactory VS ApplicationContext**

스프링 컨테이너의 종류는 `BeanFactory`와 이를 상속한 `ApplicationContext` 2가지 유형이 존재한다. `BeanFactory`를 직접 사용하는 경우는 거의 없으므로 일반적으로 `ApplicationContext` 를 스프링 컨테이너라 한다. 단,`ApplicationContext`는 `BeanFactory`를 상속받고 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/112f8e1d-797c-479e-82c8-f76b4612fe08/Untitled.png)

### BeanFactory

- 스프링 컨테이너의 최상위 인터페이스이다.
- 스프링 설정파일에 등록된 Bean 객체를 관리하고 조회하는 기본적인 기능만 제공한다.
- 컨테이너가 구동될 때 Bean객체를 생성하는 것이 아니라 **클라이언트의 요청에 의해서 Bean 객체가 사용되는 시점(Lazy Loading)**에 객체를 생성하는 방식을 사용하고 있다.

### ApplicationContext

- 일반적으로 `ApplicationContext` 를 스프링 컨테이너라 한다.
- `BeanFactory` 기능을 모두 상속받아서 제공한다.
- **컨테이너가 구동되는 시점에 객체들을 생성하는 Pre-Loading 방식**을 사용하고 있다.
- 추가적으로 제공되는 부가기능으로는 4가지가 있다.
    - 메시지소스를 활용한 국제화 기능 : 한국에서 접속하면 한국어로, 영어권에서 접근하면 영어로 출력
    - 환경변수 : 로컬, 개발, 운영등을 구분해서 처리
    - 애플리케이션 이벤트 : 이벤트를 발행하고 구독하는 모델을 편리하게 지원
    - 편리한 리소스 조회 : 파일, 클래스패스, 외부 등에서 리소스를 편리하게 조회

**⇒ 실제 개발에서는 ApplicationContext를 사용한다!**

- Context 초기화 시점에 모든 싱글톤 빈을 미리 로드한 후, 
애플리케이션 가동 후에는 빈을 지연 없이 받을 수 있다.
- 부가 기능 4가지
- 이러한 장점으로 ApplicationContext을 실제 개발에서 주로 사용한다.


------
### 스프링 컨테이너

- **AppConfig**
    - 스프링 컨테이너는 @Configuration이 붙은  클래스를 설정정보로 사용한다.
    - 여기서 `@Bean`이 붙은 메서드를 모두 호출해서 반환된 객체를 스프링 컨테이너에 등록한다. 이렇게 스프링 컨테이너에 등록된 객체를 **스프링 빈**이라고 한다.

```java
@Configuration
public class AppConfig {

    @Bean
    public MemberService() {
        return new MemberServiceImpl(memberRepository());
    }

    @Bean
    public OrderService orderService() {
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }

    @Bean
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }

    @Bean
    public DsicountPolicy discountPolicy() {
        return new RateDiscountPolicy();
    }
}
```

- **애플리케이션 실행하는 부분**
    - 이전에는 개발자가 필요한 객체를 AppConfig를 사용해서 **직접 조회**했지만, 이제부터는 스프링 컨테이너를 통해서 필요한 스프링 빈(객체)를 찾아야한다.


------
### 스프링 컨테이너 생성 과정

[https://velog.io/@syleemk/Spring-Core-스프링-컨테이너와-스프링-빈](https://velog.io/@syleemk/Spring-Core-%EC%8A%A4%ED%94%84%EB%A7%81-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88%EC%99%80-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B9%88)

**1. 스프링 컨테이너 생성**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1185c064-d1cd-4357-a5a9-082cd4aac000/Untitled.png)

- `new AnnotationConfigApplicationContext(AppConfig.class)`
- 스프링 컨테이너를 생성할 때는 구성정보(설정정보)를 지정해주어야한다.
- 여기서는 `AppConfig.class`를 구성정보로 지정했다.

**2. 스프링 빈 등록**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0d8914df-c09a-4f09-890d-7ebe173e2f8a/Untitled.png)

- 스프링 컨테이너는 파라미터로 넘어온 설정 클래스 정보를 사용해서 스프링 빈을 등록한다.
- 빈 이름은 메서드 이름을 사용한다
- 빈 이름을 직접 부여할 수도 있다.
    - `@Bean(name="memberService2")`

**3. 스프링 빈 의존관계 설정 - 준비**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/93da99ff-d3cd-4f97-b124-16e700f8e3eb/Untitled.png)

**4. 스프링 빈 의존관계 설정 - 완료**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/52e57537-d33e-445d-9fdd-186f7e30304c/Untitled.png)

- 스프링 컨테이너는 설정 정보를 참고해서 의존관계를 주입(DI)한다.
- 단순히 자바 코드를 호출하는 것 같지만, 차이가 있다.
- 스프링 컨테이너는 등록된 스프링 빈들을 싱글톤 패턴으로 생성하고 관리해준다.

스프링은 빈을 생성하고, 의존관계를 주입하는 단계가 나누어져있다.

그런데 이렇게 자바 코드로 스프링 빈을 등록하면

**생성자를 호출하면서 의존관계 주입도 "한번에" 처리된다.**

여기서는 이해를 위해 개념적으로 나누어 설명했다. 자세한 내용은 의존관계 주입에서 다시 설명한다.
