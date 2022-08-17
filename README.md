# Tech Interview

**기술 면접 대비를 위한 기본 개념을 정리하는 Repository 입니다.**



**:book: Contents**
1. [Data Structure](#1-data-structure)
2. [Network](#2-network)
3. [Operating System](#3-operating-system)
4. [Database](#4-database)
5. [Design Pattern](#5-design-pattern)
6. [Algorithm](#6-algorithm)
7. [Java](#7-java)
8. ~~[JavaScript](#contents)~~
9. [Spring](#9-spring)
10. [Security](#10-security)
11. [ETC](#11-etc)

---

## 1. Data Structure
:arrow_forward: [답변 내용](/contents/datastructure.md)
* [X] [Array](/data_structure/array.md)
* [X] [LinkedList](/data_structure/linkedlist.md)
* [X] [HashTable](/data_structure/hash.md)
* [X] [Stack](/data_structure/stack.md)
* [X] [Queue](/data_structure/queue.md)
* [X] [Graph](/data_structure/graph.md)
* [X] [Tree](/data_structure/tree.md)
* [X] [그래프(Graph)와 트리(Tree)의 차이점](/data_structure/graph_tree.md)
* [X] [Binary Heap](/data_structure/binary_heap.md)
* [X] [Red-Black Tree](/data_structure/redblack_tree.md)
* [X] [B+ Tree](/data_structure/btree.md)

## 2. Network
:arrow_forward: [답변 내용](/contents/network.md)
* [X] [OSI 7계층](/network/osi_7layer.md)
* [X] [TCP/IP의 개념](/network/tcp_ip.md)
* [X] [TCP와 UDP](/network/tcp_udp.md)
* [X] [TCP와 UDP의 헤더 분석](/network/tcp_udp_header.md)
* [X] [TCP의 3-way-handshake와 4-way-handshake](/network/handshake.md)
  * Q. TCP의 연결 설정 과정(3단계)과 연결 종료 과정(4단계)이 단계가 차이나는 이유?
  * Q. 만약 Server에서 FIN 플래그를 전송하기 전에 전송한 패킷이 Routing 지연이나 패킷 유실로 인한 재전송 등으로 인해 FIN 패킷보다 늦게 도착하는 상황이 발생하면 어떻게 될까?
  * Q. 초기 Sequence Number인 ISN을 0부터 시작하지 않고 난수를 생성해서 설정하는 이유?
* [X] [HTTP와 HTTPS](/network/http_https0.md)
* [X] [HTTP 요청/응답 헤더](/network/http_header.md)
* [X] [HTTP와 HTTPS 동작 과정](/network/http_https.md)
* [X] [CORS란](/network/cors.md)
* [X] [GET 메서드와 POST 메서드](/network/get_post_method.md)
* [X] [쿠키(Cookie)와 세션(Session)](/network/cookie_session.md)
* [X] [DNS](/network/dns.md)
* [X] [REST와 RESTful의 개념](/network/rest_restful.md)
* [X] [소켓(Socket)이란](/network/socket.md)
* [X] [Socket.io와 WebSocket의 차이](/network/socket.io_websocket.md)
* [X] [Frame, Packet, Segment, Datagram](/network/pdu.md)

## 3. Operating System
:arrow_forward: [답변 내용](/contents/os.md)
* [X] [프로세스와 스레드의 차이(Process vs Thread)](/os/process_thread.md)
* [X] [멀티 프로세스 대신 멀티 스레드를 사용하는 이유](/os/multi_thread.md)
* [X] [Thread-safe](/os/thread_safe.md)
* [X] [동기화 객체의 종류](/os/synchronization_object.md)
* [X] [뮤텍스와 세마포어의 차이](/os/mutex_and_semaphore.md)
* [X] [스케줄러](/os/cpu_scheduling.md)
* [X] [동기와 비동기](/os/sync_and_async.md)
* [X] [프로세스 동기화](/os/process_sync.md)
* [X] [메모리 관리 전략](/os/memory_management_strategy.md)
* [X] [가상 메모리](/os/virtual_memory.md)
* [X] [캐시의 지역성](/os/locality.md)
* [X] [교착상태(데드락, Deadlock)의 개념과 조건](/os/deadlock.md)
* [X] [사용자 수준 스레드와 커널 수준 스레드](/os/user_and_kernal_thread.md)
* [X] [외부 단편화와 내부 단편화](/os/fragmentation.md)
* [X] [Context Switching](/os/context_switching.md)
* [X] [Swapping](os/Swapping.md)

## 4. Database
:arrow_forward: [답변 내용](/contents/db.md)
* [X] [데이터베이스 풀](/db/connection_pool.md)
* [X] [정규화(1차 2차 3차 BCNF)](/db/normalization.md)
* [X] [트랜잭션(Transaction) 이란](/db/transaction.md)
* [X] [트랜잭션 격리 수준(Transaction Isolation Level)](/db/isolation_level.md)
* [X] [Join](/db/join.md)
* [X] [SQL injection](/db/sql_injection.md)
* [X] [Index란](/db/index.md)
* [X] [Statement와 PrepareStatement](/db/statement_preparedstatement.md)
* [X] [RDBMS와 NoSQL](/db/rdbms_nosql.md)
* [X] [효과적인 쿼리 저장](/db/effective_query.md)
* [X] [옵티마이저(Optimizer)란](/db/optimizer.md)
* [X] [Replication](/db/replication.md)
* [X] [파티셔닝(Partitioning)](/db/partitioning.md)
* [X] [샤딩(Sharding)](/db/sharding.md)
* [X] [객체 관계 매핑(Object-relational mapping, ORM)이란](/db/orm.md)
* [X] [java JDBC](/db/jdbc.md)

## 5. Design Pattern
:arrow_forward: [답변 내용](/contents/designpattern.md)
* [X] [디자인 패턴의 개념과 종류](/design_pattern/design_pattern.md)
* [X] [Singleton 패턴](/design_pattern/singleton.md)
* [X] [Strategy 패턴](/design_pattern/strategy_pattern.md)
* [X] [Observer 패턴](/design_pattern/observer.md)
* [X] [Adapter 패턴](/design_pattern/adapter.md)
* [X] [Proxy 패턴과 Proxy 서버](/design_pattern/proxy.md)
* [X] [Template Method 패턴](/design_pattern/template_method.md)
* [X] [Factory Method 패턴](/design_pattern/factory_method.md)
* [X] [MVC1 패턴과 MVC2 패턴](/design_pattern/mvc1_mvc2.md)

## 6. Algorithm 
### :pushpin: [관련 링크](https://github.com/WeareSoft/algorithm-study)
:arrow_forward: [답변 내용](/contents/algorithm.md)
* [ ] BigO
* [ ] DFS와 BFS의 차이
* [ ] Fibonacci에서의 세 가지(Recursion, Dynamic Programming, 반복) 방식에 대한 시간복잡도와 공간복잡도 차이
* [ ] 정렬 알고리즘의 종류와 개념
* [ ] Greedy 알고리즘
* [ ] 최소 신장 트리(MST, Minimum Spanning Tree)란
* [ ] Kruskal MST 알고리즘
* [ ] Prim MST 알고리즘

## 7. Java
:arrow_forward: [답변 내용](/contents/java.md)
* [X] [java 프로그래밍이란](/java/javaprogramming.md)
* [X] [Java SE와 Java EE 애플리케이션 차이](/java/javaSEandEE.md)
* [X] [java와 c/c++의 차이점](/java/java_c.md)
* [X] [java 언어의 장단점](/java/java_pros_and_cons.md)
* [X] [java의 접근 제어자의 종류와 특징](/java/accessModifier.md)
* [X] [java의 데이터 타입](/java/datatype.md)
* [X] [Wrapper class](/java/wrapper_class.md)
* [X] [OOP의 4가지 특징](/java/OOP_4features.md)
  * 추상화(Abstraction), 캡슐화(Encapsulation), 상속(Inheritance), 다형성(Polymorphism)
* [X] [OOP의 5대 원칙 (SOLID)](/java/OOP_5principles.md)
* [X] [객체지향 프로그래밍과 절차지향 프로그래밍의 차이](/java/OOP_PP.md)
* [X] [객체지향(Object-Oriented)이란](/java/object_oriented.md)
* [X] [java의 non-static 멤버와 static 멤버의 차이](/java/nonstatic_static.md)
  * Q. java의 main 메서드가 static인 이유
* [X] [java의 final 키워드 (final/finally/finalize)](/java/final_finally_finalize.md)
* [X] [java의 제네릭(Generic)과 c++의 템플릿(Template)의 차이](/java/generic.md)
* [X] [java의 가비지 컬렉션(Garbage Collection) 처리 방법](/java/gc.md)
  * GC모니터링이란
* [X] [java 직렬화(Serialization)와 역직렬화(Deserialization)란 무엇인가](/java/serialization_deserialization.md)
* [X] [클래스, 객체, 인스턴스의 차이](/java/class_obj_instance.md)
* [X] [객체(Object)란 무엇인가](/java/object.md)
* [X] [오버로딩과 오버라이딩의 차이(Overloading vs Overriding)](/java/overloading_overriding.md)
* [X] [Call by Reference와 Call by Value의 차이](/java/call_by_reference_and_value.md)
* [X] [인터페이스와 추상 클래스의 차이(Interface vs Abstract Class)](/java/abstract_class_interface.md)
* [X] [JVM 구조 및 실행과정](/java/JVM.md)
* [X] [자바 메모리 구조](/java/java_memory.md)
  * 클래스 로드 방법
  * 클래스 로드 과정
* [X] [Java Collections Framework](/java/collections.md)
  * java Map 인터페이스 구현체의 종류
  * java Set 인터페이스 구현체의 종류
  * java List 인터페이스 구현체의 종류
* [X] [Annotation](/java/annotation.md)
* [X] [String, StringBuilder, StringBuffer](/java/string_buffer_builder.md)
* [X] [동기화와 비동기화의 차이(Syncronous vs Asyncronous)](/java/syncronous_asyncronous.md)
* [X] [java에서 '=='와 'equals()'의 차이](/java/==_equals.md)
* [X] [java의 리플렉션(Reflection) 이란](/java/reflection.md)
* [X] [Stream이란?](/java/stream.md)
* [X] [Lambda란?](/java/lambda.md)
* [X] [Java 버전](/java/java_version.md)

## 8. JavaScript
:arrow_forward: [답변 내용](/contents/javascript.md)
* JavaScript Event Loop
* 함수 선언식과 함수 표현식
* 화살표 함수(Arrow Function)
* 향상된 객체 리터럴(Enhanced Object Literals)
* Modules
* 디스트럭처링(Destructuring)
* 전개 연산자(Spread Operator)
* 호이스팅(Hoisting)
* Closure
* this
* Promise
* Async/Await

## 9. Spring
:arrow_forward: [답변 내용](/contents/spring.md)
* [X] [스프링 프레임워크란](/spring/spring_framework.md)
* [X] [Spring, Spring MVC, Spring Boot의 차이](/spring/spring_spring_mvc_spring_boot.md)
* [X] [Bean이란](/spring/spring_bean.md)
* [X] [Container란](/spring/container.md)
* [X] [IOC(Inversion of Control, 제어의 역전)란](/spring/ioc.md)
* [X] [MVC 패턴이란](/spring/mvc_pattern.md)
* [X] [DI(Dependency Injection, 의존성 주입)란](/spring/spring_di.md)
* [X] [AOP(Aspect Oriented Programming)란](/spring/spring_aop.md)
* [X] [POJO](/spring/pojo.md)
* [X] [DAO와 DTO의 차이](/spring/spring_dto_dao.md)
* [X] [Spring JDBC를 이용한 데이터 접근](/spring/spring_jdbc.md)
* [X] [Filter와 Interceptor 차이](/spring/filter_intercepter.md)
* [X] [디스패처서블릿(DispatcherServlet) 이란?](/spring/Dispatcher-Servlet.md)

## 10. Security 
:arrow_forward: [답변 내용](/contents/security.md)
* [X] [대칭키와 비대칭키 차이](/security/key.md)
* [X] [패스워드 암호화 방법](/security/password.md)
* [X] [SQL Injection 공격](/security/sql_injection.md)
* [X] [CSRF 공격](/security/csrf.md)
* [X] [XSS 공격](security/xss_attack.md)
* [X] [OAuth](security/oauth.md)

## 11. ETC
:arrow_forward: [답변 내용](/contents/etc.md)
* [X] [TDD란](/etc/tdd.md)
* [X] [웹 브라우저에서 서버로 어떤 페이지를 요청하면 일어나는 일련의 과정을 설명](/etc/web_browser.md)
  * Ex. url에 'www.naver.com' 을 입력했다. 일어나는 현상에 대해 아는대로 설명하라.
* [X] [컴파일러와 인터프리터](/etc/compiler_interpreter.md)
* [X] [분산락](/etc/lock.md)
* [X] [프레임워크와 라이브러리의 차이](/etc/framework_library.md)
* [X] [64bit CPU와 32bit CPU 차이](/etc/64bit_32bit.md)
* [X] [CVS, SVN, Git](/etc/scm.md)
* [X] [Git Branch 종류(5가지)](/etc/git_branch.md)
* [X] [웹 서버(Web Server)와 웹 어플리케이션 서버(WAS)의 차이](/etc/webserver_was.md)
* [ ] 애자일 방법론이란
* [ ] Servlet과 JSP
* [ ] Redis와 Memcached의 차이
* [ ] Maven과 Gradle의 차이
* [ ] Blocking과 Non-Blocking
* [ ] 함수형 프로그래밍이란
* [ ] 이벤트 기반 프로그래밍이란
* [ ] Mock이란

---
