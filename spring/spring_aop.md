# Spring AOP(Aspect Oriented Programming)

**AOP**는 **Aspect Oriented Programming**의 약자로 **관점 지향 프로그래밍**이라고 불린다. 관점 지향은 쉽게 말해서 **어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 각각 모듈화하겠다**는 것이다. 

> 모듈화: 어떤 공통된 로직이나 기능을 하나의 단위로 묶는 것

예를들어 핵심적인 관점은 결국 우리가 적용하고자 하는 핵심 비즈니스 로직이 된다. 또한 부가적인 관점은 핵심 로직을 실행하기 위해서 행해지는 데이터베이스 연결, 로깅, 파일 입출력 등을 예로 들 수 있다.

**AOP**에서 관점을 기준으로 로직을 모듈화한다는 것은 코드들을 부분적으로 나누어서 모듈화하겠다는 의미이다. 이 때, 소스코드 상에서 다른 부분에 계속 반복해서 쓰는 코드들을 발견할 수 있는데 이것을 `흩어진 관심사(Crosscutting Concerns)`라고 부른다.

![AOP](./images/aop.png)

위 그림과 같이 흩어진 관심사를 **Aspect로 모듈화하고 핵심적인 비즈니스 로직에서 분리하여 재사용하겠다는 것**이 AOP의 취지이다.

### AOP의 주요 개념
- Target: **Aspect를 적용하는 곳**(클래스, 메서드 등)
	- 핵심 기능을 담고있는 `모듈`이다.
	- 어떤 관심사들과도 관계를 맺지 않는다.
- Aspect: **흩어진 관심사를 모듈화**한 것. 주로 부가기능을 모듈화한다.
	- 구체적인 기능을 구현한 Advice와 Advice가 어디에서 적용될지를 결정하는 PointCut을 포함한다.`(Advice+PointCut)`
	- AOP의 기본 모듈 - 싱글톤 형태의 객체로 존재한다.
- Advice: **실질적으로 어떤 기능을 해야할 지**에 대한 것(실질적인 부가기능을 담은 구현체)
	- Aspect가 무엇을 언제 적용할지를 정의한다.
- JointPoint: **Advice가 적용될 위치**, 끼어들 수 있는 지점.
	- 타겟 객체가 구현한 인터페이스의 모든 메서드는 조인 포인트가 된다.
	- 메서드 진입 지점, 생성자 호출 시점, 필드에서 값을 꺼내올 때 등 다양한 시점에 적용 가능
- PointCut: **JointPoint의 상세한 스펙을 정의**한 것. 'A 메서드의 진입 시점에 호출할 것'과 같이 더욱 구체적으로 Advice가 실행될 지점을 정할 수 있다.
	- 공통 기능이 적용될 대상 결정
	- Advice를 적용할 타겟의 메서드를 선별하는 정규표현식이다.
	- **execution**으로 시작하고 메서드의 Signature를 비교하는 방법을 주로 이용한다.

### Spring AOP의 특징

- **프록시 패턴 기반의 AOP 구현체**, 프록시 객체를 쓰는 이유는 접근제어 및 부가기능을 추가하기 위해서이다.
- **스프린 빈에만 AOP를 적용**할 수 있다.
- 모든 AOP 기능을 제공하는 것이 아니라 스프링 IoC와 연동하여 엔터프라이즈급 애플리케이션에서 가장 흔한 문제(중복 코드, 프록시 클래스 작성의 번거로움, 객체들간의 복잡도 증가 등)에 대한 해결책을 지원하는 것이 목적이다.

### Spring AOP의 사용

스프링 AOP를 사용하기 위해서는 다음과 같은 의존성을 추가해야 한다.

**gradle**

```
implementation 'org.springframework:spring-aop:2.5.6'
```

**Maven**

```
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aop</artifactId>
    <version>2.5.6</version>
</dependency>
```

**예시**

다음과 같이 **@Aspect** 애노테이션을 붙여서 이 클래스가 `Aspect(흩어진 관심사를 모듈화한 것)`를 나타내는 클래스라는 것을 명시하고, **@Component**로 붙여서 스프링 빈으로 등록하는 코드를 볼 수 있다.

**@Around** 어노테이션은 **Target**메서드를 감싸서 특정 **Advice**를 실행한다는 의미를 담고있다. 해당 코드의 **Advice**는 **Target** 메서드가 실행된 시간을 측정하기 위한 로직을 구현하였다. 또한`execution(*com.saelobi..*.EventService.*(..))`가 의미하는 바는 com.saelobi 하위 패키지 경로의 EventService 객체의 모든 메서드에 이 Aspect를 적용하겠다는 의미이다.

**소스코드 예시 1 - 상세 경로 지정**

```java
@Component
@Aspect
public class PerfAspect {

	@Around("execution(*com.saelobi..*.EventService.*(..))")
	public Object logPerf(ProceedingJoinPoint pjp) throws Throwable {
		long begin System.currentTimeMillis();
		Object retVal = pjp.proceed();
		System.out.println(System.currentTimeMillis() - begin);
		return retVal;
	}
}
```

**소스코드 예시 2 - 애노테이션 지정**

```java
@Component
@Aspect
public class PerfAspect {

	@Around("@annotation(PerLogging)")
	public Object logPerf(ProceedingJoinPoint pjp) throws Throwable {
		long begin System.currentTimeMillis();
		Object retVal = pjp.proceed();
		System.out.println(System.currentTimeMillis() - begin);
		return retVal;
	}
}
```

**소스코드 예시 3 - 스프링 빈 지정**

```java
@Component
@Aspect
public class PerfAspect {

	@Around("bean(SimpleEventService)")
	public Object logPerf(ProceedingJoinPoint pjp) throws Throwable {
		long begin System.currentTimeMillis();
		Object retVal = pjp.proceed();
		System.out.println(System.currentTimeMillis() - begin);
		return retVal;
	}
}
```