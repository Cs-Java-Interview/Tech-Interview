# GC(Garbage Collection) 처리 방법

## GC(Garbage Collection)
GC는 메모리 관리 기법 중 하나로써, **동적으로 할당했던 메모리 영역 중 필요가 없게 된 영역을 해제하는 기능이다.**

여기서 동적으로 할당했던 메모리 영역은 프로그램 런타임에 사용되는 Heap 영역 메모리를 뜻하고, 필요 없게 된 영역은 어떤 변수도 가리키지 않게 된 영역을 뜻한다.

해당 영역을 어느 시점에서 해제하는 것이 바로 GC의 역할이다.


```java
Person person = new Person(); 
person.setName("Atrox"); 
person = null;

person = new Person(); 
person.setName("Ezreal");
```

* person 변수는 기존 Atrox 이름이 붙은 Person 객체가 존재하는 메모리 영역을 가리키고 있었으나, 추후 Ezreal 이름이 붙은 Person 객체가 존재하는 메모리 영역을 가리키게 된다.

* 이때 Atrox 이름이 붙었던 Person 객체의 메모리 영역은 그 어떤 변수도 가리키고 있지 않다. 따라서 해당 영역을 어느 시점에서 자동으로 해제하는 것이 바로 GC의 역할이다.

C와 C++에서 Heap 영역의 메모리를 사용하기 위해서는 코드에서 직접 동적 메모리 영역을 할당 받고 해제하는 과정을 진행해야 했다.

하지만 이를 수동으로 직접 관리하는 것은 번거롭고 실수를 하기 쉽다. 가령 메모리 영역을 할당 받고 해제하지 않으면 **메모리 누수**가 발생할 수 있고,

이미 해제한 메모리 영역을 또 해제하면 에러가 발생할 수 있다. 반면, Java에서는 동적 메모리 영역을 GC가 알아서 수행해주므로 개발자 입장에서는 편하게 개발에 집중할 수 있다.


## GC의 장단점
### 장점
* 메모리를 수동으로 관리하던 것에서 비롯된 에러를 예방할 수 있다.
  * 개발자의 실수로 인한 메모리 누수
  * 해제된 메모리를 또 해제하는 이중 해제
  * 해제된 메모리에 접근


### 단점
* GC의 메모리 해제 타이밍을 개발자가 정확히 알기 어렵다.
* 어떠한 메모리 영역이 해제의 대상이 될 지 검사하고, 실제로 해제하는 일이 모두 오버헤드다.

## GC에서 사용하는 알고리즘

