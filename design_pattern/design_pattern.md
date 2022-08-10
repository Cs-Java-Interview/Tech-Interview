## 디자인 패턴

<aside>

    💡 개발 과정에서 자주 발생하는 문제들을 해결하기 위한 설계 방법을 정리한 패턴

</aside>

- 개발의 효율성과 유지보수성, 운용성이 높아지며 프로그램의 최적화에 도움이 된다.

## 디자인 패턴의 종류

- GoF 디자인 패턴
    - GoF(Gang of Four)라 불리는 사람들
        - 에리히 감마(Erich Gamma), 리차드 헬름(Richard Helm), 랄프 존슨(Ralph Johnson), 존 블리시디스(John Vissides)
        - 소프트웨어 개발 영역에서 디자인 패턴을 구체화하고 체계화한 사람들
        - 23가지의 디자인 패턴을 정리하고 각각의 디자인 패턴을 생성(Creational), 구조(Structural), 행위(Behavioral) 3가지로 분류했다.
        - ![image](https://user-images.githubusercontent.com/77563814/183808140-046c0bcb-3273-45e0-9d3f-58dd136f3ced.png)



- **생성 패턴(Creational Patterns)** : 객체의 생성 방식 결정
    - 객체 생성에 관련된 패턴
    - 객체의 생성과 조합을 캡슐화해 특정 객체가 생성/변경되어도 프로그램 구조에 영향을 크게 받지 않도록 유연성을 제공한다.
    - ex) 싱글톤 패턴(Singleton), 추상팩토리 패턴(Abstract Factory), 빌더 패턴(Builder), 팩토리 메서드 패턴(Factory Method)
- ****구조 패턴(Structural Patterns)**** : 객체간의 관계를 조직
    - 클래스나 객체를 조합해 더 큰 구조를 만드는 패턴
    - 예로, 서로 다른 객체나 인터페이스를 조합하는 경우
    - ex) 적응자 패턴(Adapter), 브리지 패턴(Bridge), 프록시 패턴(Proxy)
- ****행위 패턴(Behavioral Patterns)**** : 객체의 행위를 조직, 관리, 연합
    - 객체나 클래스 사이의 알고리즘이나 책임 분배에 관련된 패턴
    - 한 객체가 혼자 수행할 수 없는 작업을 여러개의 객체로 어떻게 분배하는지, 또 그렇게 하면서도 객체 사이의 결합도를 최소화하는것에 중점을 두는 방식
    - ex) 옵저버 패턴(Observer), 상태 패턴(State), 스트레이트지 패턴(Strategy), 템플릿 패턴(Template)
