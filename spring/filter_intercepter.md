### **Filter, Interceptor, AOP을 사용하는 이유**

- 상황
    - 자바 웹 개발을 하다보면, **공통적으로 처리**해야 할 업무들이 많다.
    - 예를 들어 로그인 관련(세션체크)처리, 권한 체크, XSS(Cross site script)방어, pc와 모바일 웹의 분기 처리, 로그, 페이지 인코딩 변환 등이 있다.
- 문제점
    - 공통 업무에 관련된 코드를 모든 페이지 마다 작성해야한다면
    - 중복된 코드가 많아지게 되고
    - 프로젝트 단위가 커질수록 서버에 부하를 줄 수도 있으며
    - 소스 관리도 되지 않는다.

따라서, 공통 부분은 따로 빼서 관리하는 게 좋다.

위와 같은 공통처리를 위해 활용할 수 있는 것이 3가지가 있다.

1. **Filter**
2. **Interceptor**
3. **AOP**

### **Filter, Interceptor, AOP 비교**

- 애플리케이션에서 자주 사용되는 기능(공통 부분)을 분리하여 관리하는 기능

![image](https://user-images.githubusercontent.com/77563814/179697854-d7dce7a2-10b8-44b6-932e-d8bea7617810.png)




1. **실행 시점이 다름**
    - 요청이 들어오면 Filter → Interceptor → AOP → Interceptor → Filter 순으로 실행된다.
    - Interceptor와 Filter는 Servlet 단위에서 실행되는 반면, 
    AOP는 메소드 앞에 Proxy패턴의 형태로 실행된다.
2. **실사용**
    - **AOP**
        - 비즈니스단에서 세밀하게 조정하고 싶을때
        - Interceptor나 Filter와는 달리 메소드 전후의 지점에 자유롭게 설정이 가능하다.
        - ex) 로직의 시간 측정, 트랜잭션 관리, 에러 처리 등
    - **Filter**
        - 전체적인 Request단에서 어떤 처리가 필요할때
        - ex) 인증, 이미지변환, 데이터압축, 암호화필터, 토크나이징 필터, XML 컨텐츠를 변형하는 XSLT 필터, URL 및 기타정보를 캐시하는 필터, 문자 인코딩등
    - **Interceptor**
        - 스프링 단으로 들어온 클라이언트의 요청과 관련되어 전역적으로 처리해야 할 때
        - ex) 로그인 세션 체크 등

---


## **Filter**

