## **JDBC란**

- 자바에서 데이터베이스에 접속할 수 있도록 하는 자바 표준 인터페이스**(Java DataBase Connectivity)**
- **자바**는 DBMS(Oracle, MySQL 등)의 종류에 상관없이 **하나의 JDBC API를 이용**해서 데이터베이스 **작업을 처리**
    
    ![image](https://user-images.githubusercontent.com/77563814/170852731-e788378c-c7ae-49c6-a8d3-25e9d09d0b5f.png)
    

- **JDBC가 생긴 이유**
    - 이전에는 데이터베이스의 종류 마다(ms-sql, my-sql, oracle 등) 각각의 SQL문을 사용함
    - DB의 종류에 따라 SQL문의 작성 방법이 달라서 구현이 불편했음
    - 그래서 어떤 DBMS든지 동일하게 데이터베이스의 CRUD를 구현할 수 있도록 만든 것이 JDBC이다.
    
    ![image](https://user-images.githubusercontent.com/77563814/170852891-dc27f1f4-e1e9-4475-9903-674a6c11fdfd.png)
    

- **역할**
    1. JDBC 드라이버 로드
        - `Class.forName("oracle.jdbc.driver.OracleDriver");`
    2. DB 연결
        - `Connection con = DriverManager.getConnection(URL, user, password);`
    3. Statement 생성
        - `PreparedStatement stmt = conn.prepareStatement(sql);`
    4. SQL 문 전송 (데이터의 입력/수정/삭제)
        - `stmt.executeQuery();`
    5. 결과 받기 (입력/수정/삭제가 아닌 조회의 경우)
        - `ResultSet rs = stmt.executeQuery();`
    6. 연결 종료
        - `rs.close(); // ResultSet rs
        stmt.close(); // PreparedStatement stmt
        conn.close(); // Connection conn`
        
        
### Reference

김영한님의 JPA 강의 [https://www.inflearn.com/course/ORM-JPA-Basic](https://www.inflearn.com/course/ORM-JPA-Basic)
