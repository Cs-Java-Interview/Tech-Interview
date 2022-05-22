# Statement & PreparedStatement

### Statement와 PreparedStatement

- SQL문을 실행할 수 있는 객체
- 가장 큰 차이점은 **캐시의 사용 여부**이다.

### SQL 실행 단계
1. DB Connection
2. 쿼리 문장 분석
3. 컴파일
4. 실행

### Statement

```java
String query = "select * from User where name = " + name;
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery(query);
```

- 쿼리문을 수행할 때마다 SQL 실행 단계 2~4단계를 거친다.
- SQL문을 수행하는 과정에서 매번 **컴파일**을 하기 때문에 성능상의 이슈가 발생한다.
- 실행되는 SQL문을 확인 가능하다.

### PreparedStatement

```java
String query = "select * from User where name = ?";
PreparedStatement pstmt = conn.preparedStatement();
pstmt.setString(1, name);
ResultSet rs = pstmt.executeQuery(query);
```

- **컴파일**이 미리 되어있기 떄문에 Statement에 비해서 좋은 성능을 가진다.
- 특수문자를 자동으로 파싱해주기 때문에 `SQL Injection` 공격을 예방할 수 있다.
- 특수문자를 자동으로 파싱해주기 때문에 파싱으로 인한 오류를 막을 수 있다.
- 쿼리를 반복수행한다면 `Statement` 보다는 `PreparedStatement` 객체를 사용하는 것이 성능상 좋다.

### Statement와 PreparedStatement의 아주 큰 차이는 캐시 사용여부

`Statement`를 사용하면 매번 쿼리를 수행할 때마다 계속적으로 단계를 거치면서 수행하지만, `PreparedStatement`를 사용하면 처음 한번만 2~4단계를 거치면 **캐시**에 담아 재사용을 한다. 이에따라 만약 동일한 쿼리를 반복적으로 수행한다면 `PreparedStatement`가 DB에 훨씬 적은 부하를 주며, 성능도 좋다.