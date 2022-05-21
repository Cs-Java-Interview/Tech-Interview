# SQL Injection(SQL 삽입 공격)

## SQL Injection이란?
* SQL Injection이란 악의적인 사용자가 보안상의 취약점을 이용하여, 임의의 SQL문을 `주입`해서 데이터베이스가 비정상적인 동작을 하도록 조작하는 행위
* OWASP Top 10에 항상 상위권으로 등재된 공격이며, 공격이 쉬운 반면 성공할 경우 큰 피해를 입힐 수 있는 공격
  * OWASP Top 10 : Open Web Application Security Project에서 3~4년 주기로 발표하는 웹 취약점 Top 10

## SQL Injection 공격 종류 및 방법
1. [Error based SQL Injection](#1.Error-based-SQL-Injection)
---
### 1.Error based SQL Injection
**논리적 에러 기반 SQL Injection**
![image](https://user-images.githubusercontent.com/36829127/169639099-f784aba8-4e70-4dcc-8f5c-67ed9cbbcf59.png)

* 악의적인 사용자가 임의의 SQL 구문을 주입하여 공격하는 논리적 에러 기반 공격 방법
* `'OR 1=1 --`라는 구분을 id에 입력하여 공격한다.
  * 싱글쿼터 `'` 를 이용하여 id 입력을 닫는다
  * `OR 1=1`를 이용하여 모든 WHERE절을 참으로 만든다.
  * `--`를 이용하여 뒤의 구문을 모두 주석처리 한다.

* 결론적으로 Users 테이블에 있는 모든 정보를 조회하며 가장 먼저 만들어진 계정으로 로그인에 성공.
* 보통 관리자 계정을 맨 처음 만들기 때문에 관리자 계정 접근에 성공하게 된다.




### 2.Union based SQL Injection
**Union 명령어를 이용한 SQL Injection**
![image](https://user-images.githubusercontent.com/36829127/169639685-31905911-e1ab-43ba-a061-60479461b45a.png)

* SQL에서 `Union` 키워드는 두 개의 쿼리문에 대한 결과를 통합해서 하나의 테이블로 보여주는 키워드.
* 위 사진에서 보이는 쿼리는 Board라는 테이블에서 게시글을 검색하는 쿼리문이며, 여기에 입력값으로 Union키워드와 함께 컬럼수를 맞춰 SELECT 구문을 입력.
* 인젝션된 구문은 id와 password를 요청하는 쿼리문. 인젝션이 성공하게 되면 **사용자의 개인정보가 게시글과 함께 화면에 보여짐.**



### 3.Blind SQL Injection
**DB로부터 특정 값이나 정보를 전달받기 위함이 아닌, 단순히 참과 거짓의 정보만 알 수 있을 때 사용**

#### 3.1.Boolean based SQL
![image](https://user-images.githubusercontent.com/36829127/169639935-fd609c09-b174-42f7-b4aa-202bcb85a3d0.png)

* **위 그림은 Blind Injection을 이용하여 데이터베이스의 테이블명을 알아내는 방법**
* 악의적 사용자는 임의로 가입한 abc123이라는 아이디와 함께 `abc123’ and ASCII(SUBSTR(SELECT name From information_schema.tables WHERE table_type=’base table’ limit 0,1)1,1)) > 100 --`라는 구문을 주입.
  * `SUBSTR` 을 통해 첫 글자만 추출.
  * `limit` 키워드를 통해 하나의 키워드만 조회.
  * ASCII를 통해 테이블의 첫 글자를 ascii 값으로 변환.
  * 만약 조회되는 테이블 명이 `Users`라면 `U`자가 ascii 값으로 변환되고(117), 이 값을 Injection의 `100`과 비교.
  * 100이라는 숫자값을 바꿔가며 비교한다.
  * 공격자는 이 프로세스를 자동화 스크립트를 통해 테이블 명을 알아낸다.








### 3.Blind SQL Injection
