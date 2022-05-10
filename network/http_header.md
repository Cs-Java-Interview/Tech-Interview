## HTTP 헤더

![image](https://user-images.githubusercontent.com/77563814/167558667-23da2b3a-6f18-44f0-b6aa-e6d082aa2895.png)


- HTTP 헤더란
    - 클라이언트와 서버가 요청 또는 응답으로 부가적인 정보를 전송할 수 있도록 해준다.
- HTTP 헤더내에는 4종류의 헤더가 있다.
    1. **General header** :  기본적인 정보가 담긴 헤더(바디에서 전송되는 데이터와는 관련이 없음)
    2. **Entity header**: 엔티티 바디에 대한 정보를 담는 헤더 (컨텐츠 길이 등)
    3. **Request header** : 클라이언트가 보내는 메세지의 헤더 (보내는 리소스나 클라이언트에 대한 정보로 요청 메세지에서만 사용함)
    4. **Response header** : 서버로부터 받은 메세지의 헤더 (서버나 응답 내용에 대한 정보로 응답 메세지에서만 사용함)

## 일반 헤더(General Header)

**: 바디에서 전송되는 데이터와는 관련이 없는, 기본적인 정보를 담은 헤더**

- **Date** : HTTP 메시지를 생성한 일시
- **Connection** : 클라이언트와 서버 간 연결에 대한 옵션
    - close : 현재 HTTP 메시지 직후에 TCP 접속을 끊는다는 것을 알림
    - Keep-Alive : 현재 TCP 커넥션을 유지
- **Cache-Control** : 캐시 관련

## 엔터티/개체 헤더 (Entity Header)

: **Entity(콘텐츠, 본문, 리소스 등)에 대한 정보가 담긴 헤더**

- **Content-Type**: 컨텐츠의 타입 및 문자 인코딩 방식(EUC-KR,UTF-8 등)을 지정
- **Content-Language**: 해당 개체와 가장 잘 어울리는 사용자 언어
- **Content-Encoding**: 해당 개체 데이터의 압축 방식
- **Content-Length**: 전달되는 해당 개체의 바이트 길이 또는 크기
- **Content-Location**: 해당 개체가 실제 어디에 위치하는가를 알려줌
- **Content-Disposition**: 응답 Body를 브라우저가 어떻게 표시해야 할지 알려주는 헤더
    - inline인 경우 웹페이지 화면에 표시되고, attachment인 경우 다운로드
- **Content-Security-Policy**: 다른 외부 파일들을 불러오는 경우, 차단할 소스와 불러올 소스를 명시
    - `Content-Security-Policy: default-src https:` => https를 통해서만 파일을 가져옴
    - `Content-Security-Policy: default-src 'self'` => 자신의 도메인의 파일들만 가져옴
    - `Content-Security-Policy: default-src 'none'` => 파일을 가져올 수 없음
- **Location**: 리소스가 리다이렉트(redirect)된 때에 이동된 주소, 또는 새로 생성된 리소스 주소
    - 300번대 응답이나 201 Created 응답일 때 어느 페이지로 이동할지를 알려주는 헤더
    - 새로 생성된 경우에 HTTP 상태 코드 `201 Created`가 반환됨
    - `HTTP/1.1 302 Found Location: /`
        - 이런 응답이 왔다면 브라우저는 / 주소로 redirect한다.
- **Last-Modified**: 리소스를 마지막으로 갱신한 일시

## 요청 헤더 (Request Header)

**: HTTP 요청 메시지 내에서만 사용하는, 클라이언트가 보내는 메세지의 헤더**

- **Host** : 요청하는 호스트에 대한 호스트명 및 포트번호 (***필수***)
- **User-Agent** : 클라이언트 소프트웨어(브라우저, OS) 명칭 및 버전 정보
- **From**: 클라이언트 사용자 메일 주소
- **Cookie** : 서버에 의해 Set-Cookie로 클라이언트에게 설정된 쿠키 정보
- **Referer** : 바로 직전에 머물었던 웹 링크 주소
- **If-Modified-Since** : 제시한 일시 이후로만 변경된 리소스를 취득 요청
- **Authorization** : 인증 토큰(JWT/Bearer 토큰)을 서버로 보낼 때 사용하는 헤더
    - 토큰의 종류(Basic, Bearer 등) + 실제 토큰 문자를 전송
- **Origin**
    - 서버로 POST 요청을 보낼 때, 요청이 어느 주소에서 시작되었는지 나타냄
    - 여기서 요청을 보낸 주소와 받는 주소가 다르면 *CORS 에러*가 발생
    - 응답 헤더의 **Access-Control-Allow-Origin**와 관련

## 응답 헤더 (Response Header)

**: HTTP 요청 메시지 내에서만 사용하는, 서버로부터 받은 메세지의 헤더**

- **Server**: 서버 소프트웨어 정보
- **Accept-Range**
- **Set-Cookie**: 서버측에서 클라이언트에게 세션 쿠키 정보를 설정 (RFC 2965에서 규정)
- **Expires**: 리소스가 지정된 일시까지 캐시로써 유효함
- **Age**: 캐시 응답. max-age 시간 내에서 얼마나 흘렀는지 알려줌(초 단위)
- **ETag**: HTTP 컨텐츠가 바뀌었는지를 검사할 수 있는 태그
- **Proxy-authenticate**
- **Allow**: 해당 엔터티에 대해 서버 측에서 지원 가능한 HTTP 메소드의 리스트를 나타냄
    - 때론, HTTP 요청 메세지의 HTTP 메소드 OPTIONS에 대한 응답용 항목
        - OPTIONS: 웹서버측 제공 HTTP 메소드에 대한 질의
    - `Allow: GET,HEAD` => 웹 서버측이 제공 가능한 HTTP 메서드는 GET,HEAD 뿐임을 알림 (405 Method Not Allowed 에러와 함께)
- **Access-Control-Allow-Origin**: 요청을 보내는 프론트 주소와 받는 백엔드 주소가 다르면 *CORS 에러*가 발생
    - 서버에서 이 헤더에 프론트 주소를 적어주어야 에러가 나지 않는다.
    - `Access-Control-Allow-Origin: www.zerocho.com`
        - 프로토콜, 서브도메인, 도메인, 포트 중 하나만 달라도 CORS 에러가 난다.
    - `Access-Control-Allow-Origin: *`
        - 만약 주소를 일일이 지정하기 싫다면 *으로 모든 주소에 CORS 요청을 허용되지만 그만큼 보안이 취약해진다.
    - 유사한 헤더로 `Access-Control-Request-Method, Access-Control-Request-Headers, Access-Control-Allow-Methods, Access-Control-Allow-Headers` 등이 있다.
