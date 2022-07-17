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

![image](https://user-images.githubusercontent.com/77563814/179411201-a3fb9802-2764-4900-a897-140916f9ebf5.png)

1. **실행 시점이 다름**
    - 요청이 들어오면 Filter → Interceptor → AOP → Interceptor → Filter 순으로 실행된다.
    - Interceptor와 Filter는 Servlet 단위에서 실행되는 반면, 
    AOP는 메소드 앞에 Proxy패턴의 형태로 실행된다.
2. **실사용**
    - **AOP**
        - 비즈니스단에서 세밀하게 조정하고 싶을때
        - ex) 로직의 시간 측정, 트랜잭션 관리, 에러 처리 등
    - **Filter**
        - 전체적인 Request단에서 어떤 처리가 필요할때
        - ex) 인증, 이미지변환, 데이터압축, 암호화필터, 토크나이징 필터, XML 컨텐츠를 변형하는 XSLT 필터, URL 및 기타정보를 캐시하는 필터, 문자 인코딩등
    - **Interceptor**
        - 세션 및 쿠키 체크하는 http 프로토콜 단위로 처리해야 하는 업무가 있을 때
        - ex) 로그인 세션 체크 등

---

## **Filter**


<aside>
  
  
💡 클라이언트 요청이 서블릿으로 가기 전에 먼저 처리할 수 있는 기능 (스프링이 지원하는 기능이 아닌, J2EE 표준 스펙에 있는 기능)

  
</aside>


- 동작 범위
    - Dispatcher Servlet 이전에 수행되고, 응답 처리에 대해서도 변경 및 조작 수행 가능하다.
    - WAS 내의 ApplicationContext에서 등록된 필터가 실행한다.
    - Spring Context 외부에 존재하여 Spring과 무관한 자원에 대해 동작한다.
- 설정 위치
    - 일반적으로 web.xml에 설정
    - 예외 발생 시 Web Application에서 예외 처리
- 예시
    - ex) 인코딩 변환 처리, XSS 방어, LOG, 인증 등
- 실행 메소드
    - init() : 필터 인스턴스 초기화
    - doFilter() : 실제 처리 로직
    - destroy() : 필터 인스턴스 종료

## **Interceptor**


<aside>
💡 `DispatcherServlet`에서 `Controller`로의 요청을 가로채서 처리를 해준주는 기능 (스프링에서 제공하는 기능)
</aside>


- 동작 범위
    - DispatcherServlet이 Controller 호출 전/후에 끼어들어 기능을 수행한다.
    - 따라서 Spring Context 내부에서 Controller의 요청과 응답에 관여하여 모든 Bean에 접근 가능
- 설정 위치
    - 일반적으로 servlet-context.xml에 설정
    - 예외 발생 시 @ControllerAdvice에서 @ExceptionHandler를 사용해 예외 처리
- 예시
    - ex. 로그인 체크, 권한 체크, 프로그램 실행시간 계산작업, 로그 확인 등
- 실행 메소드
    - preHandler() : Controller 실행 전
    - postHandler() : Controller 실행 후 view페이지 렌더링 되기 전
    - afterCompletion() : view Rendering 후

## **Filter, Interceptor 차이점**

1. 사용
    - **Filter :** 주로 스프링과 무관한, web app의 전역적으로 처리해야 하는 로직을 구현할 경우
    - **Interceptor :** 주로 클라이언트 요청과 관련되어 처리할 경우
2. 실행 시점
    - Filter는 Web Application에 등록을 하고, Interceptor는 Spring의 Context에 등록한다.
    - 즉 Filter는 WAS단에 설정되어 Spring과 무관한 자원에 대해 동작하고,
    - Interceptor는 Spring Context 내부에 설정되어 컨트롤러 접근 전, 후에 가로채서 기능 동작
3. Filter는 doFilter() 메소드만 있지만, Interceptor는 pre와 post로 명확하게 분리
    - Filter는 Servlet에서 처리하기 전후를 다룰 수 있다.
    - Interceptor는 Handler를 실행하기전(preHandle), Handler를 실행한 후(postHandle), view를 렌더링한 후(afterCompletion) 등, Servlet내에서도 메서드에 따라 실행 시점을 다르게 가져간다.
4. Interceptor의 경우, AOP 흉내 가능
    - handlerMethod(@RequestMapping을 사용해 매핑 된 @Controller의 메소드)를 파라미터로 제공하여 메소드 시그니처 등 추가 정보를 파악해 로직 실행 여부 판단 가능
