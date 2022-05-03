## 동기화 객체의 종류

**동기화**란, 공유 자원(임계 영역)에 여러 프로세스가 접근할 경우 데이터의 일관성을 해치는 등 문제가 발생할 수 있기에 이를 방지하는 기법

## **스레드 동기화 방법**

1. **실행 순서**의 동기화
    - 스레드의 **실행 순서**를 정의하고, 이 순서에 반드시 따르도록 하는 것
2. **메모리 접근**에 대한 동기화
    - 메모리 접근에 있어서 동시접근을 막는 것

## **동기화 기법 종류**

1. **유저 모드 동기화**
    - 커널 코드가 실행되지 않는 동기화 기법
    - 커널 모드로의 전환이 없기에 성능상 이점이 있으나 기능상의 제한이 있음
        - 크리티컬 섹션 (메모리 접근 동기화)
        - 인터락 함수 (메모리 접근 동기화)
2. **커널 모드 동기화**
    - 커널에서 제공하는 동기화 기능을 활용하는 방법
    - 커널 모드로의 변경이 필요하고 이는 성능 저하로 이어짐, 대신 다양한 기능 활용 가능
        - 뮤텍스 (메모리 접근 동기화)
        - 세마포어 (메모리 접근 동기화)
        - 이벤트 (실행순서 동기화)

---

## 운영체제의 이중모드

한 유저 프로그램에서 다른 유저 프로그램에 **접근**하게 되면 문제가 생길 수 있다. (다른 유저 프로그램을 중단한다거나, 다른 유저 프로그램의 정보를 보게 된다거나) 그래서 이를 방지하기 위해서 운영체제는 두 가지 모드를 가진다.

1. **유저 모드** : 애플리케이션 실행 중 일때의 모드
2. **커널 모드** : 운영체제가 실행될 때의 모드, 하드웨어 접근에 제약 없음

## 동기화 객체 종류

동기화 객체 : 동기화를 수행하는 객체
(ex. 커널 오브젝트 : 커널 모드에서 동기화를 수행하는 객체)

1. [Critical Section](#1.-critical-section)
2. [Mutex](#2.-mutex)
3. [Interlock](#3.-인터락-함수)
4. [Event](#4.-event)
5. [Semaphore](#5.-semaphore)
6. [Monitor](#6.-모니터)


## 1. Critical Section
- 유저 모드, 메모리 접근 동기화
- 한 프로세스 내의 스레드 사이에서의 동기화

```cpp
#include <thread>
#include <windows.h>
using namespace std;
 
CRITICAL_SECTION cs; // 크리티컬 섹션 동기화 기법
 
int cnt1;
void func() {
    EnterCriticalSection(&cs);
    for (int i = 1; i <= 1000000; i++)
        cnt1++;
    LeaveCriticalSection(&cs);
}
 
int main() {
    InitializeCriticalSection(&cs);
    for (int i = 0; i < 10; i++) {
        cnt1 = 0;
        thread t1(func);
        thread t2(func);
    }
    return 0;
}
// 출처: https://www.crocus.co.kr/1363
```

### 2. Mutex
- 커널 모드, 메모리 접근 동기화
- 한 프로세스 내의 스레드 사이에서의 동기화
- 범위가 프로세스로 확장되어 **여러 프로세스**에서 공유자원에 대한 동기화 
(앞선 크리티컬 섹션 동기화와의 차이점)
- 커널 객체로, **프로세스 다중 실행을 방지**한다.
- 접근 가능한 스레드는 하나

```cpp
using System.Threading;
class Program
{
    private static void Main(string[] args)
    {
        TestClass testClass = new TestClass();
        Thread thread1 = new Thread(testClass.Calculte);
        Thread thread2 = new Thread(testClass.Calculte);
        thread1.Start();
        thread2.Start();
    }

    private class TestClass
    {
        Mutex mutex = new Mutex(false, "Calculte");

        public void Calculte()
        {
            mutex.WaitOne();
            Thread.Sleep(5000); // 연산이 처리되는 부분이라 가정
            mutex.ReleaseMutex();
        }
    }
}
```

### 3. 인터락 함수
- 유저 모드, 메모리 접근
- 임계영역이 변수 하나일 경우, 변수 하나의 접근방식만 동기화하는 간단한 방식
- 인터락 함수들을 사용하여 동기화함

```cpp
// gCount가 임계영역일때

gCount ++; // 스레드 세이프하지 않음
InterlockedIncrement(&gCount); // 인터락 동기화 방식

// 출처 : https://igotit.tistory.com/entry/Thread-Safety-%EC%8A%A4%EB%A0%88%EB%93%9C-%EC%95%88%EC%A0%84-Interlocked-%ED%95%A8%EC%88%98%EB%93%A4
```

### 4. Event
- 커널 모드, 실행 순서 동기화
- 접근 제어 보다는 **특정 상황이 발생될 때 이를 알리는 목적**으로 사용
- ex) 임계영역에 대한 작업이 끝났음을 다른 스레드에게 알림
- 이벤트 동기화 방법
    
    1. 이벤트 객체를 Non-Signal 상태로 생성
    
    2. 하나의 스레드가 초기화 작업을 진행하고, 
    나머지 스레드는 이벤트 객체에 대해 Lock()을 호출함으로써 이벤트 객체가 신호 상태가 되기를 기다린다. (Sleep 상태)
    
    3. 작업 중인 스레드가 초기화 작업을 완료하면, 이벤트 객체를 Signal 상태로 바꾼다.
    
    4. 기다리고 있던 모든 스레드가 깨어나서 작업을 진행한다.
    

### 5. Semaphore
- 커널 모드, 메모리 접근 동기화
- 하나의 자원에 접근할 수 있는 프로세스의 수를 지정할 수 있다
- 예) 동접자 제한 등

```cpp
using System.Threading;

class Program
{
    private static void Main(string[] args)
    {
        TestClass testClass = new TestClass();
        for (int i = 0; i < 5; i++)
        {
            Thread thread = new Thread(testClass.Calculte);
            thread.Name = $"Thread ({i})";
            thread.Start();
        }
    }

    private class TestClass
    {
        //최대 실행 가능한 쓰레드 5개
        Semaphore semaphore = new Semaphore(2, 5);
        public void Calculte()
        {
            semaphore.WaitOne();
            Thread.Sleep(5000); // 연산이 처리되는 부분이라 가정
            semaphore.Release();
        }
    }
}
```

### 6. 모니터
- 프로그래밍 언어 수준에서 사용할 수 있는 고수준 동기화 도구 
(라이브러리나 프레임워크에서 지원)
- 프로그래머가 복잡한 동기화를 직접 관리하지 않음
- **자바 모니터**
    - 배타동기: `synchronization` 키워드
    - 조건동기: `wait(), notify(), notifyAll()` 메소드 (최상위 객체인 **Object가 제공하는 메소드,** 이 메서드들은 `synchronized` 블록 내에서 사용함**)**

![image](https://user-images.githubusercontent.com/77563814/166426222-a7d87c72-6437-4650-a28c-62c98e90b5dd.png)

```java
class Chopsticks {

  private boolean inUse = false

  synchronized void acquire() { // 젓가락을 잡기
    while (inUse) 
      wait(); // 조건 동기 큐 안에 넣기
    inUse = true;
  }
  synchronized void release() { // 젓가락을 놓는것
    inUse = false;
    notify(); // 조건 동기 큐에서 깨우기
  }
}
```
