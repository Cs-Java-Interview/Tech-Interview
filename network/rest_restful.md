# REST

## REST란?

### REST의 정의

- `Representational State Transfer` 의 약자
-  자원을 이름(자원의 표현)으로 구분하여 해당 자원에 상태(정보)를 주고받는 것을 의미한다.
-  즉, 자원(resource)의 `표현(representation)`에 의한 `상태(정보)`를 전달하는 것이다.

##### 자원(resource)의 표현(representation)
> 자원: 해당 소프트웨어가 관리하는 모든 것이다 -> EX) 문서, 그림, 데이터, 해당 소프트웨어 자체 등 <br/>
> 자원의 표현: 그 자원을 표현하기 위한 이름 <br/>
> DB의 학생 정보가 자원일 때, `students`를 자원의 표현으로 정한다. 

##### 상태(정보) 전달
> 데이터가 요청되어지는 시점에서 자원의 상태(정보)를 전달한다.
> `JSON` 혹은 `XML`을 통해서 데이터를 주고받는 것이 일반적이다.

### REST의 구체적인 개념

- `HTTP URI`를 통해서 **자원(Resource)**을 명시하고, `HTTP Method(GET, POST, PUT, PATCH, DELETE)`를 통해서 해당 자원에 대한 **CRUD Operation**을 적용하는 것을 의미한다.
- 즉, REST는 `자원 기반의 구조(ROA, Resource Oriented Architecture)` 설계의 중심에 Resource가 있고, HTTP Method를 통해서 Resource를 처리하도록 설계된 아키텍처를 의미한다.
- 웹 사이트의 이미지, 텍스트, DB 내용 등의 모든 자원에 고유한 ID인 HTTP URI를 부여한다.

### REST가 필요한 이유

- 애플리케이션 **분리 및 통합**
- **다양한** 클라이언트의 등장
- 최근의 서버 프로그램은 다양한 브라우저와 안드로이드폰, 아이폰과 같은 모바일 디바이스에서도 통신을 할 수 있어야 한다.

### REST의 구성요소

1. 자원(Resource): URI
	- 모든 자원에 대한 고유한 **ID**
	- HTTP URI -> EX) /members?id=10
	- Client는 URI를 사용하여 자원을 지정하고 해당 자원의 상태(정보)에 대한 조작을 Server에 요청한다.
2. 행위(Verb): HTTP Method
	- HTTP 프로토콜의 Method를 사용한다.
	- GET, POST, PUT, PATCH, DELETE 
3. 표현(Representation of Resource)
	- Client가 자원의 상태(정보)에 대한 조작을 요청하면 Server는 이에 적절한 응답(Representation)을 보낸다.
	- 자원의 표현은 JSON, XML, TEXT, RSS 등 다양하다.
	- 일반적으로 `JSON`과 `XML`이 주로 사용된다.

### REST의 특징

1. Server-Client(서버-클라이언트 구조)
	- 자원이 있는쪽이 **Server**, 자원을 요청하는 쪽이 **Client**가 된다.
		- Server: API를 제공하고 비즈니스 로직 및 저장
		- Client: 사용자 인증이나 `context(세션, 로그인 정보)` 등을 직접 관리
	- 서로간의 의존도가 줄어듦
2. Stateless(무상태)
	- HTTP 프로토콜은 `Stateless Protocol`이므로 REST역시 무상태성을 가진다.
	- Client의 context를 Server에 저장하지 않는다.
		- 즉, 세션과 쿠키같은 context정보를 신경쓰지 않아도 되므로 구현이 단순해진다.
	- Server는 각각의 요청을 완전히 별개의 것으로 인식하고 처리한다.
		- 각 API 서버는 Client의 요청만을 단순 처리한다.
		- 즉, 이전 요청이 다음 요청의 처리에 연관되어서는 안된다.
		- Server의 처리 방식에 일관성을 부여하고 부담이 줄어들며, 서비스의 자유도가 높아진다.
