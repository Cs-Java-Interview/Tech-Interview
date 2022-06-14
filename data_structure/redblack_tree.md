### ****RedBlack Tree가**** 생긴 이유?

- 이진탐색트리를 사용하는 이유는 탐색 속도가 O(logN)으로 빠르기 때문이다.
- 그러나 편향 트리가 될 경우, 탐색 시간이 선형시간 O(n)이 걸린다.
- 즉, 편향 트리가 안되도록 균형을 맞추기 위해서 ****RedBlack Tree****을 사용한다.
    - 편향 트리 : 왼쪽 혹은 오른쪽 서브트리만을 가지는 이진트리
        
        ![image](https://user-images.githubusercontent.com/77563814/173553329-d3c602fa-c538-4761-a745-2db84fa07eb0.png)

        

### ****RedBlack Tree란****

- 각 노드에 색깔을 저장하는 공간을 추가하여 **색깔을 기준으로 균형**을 맞추는 이진 탐색 트리
- 규칙
    - 모든 노드는 RED이거나 BLACK이다.
    - 루트 노드는 BLACK이다.
    - 모든 리프노드(NIL)는 BLACK이다.
    - RED 노드의 자식은 BLACK이다.
    ****== No Double Red (빨간색 노드가 연속으로 나올 수 없다)
    - 모든 리프 노드에서 Black Depth는 같다.
    == 리프노드에서 루트 노드까지 가는 경로에서 만나는 검은색 노드의 개수가 같다.

### ****RedBlack Tree 삽입 과정****

1. **새로운 노드**는 항상 **빨간색**으로 삽입한다.
    
    ![image](https://user-images.githubusercontent.com/77563814/173553249-40660c85-c898-4de1-9098-1d0ae967abb0.png)
    
2. 이때, 빨간색 노드가 연속으로 2번 나타날 수 있다 (**Double Red**) ⇒ 규칙 위반!
    
    2-1) 부모의 형제노드가 검은색이라면 -> Restructuring을 수행하면 된다.
    
    2-2) 부모의 형제노드가 빨간색이라면 -> Recoloring을 수행하면 된다.
    
    - ![image](https://user-images.githubusercontent.com/77563814/173553389-e1ba8294-08a7-405b-97f3-0f5077827793.png)
    

### ****Restructuring (부모의 형제 노드가 BLACK일 경우)****

![image](https://user-images.githubusercontent.com/77563814/173553431-ced32474-aeee-494b-a8c2-9cc7971da1cc.png)

1. 새로운 노드(N), 부모 노드(P), 조상 노드(G)를 오름차순으로 정렬한다.
2. 셋 중 중간값을 부모로 만들고 나머지 둘을 자식으로 만든다.
3. 새로 부모가 된 노드를 검은색으로 만들고 나머지 자식들을 빨간색으로 만든다.
- 이때, 값이 10인 노드는 자식 노드 NIL 2개를 가지고 있고 그 NIL들이 검은색이라고 본다. (규칙 : 모든 리프노드(NIL)는 BLACK)

### ****Recoloring (부모의 형제 노드가 RED일 경우)****

![image](https://user-images.githubusercontent.com/77563814/173553468-b01dc26e-d826-46eb-abb1-bdd20b08514e.png)

1. 새로운 노드(N)의 부모(P)와 삼촌(U)을 검은색으로 바꾸고 조상(G)을 빨간색으로 바꾼다.
    1. 이때, 조상(G)이 루트 노드라면 검은색으로 바꾼다. (규칙 : 루트 노드는 검은색)
2. 조상(G)을 빨간색으로 바꿨을 때 또 다시 Double Red가 발생한다면 또다시 Restructuring 혹은 Recoloring을 진행해서 Double Red 문제가 발생하지 않을 때까지 반복한다.
