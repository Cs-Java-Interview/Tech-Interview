# 자바 컬렉션 프레임워크(Java Collections Framework)

자바 컬렉션 프레임워크(Java Collections Framework)란 객체들을 모아놓은 자료구조를 조작하기 위해 여러 메소드들을 포함하고 있는 프레임워크이다.

컬렉션 인터페이스들은 **제네릭(Generics)** 으로 표현되어 컴파일 시점에서 객체의 타입을 체크하기 때문에 런타임 에러를 줄이는 데 도움이 된다.

컬렉션에는 대표적으로 List, Set, Map 인터페이스가 있으며 이를 구현한 다양한 클래스가 존재하고 각각의 특징들 또한 다양하다.

![image](https://user-images.githubusercontent.com/36829127/176997154-ef792207-9d54-4bed-8337-1fe7a6354e8c.png)

## Collection 인터페이스 그룹

* List, Set 인터페이스는 Collection 인터페이스를 상속받는다. (Map은 Key-value 구조로 인해 Collection 인터페이스를 상속받지 않고 별도로 정의한다

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
  
  
