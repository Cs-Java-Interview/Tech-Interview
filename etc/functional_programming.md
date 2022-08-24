# 함수형 프로그래밍(Functional Programming)

### 프로그래밍 패러다임(Programming Paradigm)

**프로그래밍 패러다임(Programming Paradigm)**은 프로그래머에게 프로그래밍의 관점을 갖게 하고 코드를 어떻게 작성할 지 결정하는 역할을 한다. 새로운 프로그래밍 패러다임을 통해서는 새로운 방식으로 생각하는 법을 배우게 되고, 이를 바탕으로 코드를 작성하게 된다.

최근의 프로그래밍 패러다임은 크게 아래와 같이 구분할 수 있다.

- **명령형** 프로그래밍: 무엇(What)을 할 것인지 나타내기보다 **어떻게(How) 할 것인지**를 설명하는 방식
	- `절차지향` 프로그래밍: 수행되어야 할 **순차적인 처리과정**을 포함하는 방식(C, C++)
	- `객체지향` 프로그래밍: 객체들의 집합으로 **프로그램의 상호작용**을 표현(C++, Java, C#)
- **선언형** 프로그래밍: 어떻게 할건지(How)를 나타내기보다 **무엇(What)을 할 건지**를 설명하는 방식
	- `함수형` 프로그래밍: **순수 함수를 조합**하고 소프트웨어를 만드는 방식(클로저, 하스켈, 리스프)

### 함수형 프로그래밍의 등장

명령형 프로그래밍을 기반으로 개발했던 개발자들은 개발하는 소프트웨어의 크기가 커짐에 따라서, 복잡하게 엉켜있는 스파게티 코드를 유지보수하는 것이 매우 힘들다는 것을 깨닫게 되었다. 그리고 이를 해결하기 위해 함수형 프로그래밍이라는 프로그래밍 패러다임에 관심을 갖게 되었다. 함수형 프로그래밍은 **거의 모든 것을 순수함수로 나누어 문제를 해결**하는 기법으로, 작은 문제를 해결하기 위한 함수를 작성하여 **가독성을 높이고 유지보수를 용이하게 해준다.**

### 함수형 프로그래밍에 대한 이해

```java
for(int i = 1; i < 10; i++){
	System.out.println(i);
}
```

앞서 설명하였듯이 함수형 프로그래밍은 대입문을 사용하지 않는 프로그래밍이며, 작은 문제를 해결하기 위한 함수를 작성한다고 설명하였다. 그렇기 때문에 함수형 프로그래밍에서는 위와 같은 코드를 다음과 같이 해결할 수 있다.

```java
process(10, print(num));
```

process 함수는 첫 번째 인자로 몇까지 iteration을 돌 것인가를 매개변수로 받고 있고, 두 번째 인자로 전달받은 값을 출력하라는 함수를 매개변수로 받고있다. 앞서 설명하였듯 함수형 프로그래밍은 무엇을(What)에 포커스를 두는 프로그래밍이라고 하였다. 그렇기 때문에 함수형 프로그래밍에서는 출력을 하는 함수를 파라미터로 넘길 수 있다.

명령형 프로그래밍에서는 메서드를 호출하면 상황에 따라서 내부의 값이 바뀔 수 있다. 즉, 우리가 개발한 함수 내에서 선언된 변수의 메모리에 할당된 값이 바뀌는 등의 변화가 발생할 수 있다. 하지만 함수형 프로그래밍에서는 대입문이 없기 때문에 메모리에 한 번 할당된 값은 새로운 값으로 변할 수 없다.

### 함수형 프로그래밍(Functional Programming)의 특징

> **부수 효과**가 없는 **순수 함수**를 **1급객체**로 간주하여 파라미터나 반환값으로 사용할 수 있으며, **참조 투명성**을 지킬 수 있다.

함수형 프로그래밍의 특징을 한줄로 요약하면 위와 같다.

#### 부수효과(Side Effect)

여기서 부수효과(Side Effect)란 다음과 같은 변화 또는 변화가 발생하는 작업을 의미한다.

- 변수의 **값이 변경**됨
- **자료 구조를 제자리에서 수정**함
- 객체의 **필드값을 설정**함
- 예외나 오류가 발생하며 실행이 중단됨
- 콘솔 또는 파일 I/O가 발생함

#### 순수 함수(Pure Function)

그리고 이러한 부수 효과(Side Effect)들을 제거한 함수들을 `순수 함수(Pure Function)`이라고 부르며, 함수형 프로그래밍에서 사용하는 함수는 이러한 순수 함수들이다.

- Memory or I/O의 관점에서 Side Effect가 없는 함수
- 함수의 실행이 **외부에 영향을 끼치지 않는** 함수

#### 순수 함수(Pure Function)의 장점

순수 함수(Pure Function)을 이용하면 얻을 수 있는 효과는 다음과 같다.

- 함수 자체가 **독립적**이며 **Side-Effect가 없기 때문에 Thread에 안정성을 보장**받을 수 있다.
- Thread에 안정성을 보장받아 **병렬 처리를 동기화 없이 진행**할 수 있다.

#### 1급 객체(First-Class Object)

1급 객체란 다음과 같은 것들이 가능한 객체를 의미한다.

- 변수나 데이터 구조안에 담을 수 있다.
- **파라미터로 전달**할 수 있다.
- **반환값으로 사용**할 수 있다.
- 할당에 사용된 **이름과 무관하게 고유한 구별이 가능**하다.

함수형 프로그래밍에서 **함수는 1급 객체로 취급**받기 때문에 위의 예제에서 본 것처럼 함수를 파라미터로 넘기는 등의 작업이 가능한 것이다. 또한 우리가 일반적으로 알고 개발했던 함수들은 함수형 프로그래밍에서 정의하는 순수 함수들과는 다르다는 것을 인지해야 한다.

#### 참조 투명성(Referential Transparency)

마지막으로 참조 투명성(Referential Transparency)이란 다음과 같다.

- **동일한 인자에 대해 항상 동일한 결과를 반환**해야 한다.
- 참조 투명성을 통해 기존의 값은 변경되지 않고 유지된다. (Immutable Data)

명령형 프로그래밍과 함수형 프로그래밍에서 사용하는 함수는 **부수효과의 유/무**에 따라 차이가 있다. 그에 따라 함수가 참조에 투명한지 안한지 나뉘어 지는데, 참조에 투명하다는 것은 말 그대로 함수를 실행하여도 어떠한 상태의 변화없이 항상 동일한 결과를 반환하여 항상 동일하게(투명하게) 실행 결과를 참조(예측)할 수 있다는 것을 의미한다.

즉, 어떤 함수 f에 어떠한 인자 x를 넣고 f를 실행하게 되면, f는 입력된 인자에만 의존하므로 항상 f(x)라는 동일한 결과를 얻는다는 것을 의미한다. **부작용을 제거**하여 프로그램의 동작을 이해하고 예측을 용이하게 하는 것은 함수형 프로그래밍으로 개발하려는 핵심 동기중 하나이다. 그리고 이러한 부분인 병렬 처리 환경에서 개발할 때 Race Condition에 대한 비용을 줄여준다. 왜냐하면 함수형 프로그래밍에서는 값의 대입이 없이 항상 동일한 실행에 대해 동일한 결과를 반환하기 때문이다.