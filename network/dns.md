# DNS

DNS는 Domain Name System의 약자이다. 그렇다면 DNS의 **Domain**에 대해 먼저 알아보자.

## Domain(도메인이란)이란?
* **도메인**이란, 웹 브라우저를 통해 특정 사이트에 진입할 때, IP 주소를 대신하여 사용하는 주소이다.
* **도메인**을 이용하여 한눈에 파악하기 힘든 IP주소를 보다 분명하게 나타낸다.
* 예를 들어, `http://111.111.22.33/`와 같이 IP 형식의 주소를 `www.cs_study.com`과 같은 형식으로 바꾸어주는 것.
* **DNS**라는 시스템을 이용해서 IP와 도메인을 매핑해준다.

## DNS(Domain Name System)란?
* DNS란 웹사이트의 IP주소와 도메인 주소를 이어주는 환경/시스템이다.
* DNS 안에서 IP와 도메인 매칭 역할을 하는것이 `DNS서버`다.

## DNS 구조
* 도메인은 계층 구조기 때문에 인터넷 주소 중 원하는 주소를 효율적으로 찾을 수 있다.
* 도메인은 **역트리 구조**로 최상위 `루트`부터 `Top-level 도메인`, `Second-level 도메인`.. 과 같이하위 레벨로 원하는 주소를 찾아간다.
* **third.second.top.**과 같은 형식이며 **naver.com**의 경우와 같이 맨 뒤의 루트(.)는 생략한다.
* 주소를 찾아가는 경로는 뒤에서부터 앞으로 진행된다.

[dns1](./images/dns1.png)

## DNS 동작과정

> naver.com을 주소창에 입력했을 DNS에서 일어나는 일

[dns2](./images/dns2.png)

1. **naver.com**가 주소창에 입력된다.
2. naver.com의 IP가 무엇인지에 대해 `Local DNS 서버`에 전달한다.
3. Local DNS에서 `Root DNS 서버`에게 도메인 주소에 대한 IP를 요청한다.
4. Root DNS 서버로부터 "com Domain"을 관리하는 `TLD(Top-Level-Domain)이름 서버` 정보를 전달 받음
5. TLD에 naver.com에 대해 질의한다.
6. TLD에서 `naver.com를 관리하는 DNS 서버`에 대한 정보를 전달한다.
7. 그 도메인을 관리하는 DNS 서버에 naver.com에 대한 IP주소 질의
8. 해당 DNS 서버에서 Local DNS 서버에게 naver.com에 대한 IP주소를 응답
9. Local DNS는 naver.com에 대한 IP를 캐싱하고 IP 주소 정보 전달




