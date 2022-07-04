## 동등 비교 연산자(==)****와 equals() 메소드****

- == 연산자 : 대상의 주소값을 비교
- equals 메소드 : 대상의 실제 값을 비교

```java
String s1 = "HELLO";
String s2 = "HELLO"; // String constant pool에 의해 s1이 생성한 "HELLO"를 참조함
String s3 = s1;
String str =  new String("HELLO");

// == : 주소값 비교
System.out.println(s1 == s2); // true
System.out.println(s1 == s3); // true
System.out.println(s1 == str); // false

// equals() : 값 비교
System.out.println(s1.equals(s2)); // true
System.out.println(s1.equals(s3)); // true
System.out.println(s1.equals(str)); // true
```

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


![image](https://user-images.githubusercontent.com/77563814/177133984-b694a441-1198-40a5-90ca-177a348b2f65.png)


1. new 키워드 `String str = new String("hello");`
    - **Heap 영역에 동적으로 메모리 공간이 할당됨**
    - 새롭게 new를 사용할 경우, 같은 문자열이더라도 Heap 영역 안에 새롭게 저장함.
2. 리터럴 변수를 대입 `String str = "hello";`
    - Heap 메모리 영역안의 **String constant pool** 에 저장
    - 만약 String constant pool에 이미 존재하는 리터럴 값이라면, 새로운 메모리에 할당하지 않고         이미 존재하는 값을  사용함 → **String Interning이라 함**
    - **String Interning 과정**
        1. String의 값이 String Constants Pool에 있는지 찾음
        2. 있다면 바로 반환
        3. 없다면 StringConstants Pool에 해당 값을 할당
        4. 반환

- String은 "Happy Java"와 같이 리터럴로 표기가 가능하지만 primitive 자료형은 아니다. String은 리터럴 표현식을 사용할 수 있도록 자바에서 특별 대우해주는 자료형이다.
    → [https://wikidocs.net/205](https://wikidocs.net/205)

**Reference**

[https://www.geeksforgeeks.org/difference-between-and-equals-method-in-java/?ref=gcse](https://www.geeksforgeeks.org/difference-between-and-equals-method-in-java/?ref=gcse)

[https://sabarada.tistory.com/137](https://sabarada.tistory.com/137)
