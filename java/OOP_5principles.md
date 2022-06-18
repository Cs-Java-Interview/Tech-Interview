# OOP 5가지 원칙 : SOLID

클린코드 저자 로버트 마틴이 좋은 객체 지향 설계의 5가지 원칙을 정리
- **SRP**: 단일 책임 원칙(single responsibility principle)
- **OCP**: 개방-폐쇄 원칙 (Open/closed principle)
- **LSP**: 리스코프 치환 원칙 (Liskov substitution principle)
- **ISP**: 인터페이스 분리 원칙 (Interface segregation principle)
- **DIP**: 의존관계 역전 원칙 (Dependency inversion principle)

## SRP : 단일 책임 원칙(single responsibility principle)

- 한 클래스는 하나의 **책임**만 가져야 한다.
- 변경이 있을 때 다른 코드의 파급 효과가 적으면 단일 책임 원칙을 잘 따른 것
- 객체가 담당하는 책임이 많아질 수록, 해당 객체의 변경에 따른 영향도의 범위가 커지며 이는 단일 책임 원칙을 위반한 것
- 예) UI 변경, 객체의 생성과 사용을 분리


휠의 구동 특성에 따라 전륜(FWD), 후륜(RWD), 사륜(AWD)로 구분되는 Car 클래스가 있다고 하자.
이를 객체로 구현한 경우는 다음과 같다.

```java
public class Car
{
    private final String WD;
    private final int[] WHEEL = { 0, 0, 0, 0 };
    
    public Car(String wd)
    {
        WD = wd;
    }
    

    public void run(int power)
    {
        switch (WD.toUpperCase())
        {
            case "FWD" -> {
                WHEEL[0] = power;
                WHEEL[1] = power;
            }
            
            case "RWD" -> {
                WHEEL[3] = power;
                WHEEL[4] = power;
            }
            
            case "AWD" -> {
                WHEEL[0] = power;
                WHEEL[1] = power;
                WHEEL[3] = power;
                WHEEL[4] = power;
            }
        }
        System.out.println("휠 동력 상태: " + WHEEL[0] + ", " + WHEEL[1] + ", " + WHEEL[2] + ", " + WHEEL[3]);
    }
}
```

* 잘 구현된 객체처럼 보이지만 이는 한 클래스에 **책임**을 세개나 부여했고 SRP원칙을 위반한 정책이다.
* 이는 간단한 구조이지만 만약 코드가 복잡해지고 규모가 커진다면 해당 코드를 수정할 때마다 다른 코드를 일괄 수정해야 할 수도 있다.
* 단일 책임 원칙을 지키기 위해선 `1객체 = 1책임`으로 객체를 간결하게 설계해야 한다.

```java

abstract public class Car
{
    protected final String WD;
    protected final int[] WHEEL = { 0, 0, 0, 0 };
    
    public Car(String wd)
    {
        WD = wd;
    }
    
    abstract public void run(int power);
}

class FrontWheelCar extends Car
{

    public FrontWheelCar(String wd)
    {
        super(wd);
    }
    
    @Override
    public void run(int power)
    {
        WHEEL[0] = power;
        WHEEL[1] = power;
        
        System.out.println("휠 동력 상태: " + WHEEL[0] + ", " + WHEEL[1] + ", " + WHEEL[2] + ", " + WHEEL[3]);
    }
}
```

* 일단 run()이라는 함수를 가진 Car Interface를 구현해준다.
* 이후 Car Interface를 상속받아 각각의 **휠 구동 방식**을 **하나**씩만 갖는 클래스를 구현한다면 객체는 단일 책임 원칙을 준수하게 된다.



## OCP: 개방-폐쇄 원칙 (Open/closed principle)
* 소프트웨어 요소는 **확장에는 열려**있으나 **변경에는 닫혀**있어야 한다.
* 다형성을 활용하여 적용할 수 있다.
* 인터페이스를 구현한 새로운 클래스를 생성하여 새로운 기능을 구현한다.

```java

// 기존 코드
@Service
public class MemberService {
    
    private MemberRepository memberRepository = new JdbcMemberRepository();
}

// 변경 코드
@Service
public class MemberService {
    
    // private MemberRepository memberRepository = new JdbcMemberRepository();
    private MemberRepository memberRepository = new MybatisMemberRepository();
}
```

* JdbcMemberRepository를 의존하는 MemberService가 새롭게 MybatisMemberRepository를 의존하려 할때, 클라이언트 부분인 MemberService 코드를 수정해야한다.
* 이는 OCP(개방-폐쇄 원칙)을 위반한 코드이다. IoC(제어의 역전), DI(의존성 주입)을 통해 이러한 문제를 해결할 수 있다.


```java
@Service
public class MemberService {
    
    private MemberRepository memberRepository;

    @Autowired
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }       
}
```

* 객체를 생성하고, 연관관계를 맺어주는 별도의 조립, 설정자(IoC, DI)를 통해 MemberService 코드를 수정하지 않고 Repository를 확장할 수 있다.


## LSP: 리스코프 치환 원칙 (Liskov substitution principle)

* 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다.
* 다형성에서 하위 클래스는 인터페이스 규약을 다 지켜야 한다는 것, 다형성을 지원하기 위한 원칙, 인터페이스를 구현한 구현체는 믿고 사용하려면, 이 원칙이 필요하다.
* 단순히 컴파일이 성공하는 것과는 다른 이야기이다. 프로그램이 수행하는 업무가 목표하는 바와 일치하는지에 대한 문제다.
* 예) 자동차 인터페이스의 엑셀은 앞으로 가라는 기능, 뒤로 가게 구현하면 LSP 위반, 느리게 가더라도 앞으로 가야함.


## ISP: 인터페이스 분리 원칙 (Interface segregation principle)
* 특정 클라이언트를 위한 인터페이스는 여러 개가 범용 인터페이스 하나보다 낫다.
* 예) 자동차 인터페이스 -> 운전 인터페이스, 정비 인터페이스로 분리
* 예) 사용자 클라이언트 -> 운전자 클라이언트, 정비사 클라이언트로 분리
* 분리 한다면 하나의 인터페이스가 변경되어도 하나의 클라이언트에게 영향을 주지 않는다(정비 인터페이스가 변경되어도 운전자 클라이언트에 영향 없음)
* 인터페이스가 명확해지고, 대체 가능성을 높일 수 있다.


## DIP: 의존관계 역전 원칙 (Dependency inversion principle)
* 개발자는 **추상화에 의존해야 하며, 구체화에 의존하면 안된다.** 원칙
* 의존성 주입은 이 원칙을 따르는 방법 중 하나이다. 구현 클래스에 의존하지 말고 인터페이스에 의존하라는 의미
* 위에 언급한 **역할**에 의존하라는 의미이다. (K3, 아반떼에 의존이 아닌 Car에 의존하라)
* 구현체에 의존하게 되면 변경이 어려워지게 된다.

```java
@Service
public class MemberService {
    
    // private MemberRepository memberRepository = new JdbcMemberRepository();
    private MemberRepository memberRepository = new MybatisMemberRepository();
}
```
* 이 코드는 인터페이스에 의존하는 것 같지만 **구현 클래스**에도 직접 의존하고 있다. (Mybatis....)
* DIP 코드를 만족하기 위해선 구현이 아닌 단순히 MemberRepository에만 의존해야 코드의 변경이 용이하다.
