# 이벤트 기반 프로그래밍


## 이벤트 기반 프로그래밍이란?

* Event Driven Programming이라고 불리오며, 이벤트의 발생에 의해 프로그램의 흐름이 결정되는 프로그래밍 패러다임
* 여기서 이벤트는 사용자 또는 센서의 입력이거나 네트워크로부터의 데이터 송수신 혹은 다른 어플리케이션의 메시지 등이 될 수 있다.
* 이벤트 기반 프로그래밍 어플리케이션에서는 **이벤트가 감지되면** **콜백 함수를 트리거하는 메인루프를 사용하고,** , 임베디드 시스템에서는 반복적인 메인루프 대신 하드웨어 인터럽트를 사용

## 이벤트 기반 프로그래밍 배경

* 이러한 이벤트 기반의 프로그래밍은 **콘솔 프로그래밍** 의 단점보완에서 나왔다.
* 한번에 하나의 프로그래밍을 수행하는 콘솔 프로그래밍은 흐름 예측이 쉬웠고 순차적 프로그래밍에 적합했지만,
* 동시에 여러 프로그래밍을 실행하는 윈도우 프로그래밍은 이벤트가 다양하게 발생하기 때문에 흐름 예측이 어려웠다.
* 따라서, 동시에 수행되는 다른 프로그램을 고려하여 개발을 진행하게 됐고 이러한 구조를 운영체제가 알아서 처리해주는 프로그래밍을 이벤트 기반 프로그래밍이라고 한다.

## 이벤트 기반 프로그래밍 구조

![image](https://user-images.githubusercontent.com/36829127/186392699-7b1959d4-9b0d-42c3-ac38-2f2a6f8c0d0c.png)


* 이벤트 기반 프로그래밍 구조는 이벤트를 발생시키는 객체(**EventEmitter**), 이벤트를 관리하는 객체(**EventDispatcher**) 그리고 이벤트가 발생했을 때 실행되는 객체(**EventHandler**)로 구성되어 있다.
* EventEmitter에서 마우스 클릭과 같은 이벤트를 발생시키면 EventEmitter에서 실행되는 메소드들이 EventHandler가 되며, 이 둘을 바인딩시킨 것이 EventDispatcher가 된다.

## 이벤트 기반 프로그래밍 과정

* 이벤트 기반 프로그래밍 구조를 짤 때는 먼저 응답할 이벤트를 처리하는 루틴을 작성하고 이벤트 핸들러를 이벤트에 바인딩시킨다.
* 그 후에 이벤트 발생 여부를 확인하고 핸들러를 호출하는 메인루프를 작성한다.
* 후에 이렇게 작성된 이벤트 기반 프로그래밍은 이벤트가 발생하게 되면 이벤트 객체를 생성한다.
* 이벤트 객체는 마우스 좌표와 같은 현재 발생한 이벤트에 대한 여러 정보를 가진 객체이며, 이벤트를 처리하는 핸들러인 이벤트 리스너를 찾고 호출시킨다.
  * 개발자는 리스너 인터페이스의 추상메소드를 구현해줘야 한다.