# Spring Bean

Spring IoC 컨테이너가 관리하는 자바 객체를 빈(Bean)이라고 부른다. `제어의 역전(IoC, Inversion of Control)`의 간략한 특징은 다음과 같다.

#### IoC(Inversion of Control)와 Bean

일반적으로 기본적인 자바 프로그램에서는 각 객체들이 프로그램의 흐름을 결정하고 각 객체를 직접 생성하고 조작하는 작업(객체를 직접 생성하여 메서드 호출)을 했다. 즉, 모든 작업을 사용자가 제어하는 구조였다. 예를들면 A객체에서 B객체에 있는 메서드를 사용하고 싶으면, B객체를 직접 A객체 내에서 생성하고 메서드를 호출한다.

하지만 IoC가 적용된 스프링의 경우에는 객체의 생성을 특별한 관리 위임 주체에게 맡긴다. 이 경우 사용자는 객체를 직접 생성하지 않고, 객체의 생명주기를 컨트롤하는 주체는 다른 주체가 된다. 즉, 사용자가 제어권을 다른 주체에게 넘기는 것을 `IoC(제어의 역전)`이라고 한다.

이 때, **스프링에 의해 생성되고 관리되는 자바 객체를 `Bean`이라고 한다.** 스프링 프레임워크에서는 스프링 빈(Bean)을 얻기 위해서 ApplicationCOntext.getBean()과 같은 메서드를 사용해서 스프링에서 직접 자바 객체를 얻어서 사용한다.

## Spring Bean을 Spring IoC Container에 등록하는 법

### 애노테이션 기반 등록

스프링에서는 여러가지 애노테이션을 사용하지만, Bean을 등록하기 위해서는 `@Component`를 사용한다. @Component 애노테이션이 붙어있는 경우에는 스프링이 애노테이션을 확인하고 자체적으로 Bean으로 등록하여 IoC 컨테이너에 보관한다.

```java
@Controller
public class RegistrationController { ... }
```

`@Controller` 애노테이션은 내부적으로 @Component 애노테이션이 들어가있다. 따라서 스프링은 이 컨트롤러를 Bean으로 등록하게된다.

### @Bean 으로 직접 등록

`@Configuration`과 `@Bean` 애노테이션을 이용해서 Bean으로 등록할 수 있다. 아래의 코드처럼 @Configuration을 이용하면 스프링에서 해당 클래스를 설정정보 클래스로 인식한다. 이 설정정보 클래스에 Bean으로 등록하고자 하는 클래스를 @Bean 애노테이션을 사용하여 Bean으로 등록할 수 있다.

```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig  {

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```