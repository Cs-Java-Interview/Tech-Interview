# Java의 메모리 구조

## JVM이 실행되는 과정

![image](https://user-images.githubusercontent.com/36829127/176988699-095a3e39-6bf8-46df-9988-d6f8be898747.png)

1. 자바 소스코드인 .java 파일을 컴파일러가 자바 바이트 코드인 .class로 변환합니다.

2. .class 코드를 JVM의 클래스 로더에게 보냅니다.

3. 클래스 토더는 JVM Runtime Data Area으로 로딩하여 JVM의 메모리에 올립니다.

4. Runtime Data Area에 로딩된 .class들은 Execution Engine을 통해 해석합니다.

5. 해석된 바이트 코드는 Runtime Data Area의 각 영역에 배치되어 수행하며 이 과정에서 Execution Engine에 의해 GC의 작동과 스레드 동기화가 이루어집니다.

## 런타임 데이터 영역

런타임 데이터 영역은 JVM의 메모리 영역으로 자바 애플리케이션을 실행할 때 사용되는 데이터를 적재하는 영역이다.

* **모든 스레드가 공유해서 사용(GC의 대상)**
  * 힙 영역(Heap Area)
  * 메서드 영역(Method 영역)

* **스레드마다 하나씩 생성**
  * 스택 영역(Stack Area)
  * PC 레지스터(PC Register)
  * 네이티브 메서드 스택(Native Method Stack)

### 힙 영역 (Heap Area)
![image](https://user-images.githubusercontent.com/36829127/176988986-825a17b1-da4e-4bcb-a637-278e79fc7027.png)

* **new** 키워드로 생성된 객체와 배열이 생성되는 영역
* 주기적으로 GC가 제거하는 영역


힙 영역은 더 자세하게는 GC를 위해 크게 3가지 영역으로 나뉜다.

Young Generation은 자바 객체가 생성되자마자 저장되고, 생긴지 얼마 안된 객체가 저장되는 공간. 
Heap 영역에 객체가 생성되면 최초로 Eden 영역에 할당된다. 
이 영역에서 데이터가 쌓이게 되면 Survivor 0 또는 1로 이동하게 되거나 minor GC에 의해 회수된다.



Young generation에서 일정 Age bit을 넘은 객체들은 Old generation으로 이동되거나 회수된다. 
Old generation에서도 메모리가 가득차게 되면 major GC를 실행하여 메모리를 회수한다.


### 스택 영역 (Stack Area)
지역변수, 파라미터, 리턴 값, 연산에 사용되는 임시 값, 메서드 호출 등 임시적인 데이터들이 생성되는 영역이며 Primitive Type 자료형을 생성할 때 저장하는 공간이다.
정적 메모리 할당이 이루어지는 장소로 Heap 영역에 동적 할당된 값에 대한 참조를 얻을 수 있다.

ex) Person p = new Person(); 에서 Person p는 Stack 영역, new Person();은 Heap 영역에 할당된다.


### PC 레지스터 (PC Register)
쓰레드가 생성될 때마다 생성되는 영역으로 **쓰레드마다 하나씩** 존재한다. 
현재 쓰레드가 실행되는 부분의 주소와 명령을 저장하고 있는 영역이다. 
이것을 이용하여 쓰레드를 돌아가면서 수행할 수 있도록 한다.


### 네이티브 메서드 스택 (Native Method Stack)
자바 의외의 언어(C, C++, 어셈블리 등)로 작성된 네이티브 코드를 실행할 때 사용되는 메모리 영역으로 일반적인 C 스택을 사용

일반적인 메소드를 실행하는 경우 JVM 스택에 쌓이다가 해당 메소드 내부에 네이티브 방식을 사용하는 메소드(예를 들면 C언어로 작성된 메소드)가 있다면 해당 메소드는 네이티브 스택에 쌓인다.