3. Cacheable(캐시 처리가능)
	- 웹 표준 HTTP 프로토콜을 그대로 사용하므로 웹에서 사용하는 기존의 인프라를 그대로 활용할 수 있다.
		- 즉, HTTP가 가진 가장 강력한 특징 중 하나인 캐싱 기능을 적용할 수 있다.
		- HTTP 프로토콜 표준에서 사용하는 `Last-Modified` 태그나 `E-Tag`를 이용하면 캐싱 구현이 가능하다.
	- 대량의 요청을 효율적으로 처리하기 위해서 캐시가 요구됨
	- 캐시 사용을 통해서 응답시간이 빨라지고 REST Server 트랜잭션이 발생하지 않기 때문에 전체 응답시간, 성능, 서버의 자원 이용률을 향상시킬 수 있다.
4. Layered System(계층화)
	- Client는 REST API Server만 호출한다.
	- REST Server는 다중 계층으로 구성될 수 있다.
		- API Server는 순수 비즈니스 로직을 수행하고 그 앞단에 보안, 로드밸런싱, 암호화, 사용자 인증 등을 추가하여 구조상의 유연성을 줄 수 있다.
		- 또한 로드밸런싱, 공유 캐시등을 통해서 확장성과 보안성을 향상시킬 수 있다.
	- PROXY, 게이트웨이 같은 네트워크 기반의 중간 매체를 사용할 수 있다.
5. Code-On-Demand(optional)
	- Server로부터 스크립트를 받아서 Client에서 실행한다.
	- 반드시 충족할 필요는 없다.
6. Uniform Interface(인터페이스 일관성)
	- URI로 지정한 Resource에 대한 조작을 통일되고 한정적인 인터페이스로 수행한다.
	- HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.
		- 특정 언어나 기술에 종속되지 않는다.

# REST API

### REST API란?
- API(Application Programming Interface)
	- 데이터와 기능의 집합을 제공하여 컴퓨터 프로그램간 상호작용을 촉진하며, 서로 정보를 교환 가능하도록 하는 것
- REST API
	- REST를 기반으로 서비스 API를 구현한 것
	- 최근 `OpenAPI(누구나 사용할 수 있도록 공개된 API: 구글 맵, 공공 데이터 등)`, `마이크로 서비스(하나의 큰 애플리케이션을 여러 개의 작은 애플리케이션으로 쪼개어 변경과 조합이 가능하도록 만든 아키텍처)`등을 제공하는 업체 대부분은 REST API를 제공한다.

### REST API의 특징
- 사내 시스템들도 REST를 기반으로 시스템을 분산해서 확장성과 재사용성을 높여 유지보수 및 운용을 편리하게 할 수 있다.
- REST는 HTTP표준을 기반으로 구현하므로, HTTP를 지원하는 프로그램 언어로 클라이언트, 서버를 구현할 수 있다.
- 즉, REST API를 제작하면 델파이 클라이언트 뿐만아니라, 자바, C#, 웹 등을 이용해 클라이언트를 제작할 수 있다.

# RESTful

### RESTful이란?
- RESTful은 일반적으로 REST라는 아키텍처를 구현하는 웹 서비스를 나타내기 위해 사용되는 용어이다.
	- REST API를 제공하는 웹 서비스를 RESTful 하다고 할 수 있다.
- RESTful은 REST를 REST답게 쓰기위한 방법으로, 누군가가 공식적으로 발표한 것이 아니다.
	- 즉, REST 원리를 따르는 시스템은 RESTful이란 용어로 지칭된다.

### RESTful의 목적
- 이해하기 쉽고 사용하기 쉬운 REST API를 만드는 것
- RESTful한 API를 구현하는 근본적인 목적이 성능향상에 있는 것이 아니라 일관적인 컨벤션을 통한 API의 이해도 및 호환성을 높이는 것이 주 동기이니, 성능이 중요한 상황에서는 굳이 RESTful한 API를 구현할 필요는 없다.

### RESTful하지 못한 경우
- Ex1) CRUD기능을 모두 POST로만 처리하는 API
- Ex2) route에 resource, id외에 정보가 들어가는 경우(/students/updateName)