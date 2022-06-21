### 1. **기본 타입** (Primitive Data Type)
- 실제 값을 저장
- 종류 : byte, short, char, int, float, double, boolean이 있다.
- 저장 위치 :
    - 기본형의 경우 크기가 작고 고정적이기에
    - 메모리영역의 스택영역에 실제 값들이 저장된다.
- 해제 : 변수가 사용된 메서드가 종료되거나 사용된 객체가 사라질 때 자동으로 파괴된다.

### 2. **참조 타입**(Reference Data Type)
- 객체의 주소를 저장
- 종류 : class, array, interface, Enumeration이 있다.
    - 기본형을 제외하고는 모두 참조형이다.
    - new 키워드를 이용하여 객체를 생성하여 데이터가 생성된 주소를 참조하는 타입이다.
- 저장 위치 :
    - 참조형의 경우 데이터의 크기가 가변적, 동적이기에
    - 실제 인스턴스는 힙영역에 생성되고, 
    그 주소값이 스택영역에 저장된다.
- 해제 : 더 이상 참조하는 변수가 없을 때 가비지 컬렉션에 의해 파괴된다.

**참고) JAVA는 기본형, 참조형 모두 Call-By-Value이다!**

- Call-By-Value : 값을 넘겨주는 호출 방식
- Call-By-Reference : 참조에 의한 호출 방식 (전달받은 값을 직접 참조)
    - 전달받은 값을 변경할 경우 원본도 같이 변경된다
- 자바에서는 주소값을 넘겨주는 방식인데,
    - 직접적인 참조를 넘긴 게 아닌, **객체를 보는 주소 값을 복사해서 넘긴다.**
    - 따라서 **Call-By-Value이고,**
    - 필요시 복사된 주소 값으로 참조가 가능하여, **주소 값이 가리키는 객체의 내용** 변경할 수 있다. (아래 3번 코드 참고)

```java
// 1. 기본 자료형 swap할 때
public class Test {

    public static void swap(int x, int y) {
        int tmp = x;
        x = y;
        y = tmp;
        }

    public static void main(String[] args) {
        int a = 111;
        int b = 222;
        swap(a, b);
        System.out.println(a); // 111
	System.out.println(b); // 222
    }
}

// 2. 참조 자료형 swap할 때
public class Test {

    public static void swap(Integer x, Integer y) {
        Integer temp = x;
        x = y;
        y = temp;
        }

    public static void main(String[] args) {
        Integer a = new Integer( 111 );
        Integer b = new Integer( 222 );

        swap(a, b); // 주소값만 복사되어서 swap 함수 밖에서는 적용X
        System.out.println(a); // 111
	System.out.println(b); // 222
    }
}

// 3. Call by Reference처럼 사용하기
public class Test {
    int value;

    public Test(int value) {
        this.value = value;
    }

    public static void swap(Test x, Test y) {
        int temp = x.value;
        x.value = y.value;
        y.value = temp;
        }

    public static void main(String[] args) {
        Test a = new Test(111);
        Test b = new Test(222);

        swap(a, b); // swap 함수 안에서 참조해서 데이터 변경할 경우, 외부에서도 반영됨
        System.out.println(a); // 222
	System.out.println(b); // 111
    }
}
```

- 비교용 C++ 코드

```java
// 1. call by value 
void Swap(int x, int y)
{
	int tmp = x;
	x = y;
	y = tmp;
}
 
int main()
{
	int a = 111;
	int b = 222;

	Swap(a, b); // swap함수 외부에서는 변경이 반영X
	cout << "a : " << a << endl; // 111
	cout << "b : " << b << endl; // 222
}

// 2. call by reference
void Swap(int *a, int *b)
{
	int tmp = *a;
	*a = *b;
	*b = tmp;
}
 
int main()
{
	int a = 111;
	int b = 222;
 
	Swap(&a, &b); // swap함수 외부에서도 변경이 반영된다
	cout << "a : " << a << endl; // 222
	cout << "b : " << b << endl; // 111
}
```

### 3. **래퍼 클래스**
- 기본 타입을 참조 타입으로 바꿔주는 클래스를 래퍼클래스(wrapper) 클래스라고 한다.
- 사용 이유?
  - 기본 타입을 참조 타입으로 바꾸면 데이터를 가공하는 있는 메서드를 사용할 수 있다.
  - 예를 들어 int를 랩퍼 클래스 Integer로 바꾸면 int 값과 편리한 여러 메서드를 같이 쓸 수 있다.

**Reference**

[https://catsbi.oopy.io/6541026f-1e19-4117-8fef-aea145e4fc1b](https://catsbi.oopy.io/6541026f-1e19-4117-8fef-aea145e4fc1b)

[https://sdesigner.tistory.com/m/93](https://sdesigner.tistory.com/m/93)

[https://velog.io/@ahnick/Java-Call-by-Value-Call-by-Reference](https://velog.io/@ahnick/Java-Call-by-Value-Call-by-Reference)
