# JAVA SE와 JAVA EE 차이점

#### Java 프로그래밍 언어에서는 4가지 플랫폼이 존재한다.
각각의 플랫폼은 Java 프로그래밍 언어로 애플리케이션 서버를 프로그래밍하기 위해 폭넓게 사용되는 플랫폼

* 자바 플랫폼 Java SE (Standard Edition)
* 자바 플랫폼 Java EE (Enterprise Edtion)
* 자바 플랫폼 Java ME (Micro Edtion)
* Java FX 

모든 자바 플랫폼은 JVM과 API로 구성되어 있다.
> * JVM : 하드웨어와 소프트웨어 플랫폼에서 자바 기술을 사용한 어플리케이션을 동작시키기 위한 프로그램
> * API : 개발자들이 직접 컴포넌트나 어플리케이션을 만들 때 사용할 수 있는 소프트웨어 컴포넌트의 집합

## Java SE (Standard Edition)
* 가장 대중적인 자바 플랫폼
* 흔히 자바 언어라고 하는 대부분의 패키지가 포함 된 에디션
* Java SE의 API는 자바 프로그래밍 언어의 핵심 기능들을 제공
  * 기초적인 타입
  * 네트워킹
  * 보안
  * DB 처리
  * 그래픽 사용자 인터페이스 개발
  * XML 파싱 등
* 가상머신, 개발도구, 배포기술, 부가 클래스 라이브러리, 툴킷 등 제공


## JAva EE (Enterprise Edtion)
* Java EE 플랫폼은 Java SE 플랫폼을 기반으로 **그 위에 탑재**된다.
* `웹 프로그래밍`에 필요한 기능을 다수 포함
  * JSP, Servlet, JDBC, JNDI, JTA 등
* 대규모, 다계층, 확장성, 신뢰성, 보안 네트워킹 API 등을 제공


-----

### (참고)
## Java ME(Micro Edition)
* 모바일 폰과 같은 자바 기반의 어플리케이션이 보다 조그만 가상 머신으로 동작시킬 수 있는 API 제공
* Java EE처럼 SE 위에서 동작함
* 작은 장치에서 동작하는 전용 클래스 라이브러리 제공
* Java EE 서비스의 클라이언트 역할을 하기도 함

