# LinkedList

## LinkedList란?
### 각 노드가 `데이터`와 `포인터`를 갖고 해당 포인터로 한줄로 연결되어 있는 자료구조
![image](https://user-images.githubusercontent.com/36829127/171393629-f008303f-7750-4c08-8b49-29e4bde6e03c.png)
* Array와 다르게 정해진 크기가 없고 동적으로 크기를 늘릴 수 있다.
* Array와는 다르게 `물리적 저장 순서`와 `논리적 저장 순서`가 일치하지 않다.

## LinkedList의 특징
1. `물리적 저장순서`와 `논리적 저장순서`가 불일치
  * 자료의 주소 값으로 포인터를 가리키며 서로 연결되어 있는 구조

2. 데이터를 추가할 때마다 **동적으로 크기**가 증가

3. 원소 검색 시, 첫번째 노드부터 일일이 확인해야 한다.

4. 데이터의 이동 없이 **중간에 삽입/삭제**가 가능하다.
* 

5. LinkedList의 종류로는 `단일 연결리스트`, `원형 연결리스트`, `이중 연결리스트`가 있다.

![image](https://user-images.githubusercontent.com/36829127/171395228-7b84811e-b0f8-4428-9ecb-110fef5e24d1.png)

> 1. 단일 연결리스트 : 다음 노드를 가리키는 포인터만을 가지는 연결리스트
> 2. 원형 연결리스트 : 마지막 노드의 포인터가 NULL이 아닌 첫번째 순서의 노드를 가리키게 하는 연결리스트
> 3. 이중 연결리스트 : 다음 노드를 가리키는 포인터와 이전 노드를 가리키는 포인터를 동시에 갖는 연결리스트


## LinkedList의 연산 및 시간복잡도

1. 접근 : O(n)
  * 인덱스를 사용하는 Array와는 다르게 검색도 처음 노드부터 원하는 노드까지 일일이 확인하기 때문에 O(n)의 시간복잡도를 가진다.
  * 이는 Array는 다르게 `RandomAccess`가 불가능하다는 의미이다.
    * RandomAccess : 인덱스 번호를 알면 사칙연산 만으로도 빠르게 접근할 수 있는 접근이며 **임의 접근**이라고도 한다.

2. 검색 : O(n)
  * 처음 노드부터 일일이 확인해야 하기 때문에 O(n)의 시간복잡도를 가진다.

3. 삽입 : O(n)
  * 원하는 노드의 앞까지 접근해야 하기 때문에 O(n)의 시간복잡도를 가진다.
    * 만약 LinkedList의 맨 앞과 맨 뒤에 삽입연산을 진행하면 O(1)의 시간복잡도를 가질 것.


![image](https://user-images.githubusercontent.com/36829127/171397868-c4f9cb80-f2da-411c-ace5-5c010f0e4f15.png)

* 그림처럼 원하는 위치에 노드를 삽입하기 위해 노드를 생성해주고 `앞 노드의 포인터`가 가리키는 노드를 `새로운 노드`로 변경해주고, `새로운 노드의 포인터`는 기존의 뒤의 노드를 가리키게 한다.
* 이러한 연산 때문에 배열리스트에 비해 매우 유연하게 연산을 수행할 수 있다.


3. 삭제 : O(n)
  * 삭제를 원하는 노드의 앞까지 접근해야 하기 때문에 O(n)의 시간복잡도를 가진다.
    * 만약 LinkedList의 맨 앞과 맨 뒤에 삭제연산을 진행하면 O(1)의 시간복잡도를 가질 것.


![image](https://user-images.githubusercontent.com/36829127/171398382-4317e5e6-bcbc-42c7-bc0d-43789d5265bf.png)

* 그림과 같이 삭제하기를 원하는 노드의 앞 뒤 노드를 포인터로 연결해주고 삭제하기 원하는 노드를 메모리에서 해제해주면 된다.

## Java에서의 LinkedList

### LinkedList 선언
```java
LinkedList list = new LinkedList();//타입 미설정 Object로 선언된다.
LinkedList<Student> members = new LinkedList<Student>();//타입설정 Student객체만 사용가능
LinkedList<Integer> num = new LinkedList<Integer>();//타입설정 int타입만 사용가능
LinkedList<Integer> num2 = new LinkedList<>();//new에서 타입 파라미터 생략가능
LinkedList<Integer> list2 = new LinkedList<Integer>(Arrays.asList(1,2));//생성시 값추가
```

### LinkedList 삽입
```java
LinkedList<Integer> linkedList = new LinkedList<>();

// 가장 앞에 데이터 추가
linkedList.addFirst(data);

// 가장 뒤에 데이터 추가
linkedList.addLast(data);

// 가장 뒤에 데이터 추가
linkedList.add(data);

// index 뒤에 데이터 추가
linkedList.add(index,data);

// index의 데이터를 변경
linkedList.set(index, changeData);

```


### LinkedList 삭제

```java
//Array.asList 사용 Linked List 생성
LinkedList<Integer> linkedList = new LinkedList<Integer>(Arrays.asList(1,2,3,4,5));

//가장 앞의 데이터 제거
linkedList.removeFirst(); 

//가장 뒤의 데이터 제거
linkedList.removeLast(); 

//index 생략시 0번째 index제거
linkedList.remove(); 

//index의 data 제거
linkedList.remove(index);

//List안의 모든 data 제거
linkedList.clear();
```


### LinkedList 검색

```java
//Array.asList 사용 Linked List 생성
LinkedList<Integer> linkedList = new LinkedList<Integer>(Arrays.asList(1,2,3,4,5));

//list에 data가 있는지 검색 있다면 true를 return
linkedList.contains(data); 

//list에 data가 있는 index 반환, 없으면 -1을 return
linkedList.indexOf(data);
```

### 그 외
```java

//Array.asList 사용 Linked List 생성
LinkedList<Integer> linkedList = new LinkedList<Integer>(Arrays.asList(1,2,3,4,5));


//현 리스트의 크기를 반환
int sizeOfList = linkedList.size();

//현재 리스트를 배열로 변환 후 반환
Object[] a = linkedList.toArray();
```



<출처>

https://velog.io/@gillog/%EC%97%B0%EA%B2%B0-%EB%A6%AC%EC%8A%A4%ED%8A%B8Linked-List

https://gusdnd852.tistory.com/100

https://coding-factory.tistory.com/552
