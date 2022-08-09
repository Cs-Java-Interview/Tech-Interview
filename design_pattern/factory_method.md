# 팩토리 메소드 패턴

## 팩토리 메소드 패턴은 생성 패턴 중 하나이다.

### 생성 패턴

* 생성 패턴은 인스턴스를 만드는 절차를 추상화하는 패턴이다.
* 생성 패턴에 속하는 패턴들은 객체를 생성, 합성하는 방법이나 객체의 표현 방법을 시스템과 분리해준다.
* 생성 패턴은 시스템이 상속(inheritance) 보다 복합(composite) 방법을 사용하는 방향으로 진화되어 가면서 더 중요해지고 있다.

**생성패턴의 이슈 2가지**

1. 생성 패턴은 시스템이 어떤 Concrete Class를 사용하는지에 대한 정보를 캡슐화한다.
2. 생성 패턴은 이들 클래스의 인스턴스들이 어떻게 만들고 어떻게 결합하는지에 대한 부분을 완전히 가려준다.

즉, 생성 패턴을 이용하면 무엇이 생성되고, 누가 이것을 생성하며, 이것이 어떻게 생성되는지, 언제 생성할 것인지 결정하는 데 유연성을 확보할 수 있게 된다.

## 팩토리 메소드 패턴이란?

* 팩토리 패턴은 객체를 생성하는 인터페이스는 미리 정의하되, 인스턴스를 만들 클래스의 결정은 서브 클래스 쪽에서 내리는 패턴이다.
* 다시 말해 **여러 개의 서브 클래스를 가진 슈퍼 클래스가 있을 때 인풋에 따라 하나의 자식 클래스의 인스턴스를 리턴해주는 방식이다.** 
* 팩토리 패턴에서는 클래스의 인스턴스를 만드는 시점을 서브 클래스로 미룬다.
  * 이 패턴은 인스턴스화에 대한 책임을 객체를 사용하는 클라이언트에서 팩토리 클래스로 가져온다.

### 팩토리 메소드는 언제 사용하는가?

1. 어떤 클래스가 자신이 생성해야 하는 객체의 클래스를 예측할 수 없을 때
2. 생성할 객체를 기술하는 책임을 자신의 서브클래스가 지정했으면 할 때


### 팩토리 메소드 패턴 장점

1. 팩토리 패턴은 클라이언트 코드로부터 서브 클래스의 인스턴스화를 제거하여 **서로 간의 종속성을 낮추고, 결합도를 느슨하게 하며(Loosely Coupled), 확장을 쉽게 한다.**
서브 클래스에 대해 수정 혹은 삭제가 일어나더라도 클라이언트는 알 수 없기 때문에 코드를 변경할 필요도 없습니다.
2. 팩토리 패턴은 클라이언트와 구현 객체들 사이에 추상화를 제공합니다.

### 팩토리 메소드 패턴 예제

팩토리 패턴에 사용되는 슈퍼 클래스는 인터페이스나 추상 클래스, 혹은 그냥 평범한 자바 클래스여도 상관없다.
아래 예제에서는 슈퍼 클래스인 `Computer`, 서브 클래스인 `Server`와 `PC`, 그리고 이 객체를 생성해주는 `Factory Class`를 작성해보자

**Super Class**
```java
public abstract class Computer {
	
    public abstract String getRAM();
    public abstract String getHDD();
    public abstract String getCPU();
	
    @Override
    public String toString(){
        return "RAM= "+this.getRAM()+", HDD="+this.getHDD()+", CPU="+this.getCPU();
    }
}
```


**Sub Class - Server**
```java
public class PC extends Computer {
 
    private String ram;
    private String hdd;
    private String cpu;
	
    public PC(String ram, String hdd, String cpu){
        this.ram=ram;
        this.hdd=hdd;
        this.cpu=cpu;
    }
    @Override
    public String getRAM() {
        return this.ram;
    }
 
    @Override
    public String getHDD() {
    return this.hdd;
    }
 
    @Override
    public String getCPU() {
        return this.cpu;
    }
 
}
```


**Sub Class - PC**

```java
public class Server extends Computer {
 
    private String ram;
    private String hdd;
    private String cpu;
	
    public Server(String ram, String hdd, String cpu){
        this.ram=ram;
        this.hdd=hdd;
        this.cpu=cpu;
    }
    @Override
    public String getRAM() {
        return this.ram;
    }
 
    @Override
    public String getHDD() {
        return this.hdd;
    }
 
    @Override
    public String getCPU() {
        return this.cpu;
    }
 
}
```


마지막으로 객체 생성의 역할을 담당하는 **Factory Class**를 생성해보자

**Factory Class**

```java
public class ComputerFactory {
 
    public static Computer getComputer(String type, String ram, String hdd, String cpu){
        if("PC".equalsIgnoreCase(type))
            return new PC(ram, hdd, cpu);
        else if("Server".equalsIgnoreCase(type))
            return new Server(ram, hdd, cpu);
		
        return null;
    }
}
```

* ComputerFactory 클래스의 getComputer 메소드를 살펴보면 **static** 메소드로 구현되었다는 점을 살펴볼 수 있다.
* 메소드 내부 코드를 보면 type의 값이 "PC"일 경우 PC 클래스의 인스턴스를, "Server"일 경우 Server 클래스의 인스턴스를 리턴하는 것을 볼 수 있다.

**이렇듯 팩토리 메소드 패턴을 사용하게 된다면 인스턴스를 필요로 하는 Application에서 Computer의 Sub 클래스에 대한 정보는 모른 채 인스턴스를 생성할 수 있게 된다.**

이를 통해, Computer 클래스에 더 많은 Sub 클래스가 추가된다 할지라도 getComputer()를 통해 인스턴스를 제공받던 Application의 코드는 수정할 필요가 없게 된다.

팩토리 메소드 패턴을 구현하는 데 중요한 점이 두 가지가 있다.
1. Factory class를 **Singleton**으로 구현해도 되고, 서브클래스를 리턴하는 **static** 메소드로 구현해도 됨
2. 팩토리 메소드는 위 예제의 getComputer()와 같이 입력된 파라미터에 따라 다른 서브 클래스의 인스턴스를 생성하고 리턴한다.

**Main method**
```java
public class TestFactory {
 
    public static void main(String[] args) {
        Computer pc = ComputerFactory.getComputer("pc","2 GB","500 GB","2.4 GHz");
        Computer server = ComputerFactory.getComputer("server","16 GB","1 TB","2.9 GHz");
        System.out.println("Factory PC Config::"+pc);
        System.out.println("Factory Server Config::"+server);
    }
 
}
```

예제에서 작성한 ComputerFactory 클래스를 사용하여 PC와 Server 클래스의 인스턴스를 생성할 수 있다.

**결과**
```
Factory PC Config::RAM= 2 GB, HDD=500 GB, CPU=2.4 GHz
Factory Server Config::RAM= 16 GB, HDD=1 TB, CPU=2.9 GHz
```


참고 : https://readystory.tistory.com/117
