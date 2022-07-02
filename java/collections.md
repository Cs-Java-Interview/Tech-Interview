# 자바 컬렉션 프레임워크(Java Collections Framework)

자바 컬렉션 프레임워크(Java Collections Framework)란 객체들을 모아놓은 자료구조를 조작하기 위해 여러 메소드들을 포함하고 있는 프레임워크이다.

컬렉션 인터페이스들은 **제네릭(Generics)** 으로 표현되어 컴파일 시점에서 객체의 타입을 체크하기 때문에 런타임 에러를 줄이는 데 도움이 된다.

컬렉션에는 대표적으로 List, Set, Map 인터페이스가 있으며 이를 구현한 다양한 클래스가 존재하고 각각의 특징들 또한 다양하다.

![image](https://user-images.githubusercontent.com/36829127/176997723-351083a2-f45a-4655-9d6a-fed639479115.png)

#### 컬렉션 프레임워크 구현관계 요약
* **List:** ArrayList, LinkedList, Vector, Stack
* **Set:** HashSet, LinkedHashSet, TreeSet
* **Queue:** priorityQueue, ArrayDeque
* **Map:** HashMap, LinkedHashMap, Hashtable, TreeMap


## Collection 인터페이스 그룹

* List, Set, Queue 인터페이스는 Collection 인터페이스를 상속받는다. (Map은 Key-value 구조로 인해 Collection 인터페이스를 상속받지 않고 별도로 정의한다)

### Collection 대표적인 메서드

* boolean add(E e) : 해당 컬렉션에 전달된 요소를 추가
* boolean remove(Object o) : 해당 컬렉션에서 전달된 객체를 제거
* void clear() : 해당 컬렉션의 모든 요소를 제거
* boolean contains(Object o) : 해당 컬렉션이 전달된 객체를 포함하고 있는지
* boolean equals(Object o) : 해당 컬렉션과 전달된 객체가 같은지
* boolean isEmpty() : 해당 컬렉션이 비어있는지
* Iterator <E> iterator() : 해당 컬렉션의 반복자(iterator)를 반환
* int size() : 해당 컬렉션의 요소의 총개수를 반환
* Object [] toArray() : 해당 컬렉션의 모든 요소를 Object 타입의 배열로 반환

## List 컬렉션
* 컬렉션 프레임워크를 상속받고 있으면 객체를 일렬로 늘어놓은 구조를 갖고 있다.
* List 컬렉션은 객체를 인덱스로 관리하기 때문에 객체를 저장하면 자동으로 인덱스가 부여되고 인덱스로 검색, 삭제를 할 수 있다.
* List 컬렉션은 객체 자체를 저장하는 것이 아니라 객체의 번지를 참조한다. 따라서 동일한 객체를 중복 저장할 수 있다.
* List 컬렉션을 구현하는 대표적인 클래스는 Array, LinkedList, Vector, Stack, Queue가 있다.

### ArrayList
List 인터페이스의 구현 클래스이다.
* 가장 배열과 유사하다. 데이터를 저장하는 공간의 크기를 가변적으로 늘릴 수 있다.
* index를 이용해 요소를 참조한다.
* 멀티 스레드 환경을 고려하지 않아 Thread Safe하지 않다.(synchronized 키워드나 Vector를 이용해서 해결해야 함)
  
### LinkedList
* LinkedList 구조로 이루어져 있다.
* 연속된 메모리를 저장하지 않고 pointer 방식으로 데이터를 연결지어 저장한다.
* 각 노드의 주소 정보만 바꿔주면 되기 때문에 삽입, 삭제 성능이 좋다.

### Vector
* ArrayList와 동일한 내부 구조를 사용하며 같은 특징
* 동기화된 메소드를 구성되어 있어 Thread Safe하다.
* 단일 스레드 환경에서도 동기화를 사용하기 때문에 비효율적이며, 성능면에서도 동일한 구조를 갖는 ArrayList보다 떨어진다.

### Stack
Stack은 LIFO(Last-In-First-Out) 특성을 가지는 자료구조이다.
* Stack은 Vector를 implements 받는다. 하지만 push(), pop()은 Stack 클래스 내부에 구현됐기 때문에, 이 메소드를 사용하려면 Stack으로 받아야 함.
  
## Set 
Set은 집합이라고도 부르며, 순서가 없고 원소의 중복을 허용하지 않는다.

### HashSet
HashSet은 집합의 특징 때문에 중복을 허용하지 않는다. 또한 순서를 갖지 않음
* 대표적으로 많이 사용되는 집합(Set)구조이다.

### LinkedHashSet
원래 집합의 특징은 중복을 허용하지 않고, 순서를 가지지 않는다는 특징이 있지만

LinkedHashSet은 중복은 허용하지 않지만, **순서는 가진다.**

### TreeSet
TreeSet도 중복을 허용하지 않고, 순서를 가지지 않는다.
* 하지만 데이터를 정렬하여 저장하고 있다는 특징이 있다.

  
  
## Queue 컬렉션
FIFO(First-In-First-Out) 자료구조를 갖고 있음.
* 처음 들어온 원소가 처음으로 나간다는 특징이 있다.
* 들어올때는 enqueue, 나갈땐 dequeue라고 함.

### PriorityQueue
일반 큐와는 다르게 원소에 우선순위를 부여하여 높은 순으로 먼저 반환한다.
* 이진 트리 구조로 구현되어 있음
```java
    // 오름차순 (default)
    Queue<Integer> priorityQueue = new PriorityQueue<>();

    // 내림차순
    Queue<Integer> priorityQueue2 = new PriorityQueue<>(Collections.reverseOrder());
```

### ArrayDeque
deque는 양쪽으로 넣고 빼는것이 가능한 자료구조이다.
![image](https://user-images.githubusercontent.com/36829127/176998166-451620fc-d520-4d12-988a-43ad2dbdbb51.png)

## Map 
Map은 `key-value`로 구성된 자료구조이다.
* Set처럼 순서를 가지 않음
* Key값은 중복이 불가능하고, Value는 가능하다.

### HashMap
HashMap은 일반적으로 많이 사용되는 Map 자료구조이다.
* 해시함수를 이용한 구조
* 정렬이 불가능하고 삽입 데이터의 순서를 보장하지 않음
* 삽입 / 삭제 / 조회 연산이 O(1)을 보장한다.

### HashTable
HashMap과 동일한 특징을 가지지만, 차이점이라고 하면 Thread-Safe하여 동기화를 지원한다.

### LinkedHashMap
일반적으로 Map 자료구조는 순서를 가지지 않지만, LinkedHashMap은 들어온 순서대로 순서를 가진다.

### TreeMap
이진트리로 구성되어 있고, TreeSet과 같이 정렬하여 데이터를 저장하고 있다.
* 데이터를 저장할때 정렬하기 때문에 저장 시간이 다른 자료구조에 비해 오래걸림

### Map 주요 메서드
* V put(K Key, V value)	: 주어진 키와 값을 추가하여 저장되면 값을 리턴합니다.
* boolean containsKey(Object Key)	: 주어진 키가 있는지 확인합니다.
* boolean containsValue(Object value)	: 주어진 값이 있는지 확인합니다.
* Set<Map.Entry<K,V>> entrySet()	: 모든 Map.Entry 객체를 Set에 담아 리턴합니다.
* Set<K> keySet()	: 모든 키를 Set객체에 담아서 리턴합니다.
* V get(Object key)	: 주어진 키에 있는 값을 리턴합니다.
* boolean isEmpty()	: 컬렉션이 비어있는지 조사합니다.
* int Size()	: 저장되어 있는 전체 객체의 수를 리턴합니다.
* Collection<V> values()	: 저장된 모든 값을 Collection에 담아서 리턴합니다.
* void clear()	: 저장된 모든 Map.Entry를 삭제합니다.
* V remove(Object Key)	: 주어진 키와 일치하는 Map.Entry를 삭제하고 값을 리턴합니다.


참조 : 
https://gbsb.tistory.com/247#map-interface-groups-classes

https://fbtmdwhd33.tistory.com/255

https://st-lab.tistory.com/142

https://coding-factory.tistory.com/550
