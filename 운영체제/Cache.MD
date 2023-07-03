## 캐시(Cache)

컴퓨터 과학에서 데이터나 값을 미리 복사해 놓는 임시 장소를 가리킨다. 캐시의 접근 시간에 비해 원래 데이터를 접근하는 시간이 오래 걸리는 경우나 값을 다시 계산하는 시간을 절약하고 싶은 경우에 사용한다.([위키백과](https://ko.wikipedia.org/wiki/%EC%BA%90%EC%8B%9C))

### 캐시 메모리 (Cache Memory)

- 속도가 빠른 장치와 느린 장치에서 속도 차이에 따른 병목 현상을 줄이기 위한 메모리
- CPU가 주기억장치에서 저장된 데이터를 읽어올 때 자주 사용하는 데이터를 캐시 메모리에 저장한 뒤, 다음에 이용할 때 주기억장치가 아닌 캐시 메모리에서 먼저 가져오면서 속도를 향상시킨다.
- CPU는 보통 2~3개의 캐시메모리를 사용
- L1 -> L2 -> L3 순서로 탐색, L1은 CPU 내부, L2는 CPU와 램 사이, L3는 주로 메인보드에 존재
- 캐시는 RAM에 비해 속도 빠른 대신 용량이 적고 비쌈
- 캐시 크기를 키우면 캐시 접근 속도가 느려지고 파워를 많이 먹음 (trade-off)

### 캐시의 원리(지역성)

- 시간 지역성
for나 while 같은 반복문에 사용하는 조건 변수처럼 한번 참조된 데이터는 잠시후 또 참조될 가능성이 높음
- 공간 지역성
A[0], A[1]과 같은 연속 접근 시, 참조된 데이터 근처에 있는 데이터가 잠시후 또 사용될 가능성이 높음

### Cache Miss

- Cold miss
    
    한번도 캐시되지 않은 데이터에 접근하는 경우
    
    cache warming을 통해 사용할 것 같은 데이터들을 미리 캐시해놓을 수 있다.
    
- Conflict miss
    
    캐시 알고리즘에 의한 충돌
    
    캐시 용량은 충분한데 같은 공간에 저장하여 캐시가 충돌하는 경우
    
- Capacity miss
    
    캐시 용량에 의한 충돌
    
    물리적인 용량이 충분하지 않아 캐시할 수 없는 경우
    

### 동작 방식

- Direct Mapped Cache
- Fully Associative Cache
- Set Associative Cache

### Good Case

- 자주 변경되지 않고
    
    캐시가 자주 갱신되지 않고 그만큼 성능적인 이점이 높다.
    
- 자주 접근하는 데이터
    
    성능향상 폭이 클 가능성이 높은 데이터이다.
    

### Bad Case

- 자주 변경되고
    
    매번 캐시가 갱신되어 오히려 성능 저하를 야기할 수 있거나 캐시와 실제 데이터간의 괴리가 발생하여 무결성을 해칠 수 있다.
    
- 자주 접근하지 않는 데이터
    
    성능향상에 크게 도움되지 않는다. 
    

## 사용예시

### CDN

- Storage
- CDN(Content Delivery Network)

### Browser

- from disk cache
- from memory cache

### Server

- DB Cache (ex: redis, memcached)
    
    Web Server에서 데이터베이스에 접근하여 데이터를 가져오는 대신 캐시DB를 두어 더 빠르게 가져올 수 있다.
    

참고

- [https://gyoogle.dev/blog/computer-science/computer-architecture/캐시 메모리.html](https://gyoogle.dev/blog/computer-science/computer-architecture/%EC%BA%90%EC%8B%9C%20%EB%A9%94%EB%AA%A8%EB%A6%AC.html)
- https://d2.naver.com/helloworld/1059747
- [https://inpa.tistory.com/entry/REDIS-📚-캐시Cache-설계-전략-지침-총정리](https://inpa.tistory.com/entry/REDIS-%F0%9F%93%9A-%EC%BA%90%EC%8B%9CCache-%EC%84%A4%EA%B3%84-%EC%A0%84%EB%9E%B5-%EC%A7%80%EC%B9%A8-%EC%B4%9D%EC%A0%95%EB%A6%AC)
