## **String  vs  StringBuffer/StringBuilder**

- 차이점 : String은 불변성(immutable)을 가지고, StringBuffer/StringBuilder은 가변성(mutable)을 가진다.
- **String 객체** : 한번 값이 할당되면 그 공간은 변하지 않음
- **StringBuffer/StringBuilder 객체 :** 한번 값이 할당되더라도 또 다른 값이 할당되면 할당된 공간이 변함
- 따라서, 문자열 변경이 자주 있을 경우에는 **StringBuffer/StringBuilder을 사용**

## **String constant pool**

- **String 값을 할당하는 방식**
    1. new 키워드 `String str = new String("hello");`
    2. 리터럴 변수를 대입 `String str = "hello";`

```java
// 1) new 키워드
String a = new String("hello");
String b = new String("hello");
System.out.println(a==b); // false

// 2) 리터럴 변수 대입
String A = "hello";
String B = "hello";
System.out.println(A==B); // true
```

1. new 키워드 `String str = new String("hello");`
    - **Heap 영역에 동적으로 메모리 공간이 할당됨**
    - 새롭게 new를 사용할 경우, 같은 문자열이더라도 Heap 영역 안에 새롭게 저장함.
2. 리터럴 변수를 대입 `String str = "hello";`
    - Heap 메모리 영역안의 **String constant pool** 에 저장
    - 만약 String constant pool에 이미 존재하는 리터럴 값이라면, 새로 저장하지 않고 이미 존재하는 값을  사용함

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
