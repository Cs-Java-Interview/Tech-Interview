# Spring JDBC를 통한 DB접근

## 일반 JDBC를 통한 접근



### 0. DB dataSource 설정

application.properties 에 값을 설정해준다.
```properties
spring.datasource.url=jdbc:h2:tcp://localhost/~/test
spring.datasource.driver-class-name=org.h2.Driver
```


### 1. 순수 JDBC
```java

public class JdbcMemberRepository implements MemberRepository{
    /*
    * DB와 연동해서 이용하기위한 MemberRepository 인터페이스의 구현체 클래스
    */

    private final DataSource dataSource;

    //application.properties 에 datasource 세팅을 해 놓았기 때문에, 스프링 부트가 데이터소스를 만들어 놓고 주입해 준다.
    public JdbcMemberRepository(DataSource dataSource) {//throws SQLException {
        this.dataSource = dataSource;
//        dataSource.getConnection();//connection을 얻을 수 있다.
    }

    @Override
    public Member save(Member member) {
        //저장을 위한 쿼리
        String sql = "insert into member(name) values(?)"; // ?는 파라미터 바인딩

        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;

        try{
            conn = getConnection();
            pstmt = conn.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS);

            pstmt.setString(1, member.getName());

            pstmt.executeUpdate();
            rs=pstmt.getGeneratedKeys();

            if(rs.next()){
                member.setId(rs.getLong(1));
            }else{
                throw new SQLException("id 조회 실패");
            }
            return member;
        } catch (SQLException e) {
            throw new IllegalStateException(e);
        }finally {
            close(conn,pstmt,rs);
        }


    }

    @Override
    public Optional<Member> findById(Long id) {
        return Optional.empty();
    }

    @Override
    public Optional<Member> findByName(String name) {
        return Optional.empty();
    }

    @Override
    public List<Member> findAll() {
        return null;
    }

    private Connection getConnection(){
        return DataSourceUtils.getConnection(dataSource);
    }
    private void close(Connection conn,PreparedStatement pstmt, ResultSet rs){
        try{
            if(rs!=null){
                rs.close();
            }
        }catch (SQLException e){
            e.printStackTrace();
        }
        try{
            if(pstmt!=null){
                pstmt.close();
            }
        }catch (SQLException e){
            e.printStackTrace();
        }
        try{
            if(conn!=null){
                close(conn);
            }
        }catch (SQLException e){
            e.printStackTrace();
        }
    }
    private void close(Connection conn) throws SQLException {
        DataSourceUtils.releaseConnection(conn,dataSource);
    }
}
```

* 특징으로는 connection을 생성,열고 닫을수 있고, PreparedStatement를 이용해 쿼리를 날리고 받아오고, ResultSet을 통해 결과값을 담아서 확인할 수 있다.
* 현재로선 대부분 쓰이지 않는다.


### 2. Spring JDBCTemplate

스프링 JDBCTemplate과 Mabatis 같은 라이브러리는 JDBC API에서 본 반복코드를 대부분 제거해준다. 하지만 SQL은 직접 작성해야한다. 

```java
public class JdbcTemplateMemberRepository implements MemberRepository{
    @Override
    public Member save(Member member) {
        return null;
    }

    @Override
    public Optional<Member> findById(Long id) {
        return Optional.empty();
    }

    @Override
    public Optional<Member> findByName(String name) {
        return Optional.empty();
    }

    @Override
    public List<Member> findAll() {
        return null;
    }
}
```

---

* 생성자를 추가해준다.
```java
    private final JdbcTemplate jdbcTemplate;

    @Autowired
    public JdbcTemplateMemberRepository(DataSource dataSource) {
        jdbcTemplate = new JdbcTemplate(dataSource);
    }
```
---

* rowMapper 메서드를 생성해준다.
```java
private RowMapper<Member> memberRowMapper(){
    return (rs, rowNum) -> {
        Member member = new Member();
        member.setId(rs.getLong("id"));
        member.setName(rs.getString("name"));
        return member;
    };
}
```
---

* findById를 오버라이드 해준다.
```java
@Override
public Optional<Member> findById(Long id) {
    List<Member> result = jdbcTemplate.query("select * from member where id = ?",memberRowMapper(),id);
    return result.stream().findAny();
}
```
---

* save,findByName, findAll을 오버라이드 해준다.
```java
@Override
public Member save(Member member) {
    SimpleJdbcInsert jdbcInsert = new SimpleJdbcInsert(jdbcTemplate);
    jdbcInsert.withTableName("member").usingGeneratedKeyColumns("id");
    
    Map<String,Object> parameters = new HashMap<>();
    parameters.put("name",member.getName());
    
    Number key = jdbcInsert.executeAndReturnKey(new MapSqlParameterSource(parameters));
    member.setId(key.longValue());
    return member;
}

@Override
public Optional<Member> findByName(String name) {
    List<Member> result = jdbcTemplate.query("select * from member where name = ?",memberRowMapper(),name);
    return result.stream().findAny(); // result 를 stream 으로 바꿔서 findAny
}

@Override
public List<Member> findAll() {
    return jdbcTemplate.query("select * from member",memberRowMapper());
}
```
---
* config 설정에서 구현체를 바꾸어준다.
```java
   @Bean
    public MemberRepository memberRepository(){
//      return new JdbcMemberRepository(dataSource);
        return new JdbcTemplateMemberRepository(dataSource);
}
```


참고 : https://devdavelee.tistory.com/166
