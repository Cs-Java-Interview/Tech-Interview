# Stack

- 선형 자료구조
- `Last In First Out (LIFO)` 즉, 나중에 들어간 원소가 먼저 나온다.

차곡차곡 쌓이는 구조로 먼저 Stack 에 들어가게 된 원소는 맨 바닥에 깔리게 된다. 

때문에 늦게 들어간 원소들은 그 위에 쌓이게 되고 호출 시 가장 위에 있는 녀석이 나온다.

### 스택(Stack)의 연산

- **pop()**: 스택에서 가장 위에 있는 항목을 제거한다.
- **push(item)**: item 하나를 스택의 가장 윗 부분에 추가한다.
- peek(): 스택의 가장 위에 있는 항목을 반환한다.
- isEmpty(): 스택이 비어 있을 때에 true를 반환한다.

### 스택(Stack)의 구현

스택은 **배열 or 연결리스트**로 구현할 수 있다. 

1. 배열로 구현한 스택

```java
public class ArrayStack {
    int top;    //인덱스
    int size;    //스택 배열의 크기
    int [] stack; // 스택
    public ArrayStack(int size) {
        this.size = size;
        stack = new int[size];
        top = -1;
    }

    public void push(int item) {
        stack[++top] = item;
    }

    public void pop() {
        int pop = stack[top];
        stack[top--] = 0;
    }

    public void peek() {
        System.out.println(stack[top]);
    }
}
```

2. 링크드리스트로 구현한 경우
- 스택의 크기가 고정되어있지 않음
- Node로 구성(데이터 + 다음 데이터를 가리키는 주소)

```java
public class Node {
    private int item; // 데이터
    private Node link; // 다음 데이터 가리키는 링크

    public Node(int item) {
        this.item = item;
        this.link = null;
    }

    protected void next(Node node) { // 다음 데이터 가리키기
        this.node = node;
    }
    protected int getData() {
        return this.item;
    }
}

// 연결리스트 관리 스택
public class NodeManager {
    Node top; // 가장 최근에 들어온 노드를 가리킴

    public NodeManager() {
        this.top = null;
    }

    public void push(int data) {
        Node node = new Node(data);
				top.next(node);    // 원래 top가 새노드를 가리키게 함
        top = node;            // top에 새 노드를 연결
    }

    public void pop() {
        top = top.getNextNode(); // 현재 top이 가리키고 있는 노드를 가리키게 함
    }

    public int peek() {
        return top.getData();
    }
}
```

3. JAVA의 스택 클래스

```java
Stack<Element> stack = new Stack<>();

public Element push(Element item);
public Element pop(); // 최근에 추가된(Top) 데이터 삭제
public Element peek(); // 최근에 추가된(Top) 데이터 조회
public boolean empty(); // stack의 값이 비었으면 true, 아니면 false
public int seach(Object o); // 인자값으로 받은 데이터의 위치 반환
```

### 스택을 사용하는 경우

- 재귀 알고리즘
- 웹 브라우저 방문기록 (뒤로가기)
- 실행 취소 (undo)
- 역순 문자열 만들기
- 후위 표기법 (연산자(+,-)가 피연산자들 뒤에 위치)
ex) A + B * C (중위)  VS  ABC * + (후위)
    - 예시)  A B C * +
    - A *B C ** +   ⇒   B  * C
    - *A D +*        ⇒   A + D
- DFS
