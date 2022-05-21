# SQL Injection(SQL 삽입 공격)

## SQL Injection이란?
* SQL Injection이란 악의적인 사용자가 보안상의 취약점을 이용하여, 임의의 SQL문을 `주입`해서 데이터베이스가 비정상적인 동작을 하도록 조작하는 행위
* OWASP Top 10에 항상 상위권으로 등재된 공격이며, 공격이 쉬운 반면 성공할 경우 큰 피해를 입힐 수 있는 공격
  * OWASP Top 10 : Open Web Application Security Project에서 3~4년 주기로 발표하는 웹 취약점 Top 10


---

## SQL Injection의 공격종류 및 

### 1. Error based SQL Injection
**논리적 에러 기반 SQL Injection**
![image](https://user-images.githubusercontent.com/36829127/169639099-f784aba8-4e70-4dcc-8f5c-67ed9cbbcf59.png)

* 악의적인 사용자가 임의의 SQL 구문을 주입하여 공격하는 논리적 에러 기반 공격 방법
* `'OR 1=1 --`라는 구분을 id에 입력하여 공격한다.
  * 싱글쿼터 `'` 를 이용하여 id 입력을 닫는다
  * `OR 1=1`를 이용하여 모든 WHERE절을 참으로 만든다.
  * `--`를 이용하여 뒤의 구문을 모두 주석처리 한다.

* 결론적으로 Users 테이블에 있는 모든 정보를 조회하며 가장 먼저 만들어진 계정으로 로그인에 성공.
* 보통 관리자 계정을 맨 처음 만들기 때문에 관리자 계정 접근에 성공하게 된다.




### 2. Union based SQL Injection
**Union 명령어를 이용한 SQL Injection**
![image](https://user-images.githubusercontent.com/36829127/169639685-31905911-e1ab-43ba-a061-60479461b45a.png)

* SQL에서 `Union` 키워드는 두 개의 쿼리문에 대한 결과를 통합해서 하나의 테이블로 보여주는 키워드.
* 위 사진에서 보이는 쿼리는 Board라는 테이블에서 게시글을 검색하는 쿼리문이며, 여기에 입력값으로 Union키워드와 함께 컬럼수를 맞춰 SELECT 구문을 입력.
* 인젝션된 구문은 id와 password를 요청하는 쿼리문. 인젝션이 성공하게 되면 **사용자의 개인정보가 게시글과 함께 화면에 보여짐.**



### 3. Blind SQL Injection
**DB로부터 특정 값이나 정보를 전달받기 위함이 아닌, 단순히 참과 거짓의 정보만 알 수 있을 때 사용**

#### 3.1. Boolean based SQL
![image](https://user-images.githubusercontent.com/36829127/169639935-fd609c09-b174-42f7-b4aa-202bcb85a3d0.png)

* **위 그림은 Blind Injection을 이용하여 데이터베이스의 테이블명을 알아내는 방법**
* 악의적 사용자는 임의로 가입한 abc123이라는 아이디와 함께 `abc123’ and ASCII(SUBSTR(SELECT name From information_schema.tables WHERE table_type=’base table’ limit 0,1)1,1)) > 100 --`라는 구문을 주입.
  * `SUBSTR` 을 통해 첫 글자만 추출.
  * `limit` 키워드를 통해 하나의 키워드만 조회.
  * ASCII를 통해 테이블의 첫 글자를 ascii 값으로 변환.
  * 만약 조회되는 테이블 명이 `Users`라면 `U`자가 ascii 값으로 변환되고(117), 이 값을 Injection의 `100`과 비교.
  * 100이라는 숫자값을 바꿔가며 비교한다.
  * 공격자는 이 프로세스를 자동화 스크립트를 통해 테이블 명을 알아낸다.


#### 3.2. Time based SQL

![image](https://user-images.githubusercontent.com/36829127/169640367-47295475-4b11-4637-ad6b-1e22643511a6.png)

* 마찬가지로 서버로부터 특정한 응답대신 boolean 응답을 통해 DB의 정보를 유추하는 기법
* 악의적인 사용자가 악의적인 사용자가 `abc123’ OR (LENGTH(DATABASE())=1 AND SLEEP(2)) –-` 이라는 구문을 주입.
 * `LENGTH` 함수는 문자열의 길이
 * `DATABASE()` 함수는 데이터베이스의 이름을 반환
 * 주입된 구문에서 `LENGTH(DATABASE())=1`가 참이면 SLEEP(2)가 작동하고, 거짓이면 작동하지 않음.
 * 그 외에도 BENCHMARK나 WAIT함수를 사용할 수 있음.


### 4. Stored Procedure SQL Injection
**저장된 프로시저에서의 SQL Injection**

* 저장 프로시저(Stored Procedure)는 일련의 쿼리를 모아 하나의 함수처럼 사용하기 위한 것
* 공격에 사용되는 대표적인 저장 프로시저는 MS-SQL에 있는 xp_cmdshell로 윈도우 명령어를 사용할 수 있게 됨


### 5. Mass SQL Injection
**다량의 SQL Injection 공격**

* 기존 SQL 인젝션과 달리 한번의 공격으로 다량의 데이터베이스가 조작되어 큰 피해를 입히는 것
* 보통 데이터베이스 값을 변조하여 DB에 악성스크립트를 삽입하고, 사용자들이 변조된 사이트 접속 시 좀비PC로 감염되게 한다. 이렇게 감염된 좀비들은 DDoS공격에 사용된다.

## SQL Injection 대응방안

### 1. 입력값에 대한 검증
* Injection에서 사용되는 기법과 키워드는 많기 때문에, 사용자의 입력 값에 대한 검증이 필요.
* 서버단에서 `화이트리스트` 기반으로 검증해야 한다. 블랙리스트 기반 검증을 하게 되면 수많은 차단리스트를 등록해야 하기 때문
 * 화이트리스트 검증 : 허용가능한 입력값에 대한 리스트 검증
 * 블랙리스트 검증 : 허용되지 않는 입력값에 대한 리스트 검증

### 2. Prepared Statement 구문사용
* Prepared Statement 구문을 사용하게 되면 사용자 입력 값이 DB의 Parameter로 들어가기 전에 DBMS가 미리 컴파일하여 실행하지 않고 대기.
* 그 후 사용자의 입력값을 인식하게 하여 공격쿼리가 들어간다고 하더라도, 사용자의 입력은 이미 의미 없는 단순 문자열이기 때문에 공격자의 의도대로 쿼리문이 작동하지 않음.

### 3. Error Msg 노출 금지
* 데이터베이스에서 오류 발생 시 에러가 발생한 쿼리문과 함께 테이블명 및 컬럼명이 노출될 수 있기 때문에 이에 관한 디테일한 노출을 금지한다.

### 4. 웹 방화벽 사용
* 웹 공격 방어에 특화되어 있는 방화벽을 사용한다.
* 웹 방화벽은 소프트웨어 형, 하드웨어 형, 프록시 형으로 나눌 수 있다.
