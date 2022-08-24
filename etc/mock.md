# Mock

## Mock이란?
* 실제 객체를 만들기에는 비용과 시간이 많이 들거나 의존성이 크게 걸쳐있어 단위테스트 시 제대로 구현하기 어려운 경우 **가짜 객체**를 만들어 사용하는 기술.

## Mock 객체는 언제 필요한가?
1. 테스트 작성을 위한 환경 구축이 어려운 경우

   * 환경 구축을 위한 작업 시간이 많이 필요할 때 Mock 객체를 사용한다.(DB, 웹서버, FTP 등)
   * 아직 개발되지 않은 모듈을 사용하는 테스트가 필요할 때 사용

2. 테스트가 특정 경우나 순간에 의존적인 경우
3. 테스트 시간이 오래 걸리는 경우

## Mock 객체 분류

**1. 테스트 더블**

  테스트를 진행하기 어려운 경우 이를 대신해 테스트를 진행할 수 있도록 만들어주는 객체를 말합니다. Mock객체와 유사한 의미를 가지며 테스트 더블이 상위 의미로 사용됩니다.

**2. 더미 객체**

단순히 인스턴스화 될 수 있는 수준으로만 객체를 구현합니다. 객체의 기능은 사용하지 않고 객체 자체로만 테스트를 진행할 수 있을 때 사용합니다.

**3. 테스트 스텁**

더미 객체보다 좀 더 구현된 객체로 더미 객체가 실제로 동작하는 것처럼 보이게 만든 객체입니다. 객체의 특정 상태를 가정해서 만들어 특정 값을 리턴하거나 특정 메시지를 출력하는 작업을 합니다.

**4. 페이크 객체**

여러 상태를 대표할 수 있도록 구현된 객체로 실제 로직이 구현된 것처럼 보이게 합니다. 페이크 객체를 만드는 복잡도로 인해서 시간이 많이 걸릴 경우 적절한 수준에서 구현하거나, Mock 프레임 워크를 사용합니다.

**5. 테스트 스파이**

테스트에 사용되는 객체, 메소드의 사용 여부 및 정상 호출 여부를 기록하고 요청 시 알려줍니다. 특정 메소드가 호출되었을 때 또 다른 메서드가 실행이 되어야 한다와 같은 행위 기반 테스트가 필요한 경우 사용합니다. 테스트 스파이는 특수한 경우를 제외하고는 잘 쓰이지 않으며 보통 Mock 프레임워크에서 기본적으로 기능을 제공합니다.

**6. Mock 객체**

행위를 검증하기 위해 사용되는 객체를 지칭하며 수동으로 만들 수도 있고 프레임워크를 통해 만들 수 있습니다. 행위 기반 테스트는 복잡도나 정확성 등 작성하기 어려운 부분이 많기 때문에 상태 기반 테스트가 가능하다면 만들지 않는 게 좋습니다.



## Mock을  유닛 테스트

### 비즈니스 로직 클래스
```java
public class SimpleService {

    private SimpleDataRepository simpleDataRepository;

    public void setSimpleDataRepository(SimpleDataRepository simpleDataRepository) {
        this.simpleDataRepository = simpleDataRepository;
    }

    public int calculateSumUsingDataService() {
        int sum = 0;
        int[] data = simpleDataRepository.findAll();
        for(int value: data) {
            sum += value;
        }
        return sum;
    }
}
```

### 리포지토리 인터페이스

```java
public interface SimpleDataRepository {

    int[] findAll();
}
```

### Test Stub를 통해서 테스트

```java
class SimpleDataRepositoryStub implements SimpleDataRepository {

    @Override
    public int[] findAll() {
        return new int[] {1, 2, 3};
    }
}

public class SimpleServiceStubTests {

    @Test
    public void calculateSumUsingDataService_basic() {
        SimpleService simpleService = new SimpleService();
        simpleService.setSimpleDataRepository(new SimpleDataRepositoryStub());

        int actualResult = simpleService.calculateSumUsingDataService();
        int expectedResult = 6;
        assertEquals(expectedResult, actualResult);
    }
}
```

여기서 `Test Stub` 이란, 필요한 인터페이스에 대한 구현 객체로 마치 실제로 동작하는 것처럼 보이게 만들어 놓은 객체입니다. 

객체의 특정 상태를 가정해서 만들어 특정 값을 리턴해 주거나 특정 메시지를 출력해 주는 작업을 주로 하게 됩니다. 

그러나 특정 상태를 가정해서 Hard Coding 된 형태이기 때문에 로직에 따른 값의 변경은 테스트 할 수 없습니다.


### Mock Object를 통한 테스트
```java
public class SimpleServiceMockTests {

    @Test
    public void calculateSumUsingDataService_basic() {
        SimpleService simpleService = new SimpleService();

        // 1. Create mock
        SimpleDataRepository simpleDataRepositoryMock = mock(SimpleDataRepository.class);

        // 2. Specify mock -> when -> then
        when(simpleDataRepositoryMock.findAll()).thenReturn(new int[] {1, 2, 3});

        simpleService.setSimpleDataRepository(simpleDataRepositoryMock);
        int actualResult = simpleService.calculateSumUsingDataService();
        int expectedResult = 6;
        assertEquals(expectedResult, actualResult);
    }
}
```

여기서 `Mock Object`란, 테스트하고자 하는 코드에서 실제로 구현하기 어려운 객체들을 대신하여 동작하기 위해 만들어진 객체입니다.

테스트 시 Mock Object의 미리 정의된 결과를 통해서 테스트를 수월하게 진행할 수 있게 됩니다.


참고 : 

https://velog.io/@dnjscksdn98/JUnit-Mockito-Mock%EC%9D%B4%EB%9E%80

https://www.crocus.co.kr/1555
