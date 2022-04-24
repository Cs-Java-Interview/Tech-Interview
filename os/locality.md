## 캐시의 지역성

`캐시 메모리`는 주기억장치에서 자주 사용하는 프로그램과 데이터를 저장해두어 속도를 빠르게 하는 메모리이다. `주기억장치`의 느린 속도를 보완하기 위해서 `CPU`와 `주기억장치` 사이의 위치하는 메모리이다. 즉, `CPU`의 처리속도와 `주기억장치`의 속도 차이로 인한 `병목현상`을 완화하기 위해서 사용하는 `고속 버퍼 메모리`이다.

이러한 캐시 메모리의 대략적인 개념을 보면 CPU가 어떤 데이터를 원할 것인지를 어느정도 예측을 할 수 있어야 한다. 캐시 메모리는 용량이 매우 작기때문에 CPU가 이후에 참조할 정보가 어느정도 들어있냐에 따라서 성능이 좌우되기 때문이다. 

CPU가 이후에 참조할 정보가 어느정도 들어있냐는 정도를 `적중율`이라고 하는데, 적중율을 높이기 위해서 `지역성`의 원리를 사용한다.

`지역성`은 기억장치 내의 정보를 어느 한순간에 특정 부분을 집중적으로 참조하는 특성을 말하는데, 대표적으로 `시간 지역성`과 `공간 지역성`이 존재한다.

### 시간 지역성

특정 데이터에 한번 접근해서 가져온 경우, 그 데이터가 가까운 미래에 또 한번 접근할 가능성이 높은 것을 의미한다. 즉, 한번 가져왔던 데이터를 또 쓸일이 있다는 것이다.

따라서 캐시는 반복적으로 사용되는 데이터가 많을수록 높은 효율을 낼수 있게된다.

### 공간 지역성

특정 데이터와 가까운 주소가 순서대로 접근되었을 경우 `공간 지역성` 이라고 한다. 즉, 앞으로 사용할 데이터들이 가져올 블록안에 많이 모여있는 경우를 뜻한다. 

필요한 데이터들이 모여있다면 한번만 메모리에 접근하면 필요한 데이터들을 많이 가져올 수 있어서 효율성이 증가할 것이고, 흩어져 있다면 효율성이 나빠질 것이다.