### Reference Counting
![image](https://user-images.githubusercontent.com/36829127/175978781-70ec1a26-dc66-4d3c-8eff-8e37420f4c4e.png)

그림 속 Root Space는 스택 변수, 전역 변수 등 heap 영역 참조를 가리키고 있는 공간이다.

**Reference Counting**은 힙 영역의 객체들이 각각 reference count라는 숫자를 가지고 있다. 여기서 reference count는 몇 가지 방법으로 해당 객체에 접근할 수 있는지를 뜻한다. 
만약 reference count가 0에 다다르면 해당 객체에 접근할 수 있는 방법이 없다는 뜻이므로 메모리 해제의 대상이 되는 것이다.

하지만 Reference Counting은 순환참조 문제가 발생할 수 있다. 그림 속 Root Space에서 모든 Heap Space를 끊는다고 가정하면 노란 색 고리 안의 객체는 서로가 서로를 참조하고 있기 때문에
reference count가 1로 유지된다. 결국 사용하지 않는 메모리 영역이 해제되지 못하고 메모리 누수가 발생하는 것이다.

### Mark And Sweep
![image](https://user-images.githubusercontent.com/36829127/175979616-cb0adfa7-e5a0-4c56-a3bf-afd4ebcf915b.png)
**Mark And Sweep** 알고리즘은 Reference Counting의 순환 참조 문제를 해결할 수 있다.
Mark And Sweep 알고리즘은 Root Space부터 해당 객체에 접근 가능한지, 아닌지를 메모리 해제의 기준으로 삼는다. 

Root Space부터 그래프 순회를 통해 연결된 객체를 찾아내고 **(Mark)** 연결이 끊어진 객체는 지운다.**(Sweep)**
Root Space부터 연결된 객체는 Reachable, 연결되지 않은 객체는 Unreachable라고 부른다.

그림에서는 Sweep 이후에 분산되어 있던 던 메모리가 예쁘게 정리된 것을 볼 수 있는데, 이것은 메모리 파편화를 방지하는 Compaction 과정이다. 다만, Mark And Sweep 알고리즘에서 Compaction이 필수는 아니다.

이렇게 Mark And Sweep 방식을 사용하면, Root Space부터 연결이 끊긴 순환 참조되는 객체들도 지울 수 있다. **Java와 JavaScript가 Mark And Sweep 방식으로 메모리 관리를 한다.**

다만 객체의 ref count가 0이 되면 지워버리는 Reference Counting 방식과는 다르게 Mark And Sweep은 의도적으로 특정 순간에 GC를 실행해야 한다. 즉, 어느 순간에는 실행 중인 애플리케이션이 GC에게 컴퓨터 리소스를 내주어야 한다는 듯이다.
따라서 **애플리케이션의 사용성을 유지하며 효율적으로 GC를 실행**하는 것이 중요하다.

#### Mark And Sweep의 특징
* 의도적으로 GC를 실행해야 한다.
* 애플리케이션과 GC 실행이 병행된다.


## 가비지 컬렉션 동작 과정
GC에 대해서 알아보기 전에 알아야 할 용어가 있다. 바로 `stop-the-world`이다. stop-the-world란, GC를 실행하기 위해 JVM이 애플리케이션 실행을 멈추는 것이다. 
stop-the-world가 발생하면 GC를 실행하는 쓰레드를 제외한 **나머지 쓰레드는 모두 작업을 멈춘다.** 이 GC의 작업이 완료된 이후에야 중단됐던 작업을 다시 시작한다.

Java는 프로그램 코드에서 메모리를 명시적으로 지정하여 해제하지 않기 때문에 가비지 컬렉터가 더 이상 필요 없는 (쓰레기) 객체를 찾아 지우는 작업을 한다. 이 가비지 컬렉터는 두 가지 가설 하에 만들어졌다.

* 대부분의 객체는 금방 불가능 상태(unreachable)가 된다.
* 오래된 객체에서 젊은 객체로의 참조는 아주 적게 존재한다.

이 가설을 `weak generational hypothesis`라고 하며, 이 가설의 장점을 최대한 살리기 위해 HotSpot VM에서는 크게 2개로 물리적 공간을 나누었다. 둘로 나눈 공간이 **Young** 영역과 **Old**영역이다.

### Root Space
![image](https://user-images.githubusercontent.com/36829127/175983085-795cc9aa-f172-4c15-ad07-3f952c647d72.png)


JVM Memory 영역 중 Root Space는 다음 3가지가 해당된다.
* Stack의 로컬 변수
* Method Area의 Static 변수
* Native Method Stack의 JNI 참조

### GC의 실행 타이밍
Mark And Sweep의 첫번째 특징은 **의도적으로 GC를 실행**한다는 것이다.
실행하는 순간을 알기 위해선 Heap 영역을 조금 더 디테일하게 살펴 보아야 한다.

![image](https://user-images.githubusercontent.com/36829127/176436389-bf76e070-b1ba-4120-813d-6d374fc2b360.png)

JVM의 Heap은 크게 `Young Generation`과 `Old Generation`으로 나뉜다. Young Generation에서 발생하는 GC는 `minor GC` Old Generation에서 발생하는 GC는 `major GC`라고 부른다.

Young Generation은 다시 `Eden`, `Survival 0 `, `Survival 1`로 나뉜다. Eden은 새롭게 생성된 객체들이 할당되는 영역이고, Survival은 minor GC에서 살아남은 객체들이 존재하는 영역이다.
이때 Survival에서 Survival 0과 1 중 **하나는 꼭 비어있어야 한다**는 규칙이 있다.

minor GC 실행 타이밍은 바로 Eden 영역이 꽉 찼을 때다.

![image](https://user-images.githubusercontent.com/36829127/176437360-66d732ac-1910-4ab7-afc0-f20517e97d78.png)
그림에서 회색 네모는 메모리에 할당된 객체를 생각하면 된다. minor gc가 발생하고 난 뒤 Reachable이라 판단된 객체는 Survival 0 영역으로 옮겨진다.

![image](https://user-images.githubusercontent.com/36829127/176437504-e28bd2e8-0e26-415f-ac78-775f53b39a4e.png)
이때 살아 남은 객체들의 숫자들이 0에서 1로 변했는데, 이는 `age bit`을 뜻한다. minor GC에서 살아남은 객체는 age bit이 1씩 증가한다.

![image](https://user-images.githubusercontent.com/36829127/176437692-a9810517-cbf8-4a35-91ee-6e0ebbf01941.png)
또 다시 에덴 영역이 꽉 찬 상황

![image](https://user-images.githubusercontent.com/36829127/176437736-6c72a7cd-03b4-4c54-ac9e-e7c050e660af.png)
그러면 minor gc가 발생하여 Reachable이라 판단된 객체들은 Survival 1 영역으로 이동한다.

![image](https://user-images.githubusercontent.com/36829127/176437866-e45d2579-2963-417f-a5cd-80e725aa33e8.png)
이후 또 Eden 영역이 꽉 찬 상황

![image](https://user-images.githubusercontent.com/36829127/176437906-ee5942c5-3dae-4460-890a-86d2c0c2b659.png)
그렇다면 minor GC가 발생하여  Reachable이라 판단된 객체들은 Survival 0 영역으로 이동한다.

보시면 가장 오래 살아남은 객체의 age bit이 3이 된 것을 확인할 수 있다.

JVM GC에서는 일정 수준의 age bit을 넘어가면 오래도록 참조될 객체라고 판단하고, 해당 객체를 Old generation에 넘겨주는데 이를 `Promotion`이라고 부른다.
Java 8에서는 Parallel GC 방식 기준으로 age bit가 15가 되면 Promotion이 진행된다.

![image](https://user-images.githubusercontent.com/36829127/176438326-be82837a-d6db-40de-8562-82b2b2ea7ec8.png)
이번 예제에서는 age bit이 3이 될 경우 Promotion이 일어난다. 그래서 survival 0 영역의 age bit이 3인 객체가 Old Generation으로 Promotion 되었다.

![image](https://user-images.githubusercontent.com/36829127/176438533-34de1c7f-b2f3-43ad-bac2-545f2f2bc0f2.png)

시간이 많이 지나면 old Generation도 다 채워지는 날이 올테고, **이 때 major GC가 발생**하면서 Mark And Sweep 방식을 통해 필요없는 메모리를 비우는데 minor GC보다 시간이 오래 걸린다.

JVM이 멈추는 stop the world 현상의 소요시간이 minor GC < major GC이다.

![image](https://user-images.githubusercontent.com/36829127/176438912-f041ac05-6147-47d5-b242-592dc00b5f61.png)

그렇다면 Heap 영역을 굳이 Young과 Old로 나눈 이유는 무엇일까?

GC 설계자들이 애플리케이션을 분석해본 겨로가 대부분의 객체의 수명이 짧다는 걸 깨달았고, 메모리 전체가 아닌 일부 부분만 탐색하는 것이 효율적이기 때문이다.
어차피 대부분의 객체가 금방 사라지니 Young Generation 안에서 최대한 메모리를 해제하도록 한 것이다.


## GC 실행 종류

### Serial GC
![image](https://user-images.githubusercontent.com/36829127/176439552-6540c39f-e15c-4865-9590-7ac7557d563e.png)

* Serial GC는 하나의 스레드로 GC를 실행하는 방식
* 하나의 스레드로 실행하기 때문에 Stop The World 시간이 길다.
* Mark And Sweep 이후 메모리 파편화를 막는 Compaction 과정도 진행된다.

### Parallel GC
![image](https://user-images.githubusercontent.com/36829127/176439847-5b4150cb-429d-4d95-87c1-d042a6b0ecbb.png)

* Parallel GC는 여러개의 스레드로 GC를 실행하므로 Stop The World 시간이 짧다.
* 멀티 코어 환경에서 작동하며 Java 8의 기본 GC방식
* GC의 오버헤드를 줄여주긴 하지만, 애플리케이션이 멈추는 것은 피할 수 없다.
* minor GC에서만 멀티 스레딩을 수행하고 major GC는 싱글 스레딩으로 작동한다.

### Parallel Old GC
* Parallel GC의 업그레이드 버전으로 **major GC도 멀티 스레딩으로 수행한다.**
* 기존 Mark Sweep Compaction의 개선 버전인 Mark Summary Compaction을 사용한다.
* 사실 Java 7 Update 4 이후는 Parallel GC를 설정해도 Parallel Old GC가 동작. Java 8의 Default 버전은 Parallel Old GC이다.

### CMS GC
![image](https://user-images.githubusercontent.com/36829127/176440565-ea9ca1a7-d339-45d1-91a7-8791bbb39d36.png)

* CMS 란 Concurrent-Mark-Sweep의 줄임말로, Stop The World를 줄이기 위해 고안됐다.
* 대부분의 가비지 컬렉션 과정을 애플리케이션 스레드와 동시에 수행하여 시간을 줄인다.
* 하지만 CPU를 많이 사용하고 MAS 과정 이후 메모리 파편화를 해결하는 Compaction이 기본적으로 제공되지 않는다.
* 장기적으로 운영되다가 조각난 메모리들이 많다면 오히려 Stop The World 시간이 늘어나는 단점이 있다.
* Java 9 버전부터 deprecated됐고 Java 14부터는 중단됐다.

> * Initial Mark: 클래스 로더에서 가장 가까운 객체 중 살아 있는 객체만 찾는다.
> * Concurrent Mark: 위에서 살아 있다고 확인한 객체에서 참조되어 있는 객체를 확인한다.
> * Remark: 위 단계에서 새로 추가되거나 참조가 끊긴 객체를 확인한다.
> * Concurrent Sweep: 쓰레기를 정리한다.


### G1 GC
![image](https://user-images.githubusercontent.com/36829127/176441142-32549980-6490-4372-bd2f-ea4ebcee092c.png)

* G1은 Garbage First의 줄임말로 Heap영억을 절반으로 쪼갰던 위의 방식과는 다르게 작동한다.
* Heap 영역을 잘게 쪼개 Region으로 나누고 어떤 부분은 Young Generation으로 또 다른 부분은 Old Generation으로 사용한다.
* 런타임에 따라 G1 GC가 필요에 따라 영역 별 Region 개수를 튜닝하므로써 STW를 최소화 한다.
* Java 9 부터는 G1 GC가 기본 방식이다.
* 자세한 내용 : https://steady-coding.tistory.com/590


참고)

https://steady-coding.tistory.com/584

https://d2.naver.com/helloworld/1329
