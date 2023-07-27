# Cookie(쿠키) / Session(세션)

## Cookie(쿠키)

- 웹 사이트에 접속할 때 서버에 의해 사용자의 컴퓨터(브라우저)에 저장되는 정보
- `<이름>=<값>` 형태의 단순한 문자열
- 유실, 변조, 도난 등 보안에 취약하여 개인정보 등은 저장해서는 안됨


### 쿠키를 다루는 방법

- **HTTP** 메시지의 **헤더(header)** 영역을 통해서 송/수신
- `Set-Cookie:user=홍길동` 서버에서 클라이언트로 쿠키 전달(응답)
- `Cookie:user=홍길동;` 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달

![Set-Cookie (HTTP Response)](./Cookie%20And%20Session/set-cookie.png)

Set-Cookie (HTTP Response)

![Cookie (HTTP Request)](./Cookie%20And%20Session/cookie.png)

Cookie (HTTP Request)


### 쿠키를 사용하는 이유

- HTTP는 무상태(Stateless) 비연결지향(Connectionless) 프로토콜
- 클라이언트와 서버가 요청과 응답을 주고 받으면 연결이 끊어지고, 다시 요청할 때 서버는 이전 요청을 기억하지 못함
- 사용자 로그인 세션 관리, 장바구니, 광고 정보 트래킹(서드파티 쿠키) 등으로 활용


### 쿠키의 속성

**생명주기 - expires, max-ages**

- expires (만료일이 되면 쿠키 삭제)
    - `Set-Cookie: expires=Sat, 26-Dec-2020 04:39:21 GMT`
- max-ages (지정 시간이 지나면 쿠키 삭제)
    - `Set-Cookie: max-age=3600` (3600초)
- expires나 max-ages를 통해 만료 시점을 관리하는 쿠키를 영속쿠키 / 그렇지 않은 쿠키를 세션쿠키라고 하며, 세션 쿠키는 브라우저 종료시 삭제되고 영속 쿠키는 브라우저 종료에도 유지

**도메인 - domain**

- 명시: 명시한 문서 기준 도메인 + 서브 도메인 포함
    - `domain=example.org` → dev.example.org도 쿠키 접근 가능
- 생략: 현재 문서 기준 도메인만 적용
    - 현재 문서인 example.org에서만 쿠키 접근 가능

**경로 - path**

- 이 경로를 포함한 하위 경로 페이지만 쿠키 접근
    - `path=/home` → /home/* 가능
    - `path=/` → (루트 경로 지정: 모든 요청에 쿠키 전달)

**보안 - Secure, HttpOnly, SameSite**

- Secure
    - https인 경우에만 전송 (http X)
- HttpOnly
    - XSS 공격 방지
    - 자바스크립트에서 접근 불가
- SameSite
    - CSRF(XSRF) 공격 방지
    - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우(= 퍼스트 파티 쿠키)만 전송
    - `None`, `Lax`, `Strict` 세 가지 옵션(정책)

---

## Session(세션)

- 웹 사이트의 여러 페이지에 걸쳐 사용되는 사용자 정보를 저장하는 방법
- 일정 시간동안 같은 브라우저로 부터 들어오는 일련의 요구를 하나의 상태로 보고 그 상태를 일정하게 유지시키는 기술
- 서비스가 돌아가는 서버 측에 데이터를 저장하고, 세션의 키값만을 클라이언트 측에 남겨둠
- 브라우저는 필요할 때마다 이 키값을 이용하여 서버에 저장된 데이터를 사용


### 세션을 다루는 방법

![세션 생성](./Cookie%20And%20Session/session-create.png)
![세션 조회](./Cookie%20And%20Session/session-read.png)


### 세션을 사용하는 이유

- 쿠키를 통해 세션 ID만 노출시키고 개인정보나 민감한 데이터를 노출시키지 않음
- 보안에 취약한 쿠키를 보완해주는 역할
- 매 요청마다 전송하는 쿠키의 크기가 커지면 상당한 트래픽을 유발할 수 있음


### **다중 서버 환경에서의 세션 관리**

- 로드 밸런싱(Load Balancing)
    - 데이터 불일치(**정합성**)
- 스티키 세션(Sticky Session)
    - 장애 발생시 세션 유실(**가용성**)
- 세션 클러스터링(Session Clustering)
    - all-to-all Session Replication
    - primary-secondary Session Replication
- 세션 스토리지(Session Storage)
    - Disk based vs In-Memory
    - Replication(Master-Slave, failover)

---

### 출처

쿠키  
[모든 개발자를 위한 HTTP 웹 기본 지식](https://inf.run/vyK5)
https://www.daleseo.com/http-cookies/
    
세션    
https://smjeon.dev/web/sticky-session/
[https://code-lab1.tistory.com/쿠키(Cookie)와 세션(Session)의 차이, 쿠키란? 세션이란?](https://code-lab1.tistory.com/298)
[https://hyuntaeknote.tistory.com/다중 서버 환경에서 세션을 어떻게 공유하고 관리할까?](https://hyuntaeknote.tistory.com/4)
    

### 더 알아보기

보안   
[https://seob.dev/posts/브라우저-쿠키와-SameSite-속성/](https://seob.dev/posts/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-%EC%BF%A0%ED%82%A4%EC%99%80-SameSite-%EC%86%8D%EC%84%B1/)
[https://program-developer.tistory.com/XSS와 CSRF(XSRF)의 차이점](https://program-developer.tistory.com/99)
[https://nsinc.tistory.com/HTTP Only와 Secure Cookie 이해하기](https://nsinc.tistory.com/121)
    
JWT 인증  
[https://inpa.tistory.com/entry/WEB-📚-JWTjson-web-token-란-💯-정리](https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-JWTjson-web-token-%EB%9E%80-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC)
