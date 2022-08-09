# MVC1 Pattern과 MVC2 Pattern 의 비교


## MVC 패턴이란 (복습)

> 모델-뷰-컨트롤러(model-view-controller, MVC)는 소프트웨어 공학에서 사용되는 소프트웨어 디자인 패턴이다. 이 패턴을 성공적으로 사용하면, 
> 사용자 인터페이스로부터 비즈니스 로직을 분리하여 애플리케이션의 시각적 요소나 그 이면에서 실행되는 비즈니스 로직을 서로 영향 없이 쉽게 고칠 수 있는 애플리케이션을 만들 수 있다. 
> MVC에서 모델은 애플리케이션의 정보(데이터)를 나타내며, 뷰는 텍스트, 체크박스 항목 등과 같은 사용자 인터페이스 요소를 나타내고, 
> 컨트롤러는 데이터와 비즈니스 로직 사이의 상호동작을 관리한다.

<img width="738" alt="image" src="https://user-images.githubusercontent.com/36829127/183573087-76840af2-1387-497f-b71f-74e9ca675daf.png">


### Model
* Model은 데이터와 애플리케이션이 무엇을 할 것인지를 정의하는 부분으로 **내부 비즈니스 로직을 처리하기 위한 역할**을 함
* 모델은 컨트롤러가 호출하면 DB와 연동하여 사용자의 입출력 데이터를 다루는 일과 같이 데이터와 연관된 비즈니스 로직을 처리하는 역할을 함
* 데이터 추출, 저장, 삭제, 업데이트 등의 역할을 수행함

### View
* View는 사용자에게 보여주는 화면(UI)이 해당됨.
* 사용자와 상호작용을 하며 컨트롤러로부터 받은 모델의 결과값을 사용자에게 화면으로 출력하는 일을 함.
* MVC에서는 여러개의 View가 존재할 수 있음.
* Model에서 받은 데이터는 별도로 저장하지 않음.

### Controller
* Controller는 Model과 View 사이를 이어주는 인터페이스 역할
* 즉, Model이 데이터를 어떻게 처리할지 알려주는 역할을 함
* 사용자로부터 View에 요청이 있으면 Controller는 해당 업무를 수행하는 Model을 호출하고 Model이 업무를 모두 수행하면 다시 결과를 View에 전달하는 역할을 함


----

## MVC1 패턴

<img width="665" alt="image" src="https://user-images.githubusercontent.com/36829127/183574587-3bb810be-bfc7-4b26-9c22-1a6aafa82caf.png">


MVC1 패턴의 경우 View와 Controller를 모두 JSP가 담당하는 형태를 가진다. 즉, JSP 하나로 유저의 요청을 받고 처리하므로 구현은 쉽다.

다만, 내용이 복잡해지거나 거대한 프로젝트가 될수록 이 패턴은 힘을 잃는다. JSP 하나에서 모두 이루어지다보니 재사용성도 떨어지고, 유지보수도 힘들어지기 때문이다.

## MVC2 패턴

<img width="663" alt="image" src="https://user-images.githubusercontent.com/36829127/183575646-8a05d9b2-dd65-415e-bd12-77e3b305a308.png">

MVC2는 표준으로 사용되는 패턴이며, 요청을 하나의 **Controller(Servlet)** 가 먼저 받는다. MVC1과는 다르게 Controller와 View가 분리되어 있다. 즉, 요청을 받는 부분과 요청을 보여주는 부분이 분리되어 있다.

역할이 분리되어 MVC1의 단점을 보완할 수 있다. M,V,C 의 모듈 중 하나의 부분만 수정해야 한다면 해당 모듈만 수정해주면 된다. 유지보수에 큰 이점을 갖는다.

## Spring MVC2 패턴
스프링 MVC는 MVC2 패턴을 따르고 있다. 

<img width="665" alt="image" src="https://user-images.githubusercontent.com/36829127/183576617-afe0f792-8250-4e0a-96fd-2e3e6480d6b0.png">

스프링에서는 유저의 요청을 받는 **DispathcerServlet**이 핵심이다. 이것이 **Front Controller**의 역할을 맡는다.

Front Controller(프런트 컨트롤러)는 우선적으로 유저(클라이언트)의 모든 요청을 받고, 그 요청을 분석하여 세부 컨트롤러들에게 필요한 작업을 나눠주게 된다.

<img width="675" alt="image" src="https://user-images.githubusercontent.com/36829127/183577036-0b3ad6c2-ffec-4314-93d8-3041a22afb6b.png">


참고 : https://chanhuiseok.github.io/posts/spring-3/
