## 64bit CPU vs 32bit CPU

여기서 bit수는 해당 CPU의 데이터 기본 처리 단위를 의미한다. 다른 말로, 레지스터의 bit 수를 의미한다.

> 여기서 레지스터란?
 : CPU (Central Processing Unit)가 요청을 처리하는 데 필요한 데이터를 일시 저장하는 기억장치 

  CPU가 RAM에 접근하는 시간(Memory access time)이 오래 걸려서 CPU 데이터 처리 속도에 영향을 주었다. 따라서 CPU와 가까운 저장공간이 필요했고, 그것이 레지스터이다.
> 

레지스터는 CPU가 처리하는 데이터의 최소 단위이다. 64bit CPU, 32bit CPU에서의 bit는 각 레지스터의 단위가 몇 bit인지 의미한다. 즉 64bit CPU는 64bit, 32bit CPU는 32bit크기의 레지스터를 가지고 처리한다.

## 64bit, 32bit OS과 하위호환

64bit CPU, 32bit CPU처럼 OS도 64bit, 32bit가 있다. CPU는 하드웨어, OS는 소프트웨어라 볼 수 있다. 

64bit CPU는 32bit, 64bit OS 다 설치할 수 있지만, 32bit CPU에는 32bit OS만 작동시킬 수 있다.

비슷하게 CPU와 어플리케이션 관계에도 하위호환이 적용된다. 64bit CPU에는 32bit, 64bit 어플리케이션을 설치할 수 있으나, 32bit CPU에는 32bit 어플리케이션만 작동시킬 수 있다.

## 메모리 용량

```
메모리의 최대 용량 = 최대 메모리 주소 개수 * 1byte(메모리 주소값의 단위)

// 32bit
4GB = 2^32 * 1byte
// 64bit
18EB = 2^64 * 1byte
```

32bit CPU의 경우 2^32개의 메모리 주소를 저장할 수 있다. 이때 메모리 주소값의 단위는 1byte이므로, 총 2^32byte의 메모리 주소를 저장할 수 있다. 2^32byte = 4,096MB = 4GB (GB = 2^30byte)이다.

따라서 32bit CPU는 최대 4GB의 메모리 공간을 가질 수 있다. 다른 말로 하면, 32bit 레지스터를 가진 RAM에서는 메모리 크기가 4GB를 넘을 수 없다. 이는 일반 사용자에게도 많이 부족한 용량이라 이후에 64bit가 등장하게 되었다.

참고로 64bit CPU에서는실제로 256TB의 용량만 사용한다. 18EB (2^64 * 1byte)크기의 큰 용량까지는 필요하지 않기 때문이다.

### Reference

- [https://eine.tistory.com/entry/64비트-32비트-CPU와-운영체제-에-대하여](https://eine.tistory.com/entry/64%EB%B9%84%ED%8A%B8-32%EB%B9%84%ED%8A%B8-CPU%EC%99%80-%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC)
- [https://velog.io/@younoah/CPU의-32비트와-64비트란](https://velog.io/@younoah/CPU%EC%9D%98-32%EB%B9%84%ED%8A%B8%EC%99%80-64%EB%B9%84%ED%8A%B8%EB%9E%80)
