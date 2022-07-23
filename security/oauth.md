# OAuth?

<img width="849" alt="image" src="https://user-images.githubusercontent.com/36829127/180604002-84a29ab3-2374-47a0-a581-a0f86acbed9f.png">


OpenID Authentication의 줄임말로, 말 그대로 인터넷 사용자들이 비밀번호를 제공하지 않고 다른 웹사이트 상의 자신들의 정보로 웹 사이트나 애플리케이션의 접근 권한을 부여할 수 있는 공통적인 수단으로 사용되는, 접근 위임을 위한 개방형 표준입니다.ㅈ

주로 여러 기업들에 의해 사용되는데, 아마존, 구글, 페이스북, 네이버, 카카오 등이 있습니다. 현재 OAuth1.0에서 발견된 취약점으로 인해 OAuth2.0으로 대체되어 사용되고 있습니다.

## OAuth 1.0의 취약점

**A session fixation attack against the OAuth Request Token approval flow (OAuth Core 1.0 Section 6) has been discovered.**

공격은 공격자가 (정직한) 소비자 사이트를 방문하여 선택적으로 해당 사이트에서 소유 한 계정에 로그인하는 것으로 시작됩니다. 공격자는 OAuth 인증 프로세스를 시작하지만 인증을 얻기 위해 소비자의 리디렉션을 따르지 않고 대신 인증 요청 URI (요청 토큰 포함) 를 저장합니다 . 나중에 공격자는 피해자가 (정직한) 소비자에 대한 피해자의 보호 된 리소스에 대한 액세스를 승인하기 위해 권한 부여 요청 URI 로 구성된 링크를 클릭 하도록 유도합니다.

링크를 클릭함으로써 피해자는 (정직한) 소비자가 공격자에게 발행 한 요청 토큰을 포함하여 공격자가 시작한 요청을 계속합니다. 피해자는 서비스 공급자의 합법적 인 승인 페이지로 리디렉션되고 서비스 공급자는 (정직한) 소비자를 승인하라는 메시지를 표시합니다. 피해자가 지속적인 공격이 있음을 감지하는 것은 불가능합니다.

피해자가 승인을받은 후 공격자는 저장된 요청 토큰을 사용하여 권한 부여 흐름을 완료하고 서비스의 일부로 (정직한) 소비자 사이트에 의해 노출되는 보호 된 리소스에 액세스 할 수 있습니다. 공격자가 (정직한) 소비자 사이트에 계정이있는 경우 액세스는 향후 방문에서 지속될 수 있습니다.

## OAuth1.0 vs OAuth2.0 비교

![image](https://user-images.githubusercontent.com/36829127/180603817-0e968e3e-64eb-407b-b05b-23dd8a797d05.png)

### OAuth 2.0은 1.0의 알려진 보안 문제 등을 개선한 버전으로 1.0을 대체(하위 호환성 미지원)

* 기존 서비스 제공자(Service Provider)를 자원 및 권한 서버로 분리하여 다수의 서비스 제공자(서버)로 구성 웹 서비스에서 발생 가능한 권한 동기화 문제 개선
* 오픈 API 요청 시 클라이언트 인증 방법으로 서명 대신, HTTPS를 의무화하여 서버 및 클라이언트 개발 편의성 개선
* 다양한 유형의 클라이언트와 이를 고려한 권한 승인 방법을 정의하여 유형별 클라이언트들에 대한 일관된 구현 가능
* 접근 토큰 재발급을 위한 재발급 토큰(Refresh Token)을 도입함으로써 접근 토큰의 유효 기간 단축 및 보안성 개선
  * 접근 토큰의 유효 기간이 과도하게 긴 경우, 접근 토큰이 유출된 경우 공격자에 의해 장시간 악용 가능
* 기타 다양한 확장성 지원

## OAuth 2.0 종류
OAuth 2.0 방식은 4가지가 있습니다.

1. Authorization Code Grant│ 권한 부여 승인 코드 방식
2. Implicit Grant │ 암묵적 승인 방식
3. Resource Owner Password Credentials Grant │ 자원 소유자 자격증명 승인 방식
4. Client Credentials Grant │클라이언트 자격증명 승인 방식

참고 : https://blog.naver.com/mds_datasecurity/222182943542


## OAuth 2.0 동작 방식(권한 부여 승인 코드 방식)
![image](https://user-images.githubusercontent.com/36829127/180604410-6b1e27c3-0291-421d-8ae9-1509f61279aa.png)

> **Client** : User가 이용하는 웹 또는 앱 어플리케이션
> 
> **Resource Owner** : Client에게 본인의 자원 접근에 대한 권한을 승인하는 User
> 
> **Resource Server** : 사용자의 보호된 자원을 호스팅하는 서버입니다.
>
> **Authorization Server** : 권한 서버. 인증/인가를 수행하는 서버로 클라이언트의 접근 자격을 확인하고 Access Token을 발급하여 권한을 부여하는 역할을 수행합니다.

시퀀스 다이어그램에는 refresh token이 없지만 보통 Authorization Server로 부터 access token(비교적 짧은 만료기간을 가짐) 과 refresh token(비교적 긴 만료기간을 가짐)을 함께 부여 받습니다.

access token은 보안상 만료기간이 짧기 때문에 얼마 지나지 않아 만료되면 사용자는 로그인을 다시 시도해야한다. 그러나 refresh token이 있다면 access token이 만료될 때 refresh token을 통해 access token을 재발급 받아 재 로그인 할 필요없게끔 해줍니다.


### 참고. OAuth 2.0와 JWT의 차이

OAuth 2.0는 하나의 플랫폼의 권한(아무 의미없는 무작위 문자열 토큰)으로 다양한 플랫폼에서 권한을 행사할 수 있게 해줌으로써 리소스 접근이 가능하게 하는데 목적을 둡니다.

JWT는 Cookie, Session을 대신하여 의미있는 문자열 토큰으로써 권한을 행사할 수 있는 토큰의 한 형식. (로그인 세션이나 주고받는 값이 유효한지 검증할 때 주로 쓰입니다..)

참고 : https://hwannny.tistory.com/92

https://velog.io/@devsh/OAuth-2.0-%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC

https://blog.naver.com/mds_datasecurity/222182943542