![image](https://user-images.githubusercontent.com/77563814/179701034-fa8bd8f0-1b48-4e81-9d13-2d05878c612b.png) [이미지 출처](https://www.baeldung.com/spring-mvc-handlerinterceptor-vs-filter)

<aside>
    
    💡 클라이언트 요청이 서블릿으로 가기 전에 먼저 처리할 수 있는 기능 (스프링이 지원하는 기능이 아닌, J2EE 표준 스펙에 있는 기능)

</aside>

- 특징
    - Dispatcher Servlet 이전에 수행되고, 응답 처리에 대해서도 변경 및 조작 수행 가능하다.
    - WAS 내의 ApplicationContext에서 등록된 필터가 실행한다.
    - 즉 Spring Context 외부에 존재하여 Spring과 무관한 자원에 대해 동작한다.
    - 따라서 **스프링과 무관하게 전역적으로 처리**해야 하는 작업들을 처리할 경우에 사용한다. 혹은, 요청 파라미터를 중심으로 검증할 때 사용된다.

- 설정 위치
    - 일반적으로 web.xml에 설정
    - 예외 발생 시 Web Application에서 예외 처리
- 예시
    - 공통된 보안 및 인증/인가 관련 작업 (XSS, CSRF 방어 등) (스프링 컨테이너까지 요청이 전달되지 못하고 차단되므로 안정성을 더욱 높임)
    - 이미지/데이터 압축 및 문자열 인코딩 변환 처리
    - 모든 요청에 대한 로깅 또는 감사
- 실행 메소드
    - init() : 필터 인스턴스 초기화
    - doFilter() : 실제 처리 로직
        - `doFilter()`의 파라미터로는 FilterChain이 있는데, FilterChain의 `doFilter()`를 통해 다음 대상으로 요청을 전달하게 된다. `chain.doFilter()`전/후에 우리가 필요한 처리 과정을 넣어줌으로써 원하는 처리를 진행할 수 있다.
    - destroy() : 필터 인스턴스 종료


## **Interceptor**

<aside>
    
    💡 `DispatcherServlet`에서 `Controller`로의 요청을 가로채서 처리를 해준주는 기능 (스프링에서 제공하는 기능)

</aside>

- 동작 범위
    - Spring Context 내부에서 Controller(Handler)의 요청과 응답에 관여하여, Spring의 모든 Bean에 접근 가능하다.
    - DispatcherServlet이 Controller 호출 전/후에 끼어들어 기능을 수행한다.
    - DispatcherServlet이 HandlerMapping을 통해 `Controller`를 찾도록 요청하는데, 그 결과로 실행 체인(HandlerExecutionChain)을 돌려준다. 이 실행 체인은 1개 이상의 인터셉터가 등록되어 있다면, 순차적으로 인터셉터를 거쳐 컨트롤러가 실행된다.
- 설정 위치
    - 일반적으로 servlet-context.xml에 설정
    - 예외 발생 시 @ControllerAdvice에서 @ExceptionHandler를 사용해 예외 처리
- 예시
    - 세부적인 보안 및 인증/인가 공통 작업 (특정 사용자에 대한 인증이 필요한 경우)
    - API 호출에 대한 로깅 또는 감사
    - Controller로 넘겨주는 정보(데이터)의 가공
- 실행 메소드
    - preHandler() : Controller 실행 전
        - return 값이 true이면 다음 단계로 넘어간다.
        - return 값이 false이면 작업을 중지한다.
    - postHandler() : Controller 실행 후 view페이지 렌더링 되기 전
        - 컨트롤러에서 예외가 발생할 시 실행되지 않는다.
    - afterCompletion() : view Rendering 후



## **Filter와 Interceptor의 차이점**

|  | Filter | Interceptor |
| --- | --- | --- |
| 관리되는 컨테이너 | 웹 컨테이너 | 스프링 컨테이너 |
| Request/Response 객체 조작 가능 여부 | O | X |
| 사용 | 공통된 보안 및 인증/인가 관련 작업, 모든 요청에 대한 로깅 또는 감사, 이미지/데이터 압축 및 문자열 인코딩, Spring 과 분리되어야 하는 기능 | 세부적인 보안 및 인증/인가 공통 작업, API 호출에 대한 로깅 또는 감사, Controller 로 넘겨주는 정보(데이터) 의 가공 |

1. 실행 시점
    - Filter는 Web Application에 등록을 하고, Interceptor는 Spring의 Context에 등록한다. 따라서 필터는 웹 컨텍스트에서 실행되고, 인터셉터는 스프링 컨텍스트에서 실행되므로 실행 시점이 다르다.
    - 필터는 디스패처 서블릿의 전후를 다룰 수 있고, 인터셉터는 컨트롤러의 전후를 다룰 수 있다.
2. 사용
    - **Filter는** 스프링과 무관하게 web app의 전역적으로 처리해야 하는 작업을 하거나, 입력으로 들어온 파라미터 그 자체에 대해 검증을 하거나, HttpServletRequest 대신에 ServletRequest를 이용하는 경우 사용하는 것이 좋다.
    - **Interceptor는** 스프링 단으로 들어온 클라이언트의 요청과 관련되어 전역적으로 처리해야 하거나, 서비스 로직을 섞어야 할 때 사용하는 것이 좋다.
3. Request/Response 객체 조작 가능
    - Filter는 Request/Response 객체 조작 가능하다(다른 객체로 바꿀 수 있음). → [참고](https://supawer0728.github.io/2018/04/04/spring-filter-interceptor/)
        - 필요 시 커스터마이징 된 ServletRequest를 대신 사용한다.
    - Interceptor는 Request/Response을 다른 객체로 넘겨줄 수 없다. 대신에 객체 내부의 값은 조작 가능하다. 예를 들어 사용자의 ID를 기반으로 조회한 사용자 정보를 HttpServletRequest에 넣어줄 수 있다.
4. 예외 처리 
    - 인터셉터는 `@ControllerAdvice`와 `@ExceptionHandler`를 사용하여 예외 처리가 가능하지만, 필터는 이를 사용하여 예외 처리를 할 수 없다.
    - 필터는 주로 `doFilter()` 메소드 주변을 try~catch 구문으로 감싸서 그 시점에서 발생한 예외를 곧바로 핸들링한다.

**Reference**

[필터와 인터셉터 차이점](https://supawer0728.github.io/2018/04/04/spring-filter-interceptor/)

[차이 및 용도](https://goddaehee.tistory.com/154)

**[Filter, Interceptor, AOP 차이](https://goddaehee.tistory.com/154)**

[그림 참고](https://steady-coding.tistory.com/601)
