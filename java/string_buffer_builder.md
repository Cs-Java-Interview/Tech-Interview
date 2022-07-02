## **String  vs  StringBuffer/StringBuilder**

- 차이점 : String은 불변성(immutable)을 가지고, StringBuffer/StringBuilder은 가변성(mutable)을 가진다.
- **String 객체** : 한번 값이 할당되면 그 공간은 변하지 않음
- **StringBuffer/StringBuilder 객체 :** 한번 값이 할당되더라도 또 다른 값이 할당되면 할당된 공간이 변함
- 따라서, 문자열 변경이 자주 있을 경우에는 **StringBuffer/StringBuilder을 사용**


## **StringBuffer  vs  StringBuilder**

- 차이점 : **동기화의 유무 (**`synchronized` 키워드 지원 여부)
- **StringBuffer**는 동기화를 지원함
    - **멀티쓰레드 환경에서 안전함(thread-safe)**
- **StringBuilder**는 **동기화를 지원하지 않음**
    - 멀티쓰레드 환경에서 사용하는 것은 적합하지 않지만 **단일쓰레드에서의 성능은 StringBuffer 보다 뛰어남**

### 각각 사용해야 하는 경우

- **String** :  문자열 변경이 적고 멀티쓰레드 환경일 경우
- **StringBuffer** : 문자열 연산(추가, 수정, 삭제)이 많고 멀티쓰레드 환경일 경우
- **StringBuilder** : 문자열 연산(추가, 수정, 삭제)이 많고 단일 쓰레드이거나 동기화를 고려하지 않아도 되는 경우
