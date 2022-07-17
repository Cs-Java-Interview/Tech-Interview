# DAO와 DTO의 차이

## DAO(Data Access Object)

* DAO란 **DB의 data에 접근하기 위한 객체**이다. 일반적으로 DB에 접근하는 코드는 SQL을 사용하려면 Connection을 생성하고, 직접 쿼리문을 작성하여 Connection을 닫는 과정이 필요하다.

* 그러나 이는 번거롭고 코드의 가독성을 떨어트리기 때문에, 어플리케이션에서 사용할 DB로직을 객체 하나의 메서드로 구현하고, 이를 호출하여 만든 것을 DAO라고 함.

* 에 대한 접근을 DAO가 담당하도록 하여 데이터베이스 엑세스를 DAO에서만 하게 되면 다수의 원격호출을 통한 오버헤드를 VO나 DTO를 통해 줄일 수 있고 다수의 DB 호출문제를 해결할 수 있다.

* **Repository Package**를 DAO로 지칭하기도 한다.

```java

@Repository
@RequiredArgsConstructor
public class MemberRepository {

    private final EntityManager em;

    public void save(Member member) {
        em.persist(member);
    }

    public Member findOne(Long id) {
        return em.find(Member.class, id);
    }

    public List<Member> findAll() {
        return em.createQuery("select m from Member m", Member.class).getResultList();
    }

    public List<Member> findByName(String name) {
        return em.createQuery("select m from Member m where m.name =:name", Member.class)
                .setParameter("name", name)
                .getResultList();
    }
}

```


## DTO(Data Transfer Object)

* DTO란 계층 간 데이터 교환을 위한 객체이다. 즉, Controller와 Service 사이에서 데이터 교환을 위해 사용되는 객체이다.

* DTO는 로직을 가지지 않는 데이터 객체이고, getter/setter 메소드만 가진 클래스를 의미한다.

* 즉 엔티티를 DTO 형태로 변환한 후 사용한다.

### Entity
```java
@Entity @Getter @Setter
public class Member {

    @Id
    @GeneratedValue
    private Long id;

    private String name;
    private int age;

}
```

### DTO Logic
```java
    @Getter
    @Setter
    static class ResponseDto {
        private String name;
        private String result = "결과입니다.";

        public ResponseDto(Member member) {
            name = member.getName();
        }
    }
```

### Controller Logic
```java
    @PostMapping("api/useDTO")
    @ResponseBody
    public ResponseDto useDTO(@RequestParam String name, @RequestParam Long id) {
    	Member findMember = memberService.findOne(id);
        return new ResponseDto(findMember);
    }
    
    @PostMapping("api/notUseDTO")
    @ResponseBody
    public FindMember notUseDTO(@RequestParam String name, @RequestParam Long id) {
        Member findMember = memberService.findOne(id);
        return new FindMember(findMember);
    }
```

* useDTO 메서드는 Entitiy의 결과를 DTO로 변환하여 원하는 결과인 name과 result만 클라이언트에게 반환한다. 
하지만 notUseDTO는 Member엔티티의 모든 정보 즉, 엔티티 그 자체를 클라이언트에게 반환한다.

* 현재 예시는 DTO를 Response에만 이용했지만 Request역시 적용할 수 있다. 


## cf.VO(Value Object)

* VO란 DTO랑 유사한 개념이지만 Read-Only 속성을 가진다.

* DTO는 인스턴스 개념이지만, VO는 리터럴 개념이다. VO는 값들에 대해 Read-Only를 보장해줘야 존재의 신뢰성이 확보되지만 DTO의 경우에는 단지 데이터를 담는 그릇의 역할일 뿐 값은 그저 전달되어야 할 대상일 뿐이다.

* 따라서 VO의 핵심은 두 객체의 모든 필드 값들이 동일하면 두 객체는 같다이다. 
  * VO는 객체들의 주소가 달라도 값이 같으면 동일한 것으로 여긴다. 예를 들어, 고유번호가 서로 다른 만원 2장이 있다고 생각하자. 이 둘은 고유번호(주소)는 다르지만 10000원(값)은 동일하다.

* 따라서 VO는 getter 메소드와 함께 비즈니스 로직도 포함할 수 있다. 단, setter 메소드는 가지지 않는다. 또, 값 비교를 위해 equals()와 hashCode() 메소드를 오버라이딩 해줘야 한다.



