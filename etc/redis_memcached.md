## Redis란

- `key-value`구조의 데이터 관리 시스템이다. (NoSQL)
- 메모리에 저장하는 Inmemory DB이다. 메모리에 저장하므로 속도가 빠르다.
- 단, Redis는 용량(메모리 가격이 비싸므로), key-value 구조 때문에 메인 데이터베이스 서버로 사용하기보다는 캐시 데이터베이스 서버로 사용한다.
- 특정 시점에 데이터를 디스크에 저장하여 파일 보관이 가능하다. ⇒ 장애발생시 복구가 가능합니다(RDB라는 현재 메모리의 Data Set에 대해서 Snapshot을 만들 수 있다).

> 인메모리 DB란?
> 
> 디스크가 아닌 메모리에 데이터를 저장하는 데이터베이스를 뜻한다. 디스크가 아닌 메모리에 데이터를 저장하기 때문에, I/O 속도가 빠르다. 단, 메모리 가격이 비싸기에 메인 데이터베이스보다는 캐시 데이터베이스로 사용한다.
> 

## Redis와 Memcached의 차이

Redis와 Memcached는 거의 비슷하나, Redis가 더 많은 기능/지료구조/데이터 복구를 지원한다.

- **데이터 타입 (** Redis  👍)
    - Redis : `String, Set, Sorted, Set, Hash, List` 을 지원한다.
    - Memcached : `String` 만 지원한다.
- **데이터 복구 (** Redis  👍)
    - Redis : 데이터를 Disk에도 저장하기 때문에 프로세스의 돌발 종료, 서버 종료 등 돌발 상황에서도 데이터를 복구할 수 있다.
    - Memcached : Disk에 따로 저장하지 않기 때문에 돌발 종료시 모든 데이터가 유실된다.
- **Data Eviction (** Redis 👍)
    - Redis : Redis는 6가지 Eviction 정책을 통해 더욱 세밀한 Eviction 제어가 가능합니다.
    - Memcached : Memcached는 LRU 알고리즘을 통한 Eviction을 지원합니다
- **멀티 스레드 지원 여부 (** Memcached  👍)
    - Redis : Single Thread로 동작한다. 한번에 하나의 명령어만 처리할 수 있다. → 데이터 손실없이 클러스터링을 통해 Scale-out할 수 있다.
    - Memcached : Multi Thread를 지원하기 때문에 서버 Scale up에 유리하다.
- 메모리 **(** Memcached  👍)
    - Redis : Copy&Write 방식을 사용하기 때문에 실제 사용하는 메모리보다 더 많은 메모리를 요구합니다.
    - Memcached : 레디스에 비해 적은 메모리를 요구한다. HTML 코드를 캐싱할 때 내부 메모리 관리가 비교적 단순하다.

> 빠른 통신 속도가 필요하면 트래픽이 몰려도 안정적인 Memcached를 사용하는게 좋고, 서비스의 기능을 위한 목적이라면 Redis를 사용하는게 좋다고 한다. 사실상 대부분의 서비스에서는 기능/자료 구조가 많은 Redis를 사용한다고 한다.
> 

**Reference**

- [https://chrisjune-13837.medium.com/redis-vs-memcached-10e796ddd717](https://chrisjune-13837.medium.com/redis-vs-memcached-10e796ddd717)
- [https://deveric.tistory.com/65](https://deveric.tistory.com/65)
