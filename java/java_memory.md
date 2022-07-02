# Java의 메모리 구조 

## JVM이 실행되는 과정

![image](https://user-images.githubusercontent.com/36829127/176988699-095a3e39-6bf8-46df-9988-d6f8be898747.png)

1. 자바 소스코드인 .java 파일을 컴파일러가 자바 바이트 코드인 .class로 변환합니다.

2. .class 코드를 JVM의 클래스 로더에게 보냅니다.

3. 클래스 로더는 JVM Runtime Data Area으로 로딩하여 JVM의 메모리에 올립니다.

4. Runtime Data Area에 로딩된 .class들은 Execution Engine을 통해 해석합니다.

5. 해석된 바이트 코드는 Runtime Data Area의 각 영역에 배치되어 수행하며 이 과정에서 Execution Engine에 의해 GC의 작동과 스레드 동기화가 이루어집니다.

## 런타임 데이터 영역

런타임 데이터 영역은 JVM의 메모리 영역으로 자바 애플리케이션을 실행할 때 사용되는 데이터를 적재하는 영역이다.

* **모든 스레드가 공유해서 사용(GC의 대상)**
  * 힙 영역(Heap Area)
  * 메서드 영역(Method 영역)

* **스레드마다 하나씩 생성**
  * 스택 영역(Stack Area)
  * PC 레지스터(PC Register)
  * 네이티브 메서드 스택(Native Method Stack)

### 메드 영역(Method 영역)

* JVM이 실행되면서 생기는 공간.
* Class 정보, 전역변수 정보, Static 변수 정보가 저장되는 공간
* Runtime Constant Poll에는 말 상수 정보가 저장된다.
* 모든 스레드에서 정보가 공유된다.

### 힙 영역 (Heap Area)
![image](https://user-images.githubusercontent.com/36829127/176988986-825a17b1-da4e-4bcb-a637-278e79fc7027.png)

* **new** 키워드로 생성된 객체와 배열이 생성되는 영역
* 주기적으로 GC가 제거하는 영역


힙 영역은 더 자세하게는 GC를 위해 크게 3가지 영역으로 나뉜다.

Young Generation은 자바 객체가 생성되자마자 저장되고, 생긴지 얼마 안된 객체가 저장되는 공간. 
Heap 영역에 객체가 생성되면 최초로 Eden 영역에 할당된다. 
이 영역에서 데이터가 쌓이게 되면 Survivor 0 또는 1로 이동하게 되거나 minor GC에 의해 회수된다.


Young generation에서 일정 Age bit을 넘은 객체들은 Old generation으로 이동되거나 회수된다. 
Old generation에서도 메모리가 가득차게 되면 major GC를 실행하여 메모리를 회수한다.


### 스택 영역 (Stack Area)
지역변수, 파라미터, 리턴 값, 연산에 사용되는 임시 값, 메서드 호출 등 임시적인 데이터들이 생성되는 영역이며 Primitive Type 자료형을 생성할 때 저장하는 공간이다.
정적 메모리 할당이 이루어지는 장소로 Heap 영역에 동적 할당된 값에 대한 참조를 얻을 수 있다.

ex) Person p = new Person(); 에서 Person p는 Stack 영역, new Person();은 Heap 영역에 할당된다.


### PC 레지스터 (PC Register)
쓰레드가 생성될 때마다 생성되는 영역으로 **쓰레드마다 하나씩** 존재한다. 
현재 쓰레드가 실행되는 부분의 주소와 명령을 저장하고 있는 영역이다. 
이것을 이용하여 쓰레드를 돌아가면서 수행할 수 있도록 한다.


### 네이티브 메서드 스택 (Native Method Stack)
자바 의외의 언어(C, C++, 어셈블리 등)로 작성된 네이티브 코드를 실행할 때 사용되는 메모리 영역으로 일반적인 C 스택을 사용

일반적인 메소드를 실행하는 경우 JVM 스택에 쌓이다가 해당 메소드 내부에 네이티브 방식을 사용하는 메소드(예를 들면 C언어로 작성된 메소드)가 있다면 해당 메소드는 네이티브 스택에 쌓인다.


## 클래스 로딩(Class loading)

* Java에서 클래스가 로딩되는 과정은 클래스 로더가 확장자가 .class 클래스 파일의 위치를 찾아 그것을 JVM위에 올려놓는 과정을 말한다.
* 자바는 클래스 파일을 **동적으로 읽어온다.** JVM이 동작하다가 클래스 파일을 참조하는 순간 동적으로 읽어서 메모리에 로드되면서 JVM에 링크된다. 자바 클래스로더는 클래스를 동적으로 메모리에 로딩하는 역할을 담당한다.


### 클래스 로더란
![image](https://user-images.githubusercontent.com/36829127/176989666-60c2f90c-e3e7-4520-abe3-83ff8fd991a5.png)

클래스 로더에는 로딩, 링크, 초기화 단계로 나뉘어져 있다.

* 로딩
 * 자바 바이트 코드(.class)를 메소드 영역에 저장한다.
 * 각 자바 바이트 코드(.class)는 JVM에 의해 메소드 영역에 다음 정보들을 저장한다.
  1. 로드된 클래스를 비롯한 그의 부모 클래스 정보
  2. 클래스 파일과 Class, Interface, Enum의 관련 여부
  3. 변수나 메소드 등의 정보
 
* 링크
 * 검증 : 읽어들인 클래스가 자바 언어의 명세 및 JVM 명세에 명시된 대로 잘 구성된지 확인한다.
 * 준비 : 클래스가 필요로하는 메모리를 할당하고, 클래스에서 정의된 필드, 메소드, 인터페이스를 나타내는 데이터 구조를 준비한다.
 * 분석 : 심볼릭 메모리 레퍼런스를 메소드 영역에 있는 실제 레퍼런스로 교체한다.

* 초기화
 * 클래스 변수들을 적절한 값으로 초기화 한다. 즉, static 필드들이 설정된 값으로 초기화한다.


### 클래스 로더의 종류
![image](https://user-images.githubusercontent.com/36829127/176989826-b0276ab8-df6c-4942-b834-9aa7f4b9869c.png)

클래스 로더는 계층적으로 구성되어 있다.
> 계층적 구성의 이유
> 
> * 모듈화가 가능
> * 충돌을 피할 수 있음
> * 효율적인 사용 가능

#### 부트스트랩 클래스 로더(Bootstrap Class Loader)
JVM 시작 시 가장 최초로 실행되는 클래스 로더이다. 부트스트랩 클래스 로더는 자바 클래스를 로드하는 것이 아닌, 
자바 클래스를 로드할 수 있는 자바 자체의 클래스 로더와 최소한의 자바 클래스(java.lang.Object, Class, ClassLoader)만 로드한다.


**Java 8**
* e/lib/rt.jar 및 기타 핵심 라이브러리와 같은 JDK의 내부 클래스를 로드한다.

**Java 9 이후**
* 더 이상 /re.jar이 존재하지 않으며, /lib 내에 모듈화되어 포함됐다. 이제는 정확하게 ClassLoader 내 최상위 클래스들만 로드한다.

#### 확장 클래스 로더(Extension Class Loader)
확장 클래스 로더는 부트스트랩 로더를 부모로 갖는 클래스 로더로서, 확장 자바 클래스들을 로드한다. 
java.ext.dirs 환경 변수에 설정된 디렉토리의 클래스 파일들을 로드하고, 이 값이 설정되어 있지 않은 경우 ${JAVA_HOME}/jre/lib/ext에 있는 클래스 파일을 로드한다.


**Java 8**
* URLClassLoader를 상속하며, jre/lib/ext 내 모든 클래스를 로드한다.


**Java 9 이후**
* Platform Loader로 변경되었으며, URLClassLoader가 아닌 BuiltinClassLoader를 상속한다. Inner Static 클래스로 구현되어 있다.


#### 시스템 클래스 로더
자바 프로그램 시 지정한 ClassPath에 있는 클래스 파일 또는 jar에 속한 클래스들을 로드한다. 쉽게 말하자면 우리가 만든 class 확장자 파일을 로드한다.


### 클래스 로드 과정
![image](https://user-images.githubusercontent.com/36829127/176990039-c5bfe095-790f-463a-a61f-0b35d0eb5c15.png)
System Loader가 클래스를 로딩할 때 그 로딩 요청은 부모 로더들로 거슬러 올라가서 부트스트랩 로더에 다다른 뒤 그 밑으로 요청을 수행하는 방식을 취한다.

1. JVM의 메소드 영역에 클래스가 로드되어 있는 지 확인한다. 만일 로드되어 있는 경우 해당 클래스를 사용한다.
2. 메소드 영역에 클래스가 로드되어 있지 않는 경구, 시스템 클래스 로더에 클래스 로드를 요청한다.
3. 시스템 클래스 로더는 확장 클래스 로더에 요청을 **위임**한다.
4. 확장 클래스 로더는 부트스트랩 클래스 로더에 요청을 **위임**한다.
5. 부트스트랩 클래스 로더는 부트스트랩 ClassPath(JDK/JRE/LIB)에 해당 클래스가 있는지 확인한다. 클래스가 존재하지 않는 경우 확장 클래스 로더에 **요청을 넘긴다.**
6. 확장 클래스 로더는 확장 ClassPath(JDK/JRE/LIB/EXT)에 해당 클래스가 있는지 확인한다. 클래스가 존재하지 않는 경우 시스템 클래스 로더에 **요청을 넘긴다.**
7. 시스템 로더는 시스템 ClassPath에 해당 클래스가 있는지 확인한다. 클래스가 존재하지 않는 경우 **ClassNotFoundException**을 발생시킨다.

### 클래스 로더의 원칙
1. 위임(Delegation)
 - 클래스 로딩 작업을 상위 클래스에 위임
2. 가시성(Visibility)
 - 하위 클래스로더는 상위 클래스로더가 로드한 클래스의 내용을 볼 수 있지만 그 반대는 불가능하다.
3. 유일성(Uniqueness)
 - 하위 클래스로더는 상위 클래스로더가 로딩한 클래스를 다시 로딩하지 않게 해서 유일성을 보장한다.


### 동적 클래스 로딩
자바의 클래스 로딩은 클래스 참조 시점에 JVM에 코드가 링크되고, 실제 런타임 시점에 로딩되는 동적 로딩을 거친다. 
런타임에 동적으로 클래스를 로딩한다는 것은 JVM이 미리 모든 클래스에 대한 정보를 메소드 영역에 로딩하지 않는 다는 것을 의미한다.
동적 클래스 로딩 방식은 두가지가 있다.

#### 1. 로드 타임 동적 로딩(Load-time Dynamic Loading)
```java
public class HelloWorld { 
        public static void main(String[] args) { 
                System.out.println("안녕하세요!"); 
        } 
}
```

위 코드는 다음과 같이 동작한다.
1. JVM이 시작되고 부트스트랩 클래스 로더가 생성된 후에 모든 클래스가 상속받고 있는 Object 클래스를 읽어온다.
2. 클래스 로더는 명령 행에서 지정한 HelloWorld 클래스를 로딩하기 위해, HelloWorld.class 파일을 읽는다.
3. HelloWorld 클래스를 로딩하는 과정에서 필요한 java.lang.String과 java.lange.System을 로딩한다.

이처럼 하나의 클래스를 로딩하는 과정에서 동적으로 다른 클래스도 로딩하는 것을 **로드 타임 동적 로딩**이라고 한다.


#### 2. 런타임 동적 로딩(Run-time Dynamic Loading)
```java
public class RuntimeLoading { 
        public static void main(String[] args) { 
                try { 
                        Class cls = Class.forName(args[0]); 
                        Object obj = cls.newInstance(); 
                        Runnable r = (Runnable) obj; 
                        r.run(); 
                } catch (Exception e) {
                        e.printStackTrace();
                }
        }
}
```

위 코드에서 Class.forNmae(className)은 파라미터로 받은 className에 해당하는 클래스를 로딩한 후에 객체를 반환하며, 다음과 같이 동작
* Class.forName() 메소드가 실행되기 전까지는 RuntimeLoading 클래스에서 어떤 클래스를 참조하는지 알 수 없다.
* 따라서 RuntimeLoading 클래스를 로딩할 때는 어떤 클래스도 읽어오지 않고, RuntimeLoading 클래스의 main() 메소드가 실행되고 Class.forName(args[0]) 를 호출하는 순간에 비로소 args[0] 에 해당하는 클래스를 로딩한다.

이처럼 클래스를 로딩할때가 아닌 코드를 실행하는 순간에 클래스를 로딩하는 것을 **런타임 동적 로딩**이라고 한다.


**참조**

https://velog.io/@shin_stealer/%EC%9E%90%EB%B0%94%EC%9D%98-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B5%AC%EC%A1%B0

https://engkimbs.tistory.com/606

https://steady-coding.tistory.com/593
