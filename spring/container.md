목차
1. [스프링 컨테이너를 사용하는 이유?](#스프링-컨테이너를-사용하는-이유?)
2. [스프링 컨테이너란](#스프링-컨테이너란)
3. [스프링 컨테이너 생성 과정](#스프링-컨테이너-생성-과정)
4. [싱글톤](#싱글톤)


>  **객체지향 설계의 5원칙 중 **OCP, DIP**이란**
>
>- **OCP(Open Closed Principle) : 개방 폐쇄 원칙**
>    - 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.
>    - 기능을 확장하려면 코드를 변경해야하는데 코드의 변경없이 확장이 가능한가? → **다형성**을 잘 활용하면 지키는 것이 가능하다
>- **DIP(Dependency Inversion Principle) : 의존 역전 원칙**
>    - 추상화에 의존하고, 구체화에 의존하면 안된다
>    - 구현 클래스에 의존하지 말고, 인터페이스에 의존해야한다.
>    - ex) 운전자는 자동차라는 역할에만 의존 (O)
>      자동차의 구현체인 k3, 아반떼같은 자동차 모델에 의존 (X)
    

# 스프링 컨테이너를 사용하는 이유?

: 결론부터 말하자면, 객체지향 설계의 5원칙 중 DIP, OCP를 지키기 위해서이다.

- 만약 순수 자바로만 코드를 짜다보면, 역할과 구현을 충실하게 분리하여서 코딩하더라도 OCP, DIP를 위배하게 된다.
- 아무리 인터페이스를 분리하고, 그 인터페이스에 해당하는 구현체들을 만드는 방식으로 프로그램을 구상하더라도, 결국 해당 클래스 내부에는 구체적인 구현체를 선언하고 할당해야하는 과정이 존재한다.
- 즉 구현체를 바꿀 때마다, 프로그래머가 해당 클래스 내부의 코드들을 일일히 수정해야 한다는 것을 의미한다.

⇒ 이는 개방 폐쇄 원칙(OCP)원칙과 의존 역전 원칙(DIP)을 위배한다❗

예시) 만약 자바코드로만 구현할 경우

![image](https://user-images.githubusercontent.com/77563814/178150492-a12b775b-881f-4f3e-a8a2-fa3f725496ad.png)
*왼쪽은 기대했던 의존관계, 오른쪽은 자바코드로만 구현할 경우 실제 의존관계*

```java
public class OrderServiceImpl implements OrderService{

    //private final DiscountPolicy discountPolicy = new FixDiscountPolicy();
    private final DiscountPolicy discountPolicy = new RateDiscountPolicy();
	...
}
```

위의 예시처럼 자바 코드로만 개발할 경우, 구현체를 선언하고 의존관계를 설정하게 될 것이다.

1. 구현체가 아닌 추상체에 의존하라는 DIP 원칙 ❌위반!
    
    `OrderServiceImpl`은 `DiscountPolicy` 뿐만 아니라 
    그 구현체인 `FixDiscountPolicy` 혹은 `RateDiscountPolicy`를 의존하고 있다.
    
2. 확장에는 열려있고 변경에는 닫혀있어야 한다는 OCP ❌위반!
    
    기존 코드인 `OrderSerivceImpl`에서의 코드 변경 없이는 확장할 수 없다.
    

이를 해결하기 위해 구현체가 아닌 인터페이스에만 의존할 수 있도록 아래와 같이 설계가 변경되어야한다. 

```java
public class OrderServiceImpl implements OrderService{

    //private final DiscountPolicy discountPolicy = new RateDiscountPolicy();
    
    // 인터페이스에만 의존하도록
    private DiscountPolicy discountPolicy;

  	...
}
```

단, 현재는 구현체없이 인터페이스만 있으므로 널포인트 예외가 발생한다. 

이를 해결하기 위해서는 다른 누군가가 `OrderServiceImpl`에 `DiscountPolicy` 구현 객체를 대신 생성하고 주입해주어야 한다.

따라서 구현 객체를 생성하고 연결하는 담당하는 Appconfig 클래스를 만든다.


![image](https://user-images.githubusercontent.com/77563814/178150717-527a3879-a801-413e-9630-c53896eee350.png)
`OrderServiceImpl`입장에서는 생성자를 통해 어떤 구현체가 들어올지 알 수 없고, 오직 외부 AppConfig에서 결정된다.

Appconfig 클래스를 통해서 OCP, DIP 모두 만족하게 된다.

1. 구현체가 아닌 추상체에 의존하라는 DIP  ⭕
    
    `OrderServiceImpl`은 인터페이스인 `DiscountPolicy`만 의존하고 있다.
    
2. 확장에는 열려있고 변경에는 닫혀있어야 한다는 OCP ⭕
    
    기능을 변경 확장할 때 기존 클라이언트 코드를 변경할 필요가 없다.
    
    
    
 -------
# 스프링 컨테이너란
 
 ### Bean(빈)

- 스프링이 IoC 방식으로 관리하는 오브젝트, 쉽게 말해 **자바 객체들**
- 스프링이 직접 제어권을 갖고 생성과 제어를 담당하는 오브젝트
- 각 메서드에 @Bean을 붙이면 스프링 컨테이너에 자동으로 등록된다.
- 자바에서 new를 통해 생성되는 객체를 의미하는 것이 아니라 컨테이너에서 생성하고 관리하는 객체를 의미한다.

### 스프링 컨테이너

- 자바 객체(Bean)들을 담고있는 공간
- 등록된 스프링 빈을 생성하고 의존관계를 주입하고 생명주기를 관리해준다.
- 빈의 생성부터 소멸까지를 개발자 대신 관리해주는 곳
- Bean Factory(빈 팩토리)과 Application Context(애플리케이션 컨텍스트)이 있다.
- 실제 개발에서는 ApplicationContext를 사용한다. (아래 자세히)

### **BeanFactory VS ApplicationContext**

스프링 컨테이너의 종류는 `BeanFactory`와 이를 상속한 `ApplicationContext` 2가지 유형이 존재한다. `BeanFactory`를 직접 사용하는 경우는 거의 없으므로 일반적으로 `ApplicationContext` 를 스프링 컨테이너라 한다. 단,`ApplicationContext`는 `BeanFactory`를 상속받고 있다.

![image](https://user-images.githubusercontent.com/77563814/178150967-7e9bc631-78c1-430b-99c3-9476ee167f07.png)

### BeanFactory

- 스프링 컨테이너의 최상위 인터페이스이다.
- 스프링 설정파일에 등록된 Bean 객체를 관리하고 조회하는 기본적인 기능만 제공한다.
- 컨테이너가 구동될 때 Bean객체를 생성하는 것이 아니라 클라이언트의 요청에 의해서 Bean 객체가 사용되는 시점(Lazy Loading)에 객체를 생성하는 방식을 사용하고 있다.

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


# 스프링 컨테이너 생성 과정

**1. 스프링 컨테이너 생성**

![image](https://user-images.githubusercontent.com/77563814/178151361-90d4ae10-b248-4d51-9164-d6e340a43a23.png)
- `new AnnotationConfigApplicationContext(AppConfig.class)`
- 스프링 컨테이너를 생성할 때는 설정정보를 지정해주어야한다.
- 여기서는 `AppConfig.class`를 설정정보로 지정했다.

**2. 스프링 빈 등록**

![image](https://user-images.githubusercontent.com/77563814/178151367-49660570-1bf5-46b0-a6d9-d54583e9f754.png)
- 스프링 컨테이너는 파라미터로 넘어온 설정 클래스 정보를 사용해서 스프링 빈을 등록한다.
- 빈 이름은 메서드 이름을 사용한다
- 빈 이름을 직접 부여할 수도 있다.
    - `@Bean(name="memberService2")`

**3. 스프링 빈 의존관계 설정 - 준비**

![image](https://user-images.githubusercontent.com/77563814/178151369-c13802e2-bc17-4eac-b1b2-35084aad69c8.png)


4. 스프링 빈 의존관계 설정 - 완료

![image](https://user-images.githubusercontent.com/77563814/178151373-8a754bf6-5eaa-4a8f-88ef-6a6b9596518a.png)
- 스프링 컨테이너는 설정 정보를 참고해서 의존관계를 주입(DI)한다.
- 단순히 자바 코드를 호출하는 것 같지만, 차이가 있다.
- 스프링 컨테이너는 등록된 스프링 빈들을 싱글톤 패턴으로 생성하고 관리해준다.



# 싱글톤
- 웹 애플리케이션은 보통 여러 고객이 동시에 요청한다.
- 스프링 없는 순수한 DI 컨테이너 AppConfig의 경우 요청을 받을 때마다 객체를 새로 생성한다.
- 이 경우, 매 고객마다 객체가 생성/소멸되어 메모리 낭비가 심하다.
- => 따라서 해당 객체가 딱 1개만 생성되고, 공유하도록 설계하면 된다! 이를 싱글톤 패턴이라 한다.

> **싱글톤 패턴이란**
> 클래스의 인스턴스가 딱 1개만 생성되는 것을 보장하는 디자인 패턴으로,
> private 생성자를 사용해서 외부에서 임의로 new 키워드를 사용하지 못하도록 막는다.


**Reference**
- 인프런 김영한님의 <스프링 핵심 원리 - 기본편>
 
