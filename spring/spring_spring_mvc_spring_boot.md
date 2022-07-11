# Spring & Spring MVC & Spring Boot

#### Spring

Java 기반의 웹 애플리케이션 개발을 위한 오픈소스 프레임워크

#### Spring MVC

웹 애플리케이션 개발에 있어서 `MVC 패턴`을 적용할 수 있도록 Spring에서 제공하는 프레임워크

#### Spring Boot

Spring **설정들을 자동화**해주는 Spring 기반의 프레임워크

### Spring과 Spring MVC의 차이

Spring MVC는 Spring을 기반으로 개발자가 좀 더 쉽고 편리하게 사용할 수 있도록 제공되는 프레임워크이다. 웹 애플리케이션 개발에 있어서 MVC 패턴을 적용할 수 있도록 Spring에서 편리하게 제공하는 프레임워크인 것이다.

즉, Spring MVC는 Dispatcher Servlet, ModelAndView, View Resolver와 같은 단순개념을 사용해서 웹 애플리케이션 개발을 쉽게 할 수 있도록 해주는 프레임워크이다.

### Spring MVC와 Spring Boot의 차이

Spring MVC와 Spring Boot의 가장 큰 차이는 `설정의 자동화`이다. Spring MVC 의 경우에 설정정보 파일에 Dispatcher servlet, Handler Mapping, View Resolver 설정등을 해줘야 웬만한 기능들이 정상적으로 동작한다. 하지만 Spring Boot를 사용하면 이러한 설정정보들을 자동으로 설정해준다.

이 뿐만아니라 **WAS 내장 여부 차이**도 존재한다. Servlet 기반의 프로그램을 실행하기 위해서는 WAS가 필요한데, Spring MVC 프로젝트의 경우 따로 Tomcat과 같은 WAS 서버를 설치해줘야 한다. 반면에 Spring Boot를 사용하면 Tomcat이 내장되어 있어서 별도의 웹 애플리케이션 서버의 설치가 필요없다.

참고로 말하면 설정정보 파일에서 Spring Web과 관련된 정보들은 Spring MVC 기반의 프로젝트를 생성하는 것을 뜻한다.