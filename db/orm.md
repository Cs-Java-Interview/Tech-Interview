### 영속성이란

- 데이터를 생성한 프로그램이 종료되더라도 사리지지 않는 데이터의 특성을 말한다.
- 영속성을 갖지 않는 데이터는 단지 PC의 메모리에서만 존재하기 때문에 
프로그램이 종료되면 모두 잃어버리게 된다. 
때문에 파일 시스템, 데이터베이스 등을 활용하여 데이터를 영구적으로 저장하기 위한 영속성을 부여한다.
- Persistence Layer
    - 프로그램의 아키텍처에서 **데이터에 영속성을 부여해주는 계층**을 말한다.
    - JDBC를 이용하여 직접 구현할 수 있지만 Persistence Framework를 이용한 개발이 많다.
        ![image](https://user-images.githubusercontent.com/77563814/170852652-2ca86550-9a61-4f94-8ffc-ce7037f90a56.png)
        

**Persistence Framework**는 SQL Mapper 와 ORM으로 나뉜다.

- SQL Mapper
    - SQL 문장으로 직접 데이터베이스를 다룬다. 즉 SQL Mapper는 SQL을 명시해 줘야 한다.
    - e**x)** MyBatis
- ORM (Persistence API)
    - 객체와 관계형 데이터베이스의 데이터를 자동으로 매핑(연결)해주는 것을 말한다.
    - ex) JPA
- 차이점
    - ORM은 관계형 데이터베이스의 '관계'를 Object에 반영하는것이 목적이라면,
    - SQL Mapper는 단순히 필드를 매핑시키는 것이 목적이라는 점에서 지향점의 차이가 있다.

## ORM이란

: Object-Relational Mapping (Persistant API라고도 부른다.)

- 객체와 관계형 데이터베이스의 데이터를 자동으로 매핑해주는 것
    - 객체 지향 프로그래밍은 **클래스**를 사용하고, 관계형 데이터베이스는 **테이블**을 사용하기에
    - **객체 모델과 관계형 모델 간에 패러다임의 불일치가 존재**한다.
    - ORM을 통해 SQL을 자동으로 생성하여 불일치를 해결해준다.
    - 쉽게 말하면, 개발자가 객체는 객체대로, RDB는 RDB대로 설계 후 그 사이의 차이(패러다임의 차이)는 ORM이 자동 해결해준다.

```
    💡 이때, ‘객체와 관계형 DB’ 사이 패러다임의 불일치란?
    객체는 추상화, 상속, 다형성(클래스, 메소드)의 특징을 가지고 있는 반면, RDB는 데이터 중심으로 이뤄져있어
    RDB에 객체를 저장하는 데 불일치가 발생함
```


- **‘객체와 관계형 DB’ 사이 패러다임의 불일치**
    1. **상속**
        - 객체랑 다르게 테이블은 상속이라는 기능이 없음
    2. **연관관계**
        - 객체는 참조를 사용해서 연관된 객체를 조회하는 데,
        - 테이블은 외래키로 연관관계를 설정하고 조인으로 연관 테이블을 조회함
        ```
        // 원래의 객체 지향 방식
            class Member {
                String id; //MEMBER_ID 컬럼 사용
                Team team; //참조로 연관관계를 맺는다. => Long teamId; 테이블처럼 바꿀 경우
                
                Team getTeam() {
                return team;
                }
            }
            class Team {
                Long id; // TEAM_ID PK 사용
                String name; // NAME 컬럼 사용
            }
         ```
    3. **객체 그래프 탐색**
          - ![image](https://user-images.githubusercontent.com/77563814/170852558-ea3114e7-88c1-49bd-bbbe-5cd40beb9747.png)
          - 객체는 상속, 연관 관계라면 자유롭게 그래프를 탐색 가능하다.
          - 반면 DB에서 SQL을 직접 다루면, 미리 그래프의 범위를 정해주어야 하고, 정해진 그래프 범위로 넘어갈 수 없으며 데이터가 없으면 탐색할 수 없다.
        
    4. **값 비교**
        - DB는 PK(기본 키)로 각 로우를 구분하는 반면, 객체는 동일성/동등성 비교를 한다.
        
        ```
        String memberId = "100";
        Member member1 = memberDAO.getMember(memberId);
        Member member2 = memberDAO.getMember(memberId);
        
        member1 == member2; //다르다.
        // DB에서 같은 로우를 조회했지만,
        // 객체 입장에서는 new로 생성된 각각 다른 인스턴스이기때문에 다른값으로 나옴
        ```
        

**ORM의 장단점**

- 장점
    - 객체 지향적인 코드
        - SQL 쿼리 보다 더 직관적이고, 비즈니스 로직에 더 집중할 수 있게 도와줍니다.
    - 재사용성 및 유지보수의 편리성이 증가합니다.
        - ORM은 독립적으로 작성되어 있고, 해당 객체들을 재활용할 수 있다.
    - DBMS에 대한 종속적이지 않다.
- 단점
    - 완벽한 ORM으로만 서비스를 구현하기가 어렵습니다.
        - 사용하기는 편하지만 설계는 매우 신중하게 해야합니다.
        - 프로젝트의 복잡성이 커질 경우 난이도 또한 올라갈 수 있습니다.
        - 잘못 구현된 경우에 속도 저하 및 심각할 경우 일관성이 무너지는 문제점이 생길수 있습니다.

- JPA란
    - **자바의 ORM 기술 표준, Java Persistence API**
    - 직접적인 SQL 문을 사용하지 않고 자바 코드를 사용해서 DB에 접근, 조작할 수 있는 기술
    - **JPA 역시 내부적으로 JDBC를 사용한다.**
        
        ![image](https://user-images.githubusercontent.com/77563814/170852565-adb1cd93-05c0-40d8-bf3b-662df385af40.png)
        
    - DB마다 제공하는 SQL 문법이 다른데(테이터베이스 방언) JPA 사용하면 알아서 번역해줘서 JPA는 특정 DB에 종속되지 않고 사용 가능함.

- Hibernate란
    - **구현체의 한 종류로,** (*DataNucleus, EclipseLink등 다른 구현체도 존재)*
    - **JPA가 DB와 자바 객체를 매핑하기 위한 인터페이스이고 Hibernate는 이를 구현한 라이브러리이다** (마치 인터페이스-클래스의 관계)
    - JPA는 *이러이러한 기능이 존재해야 한다*라고 정의한 규약이고, 실제로 개발자가 사용하는 것은 그 구현체들(Hibernate 등)



### Reference

김영한님의 JPA 강의 [https://www.inflearn.com/course/ORM-JPA-Basic](https://www.inflearn.com/course/ORM-JPA-Basic)
