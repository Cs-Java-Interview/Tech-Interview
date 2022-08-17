# Web Server와 WAS의 차이

<img width="798" alt="image" src="https://user-images.githubusercontent.com/36829127/185095539-5fa09848-b17c-4fc6-a117-eb808b742e1b.png">

먼저 **정적 페이지**와 **동적 페이지**를 알아보자

### 정적 페이지 (Static Page)

**바뀌지 않는 페이지**

* 웹 서버는 파일 경로 이름을 받고, 경로와 일치하는 file contents를 반환함, 항상 동일한 페이지를 반환함
* image, html, css, javascript 파일과 같이 컴퓨터에 저장된 파일들



### 동적 페이지 (Dynamic Page)

**인자에 따라 바뀌는 페이지**

* 인자의 내용에 맞게 동적인 contents를 반환한다.
* 웹 서버에 의해 실행되는 프로그램을 통해 만들어진 결과물임 (Servlet : was 위에서 돌아가는 자바 프로그램)
* 개발자는 Servlet에 doGet() 메소드를 구현함


## Web Server와 WAS의 차이

<img width="771" alt="image" src="https://user-images.githubusercontent.com/36829127/185096425-8162341e-0465-4efc-b44c-8a4660a67840.png">

### Web Server

개념에 있어서 하드웨어와 소프트웨어로 구분된다.

* 하드웨어 : Web 서버가 설치되어 있는 컴퓨터
* **소프트웨어 : 웹 브라우저 클라이언트로부터 HTTP 요청을 받고, 정적인 컨텐츠(html, css 등)를 제공하는 컴퓨터 프로그램**


* HTTP 프로토콜을 기반으로 하여 클라이언트(웹 브라우저 또는 웹 크롤러)의 요청을 서비스 하는 기능을 담당한다.
* **기능1)**
  * **정적인 컨텐츠 제공**, WAS를 거치지 않고 바로 자원을 제공한다

* **기능2)**
  * 동적인 컨텐츠 제공을 위한 요청 전달, 클라이언트의 요청(Request)을 WAS에 보내고, WAS가 처리한 결과를 클라이언트에게 전달(응답, Response)한다.
    * 클라이언트는 일반적으로 웹 브라우저를 의미한다.

* 종류 : Apache, Nginx, ISS 등


### WAS(Web Application Server)

DB 조회나 다양한 로직 처리를 요구하는 동적인 컨텐츠를 제공하기 위해 만들어진 Application Server

* HTTP를 통해 컴퓨터나 장치에 애플리케이션을 수행해주는 미들웨어(소프트웨어 엔진)이다.
* “웹 컨테이너(Web Container)” 혹은 “서블릿 컨테이너(Servlet Container)”라고도 불린다.
  * Container란 JSP, Servlet을 실행시킬 수 있는 소프트웨어를 말한다.
  * 즉, WAS는 JSP, Servlet 구동 환경을 제공한다.
* **프로그램 실행 환경과 데이터베이스 접속 기능을 제공한다.**
* **비즈니스 로직 수행이 가능하다.**

* 종류 : Tomcat, JEUS 등

## 동작 방식
<img width="761" alt="image" src="https://user-images.githubusercontent.com/36829127/185098199-d8b73c86-080a-4565-bb3e-387b586f4fb1.png">


## 웹 서버와 WAS를 따로 사용하는 이유

* 서로의 기능을 분리하여 **서버 부하를 방지**할 수 있다.
  * WAS는 DB 조회 등 페이지를 만들기 위한 다양한 로직을 처리하는데, 단순한 정적 컨텐츠까지도 WAS에서 제공하면 다른 작업에 사용하는 리소스로 인해 지연이 생겨날 수 있다.
  * 다만, 톰캣 5.5 이상부터는 성능이 크게 떨어지지 않는다고 한다.

* 물리적으로 분리하여 **보안을 강화**할 수 있다.
  * SSL에 대한 암복호화 처리에 웹 서버를 사용한다.
  * 웹 서버를 앞단에 두어, 공격이 있을 때 중요한 정보가 담긴 DB나 로직까지(WAS까지) 전파되지 못하게 한다.

<img width="761" alt="image" src="https://user-images.githubusercontent.com/36829127/185098767-b1c945fe-64bf-4d30-9528-eec12a43642d.png">

* 여러 대의 WAS를 연결 가능할 수 있다.
  * Load Balancing
    * fail over(작동 중지된 WAS를 대신해 다른 WAS를 사용하여 장애를 극복함)
    * fail back(작동 중지된 WAS를 재동작시킴)

* 대용량 웹 애플리케이션의 경우(여러 개의 서버 사용) Web Server와 WAS를 분리하여 무중단 운영을 위한 장애 극복에 쉽게 대응할 수 있다.
* 다른 종류의 WAS로 서비스 가능
  * 하나의 서버에서 PHP Application과 Java Application을 함께 사용할 수 있다.


<img width="756" alt="image" src="https://user-images.githubusercontent.com/36829127/185099013-aa817390-ded5-4740-bdd1-6c54e2d7becc.png">



---

참고 : 

https://velog.io/@bky373/Web-웹-서버와-WAS

https://gyoogle.dev/blog/web-knowledge/Web%20Server와%20WAS의%20차이.html

https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html


