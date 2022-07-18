**서블릿(servlet)이란**

- 자바 웹 어플리케이션의 구성 요소 중 JAVA 언어를 사용하여 **동적인 처리**를 하는 역할을 담당한다. 서블릿(servlet)은 WAS에 동작하는 JAVA 클래스이며, `HttpServlet`을 상속받는다.

## Dispatcher-Servlet의 등장 배경

Spring을 사용하지 않을 경우, 주소마다 Servlet 객체를 생성해주고, web.xml에 각각 매핑해주어야 한다. 이때 접속하는 페이지(메뉴, 로그인, 관리자 페이지 등)가 많으므로 한 프로젝트에 너무 많은 서블릿 객체가 생성되게 된다.

이를 해결하기 위해 DispatcherServlet이 모든 요청을 받고, 각각 해당하는 Controller을 연결해주는 프론트 컨트롤러의 역할을 함으로써, web.xml에 서블릿을 일일히 등록할 필요가 없어졌다. 또한 공통적으로 처리해주어야하는 일들을 DispatcherServlet으로 간단히 처리할 수 있게 되었다.

## **Dispatcher Servlet란**

![image](https://user-images.githubusercontent.com/77563814/179492570-7c5dbfc9-c5fd-4082-baa3-1bc066cbee35.png)
[이미지 출처](https://www.geeksforgeeks.org/what-is-dispatcher-servlet-in-spring/)

<aside>
  
    💡 HTTP 프로토콜로 들어오는 모든 요청을 가장 먼저 받아 적합한 컨트롤러에 위임해주는 프론트 컨트롤러 (Front Controller)
    즉, 모든 요청을 받아 처리할 컨트롤러를 찾아 위임하고, 처리 결과를 응답해주는 역할

</aside>

- 처리 순서
  1. 클라이언트로부터 어떤 요청이 들어오면 서블릿 컨테이너(ex. 톰캣)이 요청을 받는다. 
  2. 이때 공통 작업은 DispatcherServlet에서 처리하고
  3. 이외 작업은 적절한 세부 컨트롤러로 위임한다.
- 특징
    - Front Controller, 즉 주로 서블릿 컨테이너의 제일 앞에서 서버로 들어오는 클라이언트의 모든 요청을 받아서 처리해주는 컨트롤러이다.
    - DispatcherServlet도 Servlet이다. DispatcherServlet -> FrameworkServlet -> HttpServletBean -> `HttpServlet`상속 구조를 갖고 있다.
- 장점
    1. 서블릿을 web.xml에 모두 등록해줄 필요가 없다.
        
        과거에는 모든 서블릿을 URL 매핑을 위해 web.xml에 모두 등록해주어야 했지만, dispatcher-servlet이 해당 어플리케이션으로 들어오는 모든 요청을 핸들링해주고 공통 작업을 처리면서 상당히 편리하게 이용할 수 있게 되었다.
        
    2. 공통 요청 사항을 컨트롤할 수 있다.
        
        사용자의 모든 요청에 대한 인코딩 처리, 에러페이지 처리 등을 한 곳에서 처리할 수 있다.
        

## **Dispatcher Servlet 동작 흐름**

![image](https://user-images.githubusercontent.com/77563814/179492583-1f0fd626-76d0-4a2c-b4f8-a13e931fcac9.png)

역할

- `HandlerMapping` : Request를 분석하여 해당되는 Controller를 찾는다.
- `HandlerAdapter` : 결정된 Controller의 메서드를 호출한다.
- `ViewResolver` : 해당하는 view를 선택하여 리턴한다.

**동작 과정**

1. 클라이언트의 요청을 `DispatcherServlet`이 받음
2. `HandlerMapping`으로 요청을 위임할 `Controller`를 찾음
3. `HandlerAdapter`가 결정된 `Controller`로 요청을 위임함
4. 비지니스 로직을 처리함 (Controller -> Service -> Repository 동작)
5. 로직 처리 결과를 리턴
6. `HandlerAdapter`가 `Controller`실행결과를 ModelAndView로 변환해서 `DispatcherServlet`에 리턴함
7. `ViewResolver`객체를 이용해서 결과를 보여줄 뷰를 검색한다
8. `ViewResolver`가 리턴한 View객체에게 응답 결과 생성을 요청한다.
9. 서버의 응답을 클라이언트로 반환함
- JSP일 경우, View 객체는 JSP를 실행함으로서 브라우저에게 전송할 응답 결과를 생성한다.

**Reference**

[Dispatcher-Servlet 전체 흐름](https://devmoony.tistory.com/102)

[HandlerMapping , HandlerAdapter , ViewResolver](https://show400035.tistory.com/132) 

[동작 흐름](https://brilliantdevelop.tistory.com/90)

[Tomcat, Spring MVC의 동작 과정](https://taes-k.github.io/2020/02/16/servlet-container-spring-container/)
