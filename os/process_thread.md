## 프로세스와 스레드의 차이

- **프로세스** : (하드디스크에 있는) 프로그램을 **메인 메모리에 올려 동적으로 실행시킨 상태**로, 
OS 입장에서의 최소 작업 단위
- **스레드** : 프로그램의 단위가 커지고 복잡해짐에 따라 **프로세스보다 더 작은 실행 단위**로, 
CPU 입장에서의 최소 작업 단위

## 프로세스

![image](https://user-images.githubusercontent.com/77563814/165147759-3f785416-1d9a-4564-83e3-44ad2368483e.png)

- **프로그램과 프로세스란?**
    - 프로그램 : 어떤 작업을 실행할 수 있는 파일
    (하드에 저장되어있는 정적인 상태의 실행파일)
    - 프로세스 : 실행 중인 프로그램
    (프로그램을 메모리에 올려 실행시킨 동적인 상태)
    
- **프로세스의 특징**
    - 프로세스가 메모리에 올라갈 때 운영체제로부터 **시스템 자원**을 할당 받는다.
    - **각각 독립적인 메모리 영역(Code, Data, Stack, Heap)을 할당받는다.**
    - 하나의 프로세스는 다른 프로세스의 변수나 자료구조에 접근할 수 없다.
    - 다른 프로세스의 자원에 접근하려면 프로세스 간의 통신(IPC, inter-process communication)을 사용해야 한다.
    - 프로세스당 최소 1개의 스레드(Main 스레드)를 가지고 있다.
    
- **시스템 자원의 종류**
    - CPU 시간
    - 메인 메모리의 주소 공간 (각 프로세스마다 별도의 주소 공간에서 실행)
    - **각각 독립적인 메모리 영역** [JAVA 메모리 영역](https://devkingdom.tistory.com/226)
        - **Code :** 코드 자체를 구성하는 메모리 영역
        - **Data :** 전역변수(global), 정적변수(static), 배열(array), 구조체(structure) 등이 저장되는 영역으로, 
프로그램이 실행 시 생성, 프로그램이 종료 시 반환
        - **Stack :** 지역(local) 변수, 매개변수(parameter), 리턴 값 등 잠시 사용되었다가 사라지는 데이터를 저장하는 영역으로, 
함수 호출 시 생성, 함수 종료시 반환
        - **Heap :** 프로그래머가 동적으로 사용하는 영역
        
- **프로세스의 단점**
    - 각각 **독립된 메모리 영역(Code, Data, Stack, Heap의 구조)을 할당**받는다.
    → 하나의 프로세스는 다른 프로세스의 변수나 자료구조에 접근할 수 없다.
    - 단점 : 각각 **독립된 메모리 영역을 가지기에** Context Switching에 많은 오버헤드가 발생한다.
    
    ( Context Switching 이란, 동작 중인 프로세스가 대기를 타면서 해당 프로세스의 상태(Context)를 보관하고, 대기하고 있던 다음 순번의 프로세스가 동작하면서 이전에 보관했던 프로세스의 상태(Context)를 복구하는 과정.)
    



## 스레드

![image](https://user-images.githubusercontent.com/77563814/165148304-d96dfd3c-f626-4619-a3a0-9ba84c2b045a.png)

- **스레드란**
    - **프로세스 내에서 실행되는 여러 흐름의 단위**
- **스레드의 특징 및 장단점**
    - 같은 프로세스의 스레드들끼리는 각각 Stack만 따로 할당받고, 프로세스 내의 주소 공간이나 자원들(Code/Data/Heap)은 공유한다.
    - 장점
        - 스레드간 데이터를 주고 받는 때 자원 소모가 줄고, 응답 시간 단축된다.
        - Context Switching의 오버헤드가 해결된다.
    - 단점
        - 서로 데이터를 사용하다가 충돌이 일어날 수 있다.
        - 어떤 스레드 하나에서 오류가 발생한다면 같은 프로세스 내의 다른 스레드 모두가 강제로 종료된다.

## 자바 스레드

- 자바에는 프로세스가 존재하지 않고 스레드만 존재
- JVM가 운영체제의 역할을 하여, JVM이 스레드 스케줄링을 한다.
- 싱글 스레드(Main 스레드)의 경우 코드의 위에서부터 아래로 순차적으로 실행한다.
- 멀티 스레드의 경우
    - 멀티 쓰레드를 사용하면 동시에 여러 개의 코드를 수행할 수 있다.

        동시에 엄청난 양이 들어오는 서비스에서는
        
        하나씩 처리하면 엄청난 시간이 걸리기 때문에
        
        쓰레드를 사용하여 많은 양도 한번에 처리하는 멀티스레드를 사용한다.
```java 
public class Task implements Runnable {
    @Override
    public void run() {
        // 실행 내용
    }
}

public static void main(String args[]){
    Runnable task = new Task();
    Thread thread1 = new Thread(task);
    Thread thread2 = new Thread(task);

    thread1.start(); // 작업A
    thread2.start(); // 작업B
}

// 두 작업이 동시에 실행된다
```
### **Java에서 Thread를 구현하는 방식**
    1. Thread 클래스를 이용하는 것
    2. Runnable 인터페이스를 이용하는 것 (일반적인 방식!)
    ⇒ 두 가지 중 한 가지 방법 선택 후 run() 함수 구현하고 start()로 실행
    
1. Thread 클래스를 이용하는 것
    - Thread 클래스를 상속받으면 다른 클래스를 상속 받지 못한다.

```java
class MyThread extends Thread {
 
    public void run() {
        작업내용          // Thread 클래스의 run()을 오버라이딩
    }
}

public class Thread implements Runnable
```

2. Runnable 인터페이스를 이용하는 것
    - Runnable은 run만 정의되어있는 인터페이스로, 재사용성이 높고 코드의 일관성을 유지할 수 있는 객체지향적인 방식이다.

```java
public class Test implements Runnable {
 
    @Override
    public void run() {
        작업내용          // Runnable 인터페이스의 run()을 구현
    }
}
```

[자바에서 스레드 구현](https://devlog-wjdrbs96.tistory.com/145)


[자바에서 멀티 스레드 처리](https://devkingdom.tistory.com/275?category=941391)
