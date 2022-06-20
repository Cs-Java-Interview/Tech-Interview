# OOP(객체 지향 프로그래밍) 4개 특징

OOP 프로그래밍 4대 특징은 다음과 같다.
1. 캡슐화(Encapsulation)
2. 상속(Inheritance)
3. 추상화(Abstraction)
4. 다형성(Polymorphism)


## 1. 캡슐화(Encapsulation)

* 객체가 맡은 역할을 수행하기 하나의 목적을 기준으로, 프로퍼티와 메소드를 묶는 것
* 캡슐화의 목적은 객체를 캡슐로 싸서 **내부를 보호하고 볼 수 없게 하는 것**
* 캡슐화는 **정보 은닉**을 통해 **높은 응집도**와 **낮은 결합도**를 갖게 한다.
  * 정보 은닉 : `private`와 같은 키워드를 통해 필요 없는 정보는 외부에서 접근하지 못하도록 제한하는 것 

### Tell, Don't Ask(TDA 원칙)

**데이터를 달라고 하지 말고 시켜라**

객체 지향적 사고방식 원칙 중 하나인데, **캡츌화**가 잘 이루어진 설계를 위해 탄생한 원칙이다.
> 메시지 전송자는 메시지 수신자의 상태를 기반으로 결정을 내린 후 메시지 수신자의 상태를 바꿔서는 안 된다. 객체의 외부에서 해당 객체의 상태를 기반으로 결정을 내리는 것은 객체의 캡슐화를 위반한다.

**Tell, Don't Ask** 원칙을 따르면
* 밀접하게 연관된 정보와 행동을 함께 가지는 객체를 만들 수 있다.
* 객체의 정보를 이용하는 행동을 객체의 외부가 아닌 내부에 위치시키기 때문에 자연스럽게 정보와 행동을 동일한 클래스 안에 두게 된다.
* 보다 자연스럽게 정보 전문가에게 책임을 할당하게 되고 높은 응집도를 가진 클래스를 얻을 확률이 높아진다.

객체가 어떻게 작업을 수행하는지를 노출해서는 안 된다. 인터페이스는 객체가 어떻게 하는지가 아니라 **무엇을 해야하는지**를 서술해야 한다.

```java

//TDA 원칙을 지키지 않은 코드
if(acc.getMembership() == REGULAR) {
    ... 정회원 기능
}

//TDA 원칙을 지킨 코드
if(acc.hasRegularPermission()) {
    ... 정회원 기능
}
```


###  Demeter’s Law(데미테르 or 디미터 법칙)

**가까운 친구하고만 이야기해라**

- 메서드에서 생성한 객체의 메서드만 호출
- 파라미터로 받는객체의 메서드만 호출
- 필드로 참조하는 객체의 메서드만 호출

즉, 파라미터나 리턴값의 메소드를 연속 호출하지 말라는 뜻이다.
이러한 법칙이 생겨난 이유는 객체는 다른 객체가 어떠한 자료를 갖고 있는지 속사정을 몰라야 하기 때문이다.


```java

//디미터 법칙을 지키지 않은 코드
public void someMethod() {
    acc.getExpDate().isAfter(now)
    // ...
}
//Date라는 외부 객체의 메소드를 사용하고 있기 때문에 디미터 법칙을 위반한다


//디미터 법칙을 지킨 코드
public void someMethod() {
    Date date = acc.getExpDate();
    date.isAfter(now);
    // ...
}
```

* acc의 메소드만 사용하는 것 같았는데 알고보니 새로운 Date의 객체가 만들어졌고 Date의 메소드를 사용하게 된다.
* 이는 객체 간 **높은 결합도**를 갖게 하기 때문에 코드 변경 시 문제가 발생할 가능성이 높아진다.



## 2. 상속(Inheritance)
* 이미 작성된 상위클래스의 특성을 그대로 이어받아 새로운 클래스(하위클래스)를 생성하는 기법
* 기존의 코드를 재사용하거나 재정의하는 것 -> **재사용 + 확장**
* 상속은 계층적인 개념이 아니다. 부모와 자식 관계가 아닌 `동물과 포유류`와 같은 분류의 개념으로 보는게 옳음

![image](https://user-images.githubusercontent.com/36829127/174426348-e16d7f3c-a00c-4c59-93fd-34877a346ea9.png)

### 상속관계에서 만족해야만 하는 문장

**'하위클래스는 상위클래스이다.'**

- 영희는 아빠이다.(계층 관계는 일반화가 불가능하다)
- **펭귄은 동물이다.(분류의 경우 일반화가 가능하다)**


**'is a'보다는 'a kind of'**

* **펭귄은 조류의 한 분류이다.**


## 3. 추상화(Abstraction)
* 추상화란 구체적인 것을 분해해서 관찰자가 관심 있는 특성만 가지고 재조합하는 것이다. 
* 세부사항은 버리고 중요하고 공통적인 것만 취해 내가 관심있는 것에 집중하는 것

"사람 클래스"가 있다고 생각해보면 '먹다, 자다, 일하다, 울다, 몸무게, 시력, 나이 등' 여러 **공통된 특성**을 찾게 된다.
하지만 사람의 모든 특성을 나열할 필요는 없다.

**병원 프로그램**을 만든다면 **'시력, 몸무게 등'** 의 기능이 필요할 것이다.

**은행 프로그램**을 만든다면 **'시력, 몸무게 등'** 의 기능은 필요없고 **'나이, 연봉, 이체하다, 대출하다'** 등의 기능이 필요할 것이다.

추상화를 통해 구체적인 것을 분해해서 관심 영역만 갖고 재조합을 하는 것을 할 수 있으며, 이를 **모델링**라고도 한다.


## 4. 다형성(Polymorphism)

* 같은 기능(메소드)를 호출하더라도 객체에 따라 다르게 동작하는 것
* 예를 들어, 강아지, 고양이, 닭 클래스는 Animal 클래스를 상속받고 소리내기 메소드를 각각 다르게 구현할 수 있다. 이를 **오버라이딩**이라 한다.
* 또 다른 사례로 클래스 내에서 이름이 같지만 Parameter를 다른 형식으로 받는 등의 다르게 구현하는 **오버로딩**이 있다.

### 다형성의 본질
다형성을 통해 개발할 때 **역할**과 **구현**을 분리할 수 있다.

![image](https://user-images.githubusercontent.com/36829127/174437787-24896666-3602-4250-a801-645eb79d6bbe.png)


* (예시) 운전자 - 자동차 : 자동차는 전진, 후진, 브레이크 등의 자동차 **역할**이 있고, 그 역할을 구현한 K3, 아반떼 등과 같은 **구현**이 있다.
* 운전자는 차종이 바뀌어도 운전만 할 수 있다면 아무 영향을 받지 않는다.
* 클라이언트는 내가 사용하고자 하는 객체의 **역할**만 알면되고 그 객체의 **구현**이 어떻게 생겼는지는 몰라도 된다.
* 즉, **다형성의 본질** 은 인터페이스를 구현한 객체 인스턴스를 실행 시점에 유연하게 변경할 수 있다는 것이며 객체지향 프로그래밍의 가장 큰 강점이다.


