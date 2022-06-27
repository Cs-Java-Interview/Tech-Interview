### 객체란

- 객체(Object)란 프로그램으로 구현할 어떠한 대상이다.
- 물리적으로 존재하거나 추상적으로 생각할 수 있는 것 중에서 자신만의 고유한 속성을 가지고 있는 것을 말한다.
- 객체는 속성과 동작으로 이루어져 있고, 
자바에서는 각각 필드(field) 와 메소드(method) 라 부른다.


### 객체지향 프로그래밍을 구현하는 방법

1. ****클래스 기반 언어****
    - 클래스로 객체의 자료구조와 기능을 정의하고 생성자를 통해 인스턴스를 생성한다.
    - ex) Java, C++, C#, Python, PHP 등
    - 
        
        ```java
        class Myclass { // 클래스
          private String name;
          public void setName(String name) {
            this.name = name;
          }
        }
          public static void main(String[] args) {
            Myclass test = new Myclass(); // 인스턴스
          }
        }
        ```
        
2. ****프로토타입 기반****
    - 클래스가 없어서 객체지향이 아니라고 생각할 수 있으나 프로토타입 기반의 객체지향 언어다.
    - ex) 자바스크립트 (멀티-패러다임 언어fh, 명령형, 함수형, 프로토타입 기반 객체지향 언어다.)
    - 자바스크립트는 class문이 포함되지 않은 프로토타입 기반 언어이다.
    -  ```jsx
        function Myclass (name) { // 자바스크립트 생성자
          this.name = name;
        }

        var test = new Myclass('Sample');
        ```
