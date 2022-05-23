## 데이터베이스 접근 과정

1. 클라이언트가 서버측에 DB 접근이 필요한 요청을 했다면, DB로 쿼리문을 날린다.
2. **웹서버가 DB서버에 접근하기 위해서는 DB Connection 객체를 생성하여 접근한다**.

이때, **웹 서버에서 DB서버에 최초로 연결되어 Connection 객체를 생성할 때가 가장 오래 걸린다.**

DB Connection 객체를 생성하는 비용과 시간을 줄이고, 네트워크 연결에 대한 부담을 줄이고자 데이터베이스 풀을 사용한다. 데이터베이스 풀 기법은 사전에 Connection 객체들을 생성하여 데이터베이스 풀에 보관하고 필요 시에 사용한다.

![https://github.com/WeareSoft/tech-interview/raw/master/contents/images/db-img/db-connection-02.png](https://github.com/WeareSoft/tech-interview/raw/master/contents/images/db-img/db-connection-02.png)

### 데이터베이스 풀이란

- 클라이언트의 요청에 따라 (스레드에서) 데이터베이스에 접근하기 위한 Connection들을 모아두는 공간 혹은 이러한 기법을 뜻함
- **Connection Pool 동작 과정**
    1. WAS가 실행되면서 미리 일정량의 DB Connection 객체를 생성하고 `Pool` 이라는 공간에 저장해 둔다.
    2. HTTP 요청에 따라 필요할 때 (DB 접근 필요시) Pool에서 Connection 객체를 가져다 쓰고 반환한다.
    3. 이와 같은 방식으로 HTTP 요청 마다 DB Driver를 로드하고 물리적인 연결에 의한 Connection 객체를 생성하는 비용이 줄어들게 된다.
- 장점
    - 매 연결마다 Connection 객체를 생성하고 소멸시키는 **비용을 줄일 수 있다**.
    - 미리 생성된 Connection 객체를 사용하기 때문에, **DB 접근 시간이 단축된다**.
    - DB에 접근하는 Connection의 수를 제한하여, **메모리와 DB에 걸리는 부하를 조정할 수 있다**.

### 스레드 풀이란

- **Thread Pool**
    - 매 요청마다 요청을 처리할 Thread를 만드는것이 아닌, 미리 생성한 pool 내의 Thread를 소멸시키지 않고 재사용하여 효율적으로 자원을 활용하는 기법.
- **Thread Pool과 Connection pool**
    - Thread와 Connection Pool의 개수는 성능에 직접적인 영향을 주기에 적절한 수로 관리해야한다.
    - 많이 사용하면 할 수록 메모리를 많이 점유하게 된다
    - 메모리를 위해 적게 지정한다면, 서버에서는 많은 요청을 처리하지 못하고 대기 할 수 밖에 없다.
    - ‘Thread의 개수 > DB의 Connection Pool의 갯수’이어야 효율적이다.
    - 왜냐하면, 애플리케이션에 대한 모든 요청이 DB에 접근하는 것은 아니기 때문이다.

---
아래는 데이터베이스 풀을 사용하지 않은 데이터베이스 접근 방식이다. (WAS에서 DB 서버에 접근하고 데이터를 처리하기까지의 과정)

```java
String driverPath = "net.sourceforge.jtds.jdbc.Driver"; // 1
String address = "jdbc:jtds:sqlserver://IP/DB";
String userName = "user";
String password = "password";
String query = "SELECT ... where id = ?";
try {
 Class.forName(driverPath);
 Connection connection = DriverManager.getConnection(address, userName, password); // 2.
 PreparedStatement ps = con.prepareStatement(query); // 3.
 ps.setString(1, id);
 ResultSet rs = get.executeQuery(); // 4.
 // ....
} catch (Exception e) { }
} finally {
 rs.close(); // 6.
 ps.close(); 
}
```

1. DB 서버 접속을 위해 JDBC 드라이버를 로드
2. DB Connection 객체를 생성 **(매 연결 시 Connection 객체를 생성해야함)**
3. 쿼리를 수행하기 위한 PreparedStatement 객체를 생성
4. executeQuery를 수행
5. 그 결과로 ResultSet 객체를 받아서 데이터를 처리한다.
6. 처리에 사용된 리소스들(**Result , Statement, Connection**)을 close하여 반환
