## 상태 코드

- 클라이언트가 보낸 **요청의 처리 상태**를 서버가 응답에서 알려주는 기능
- 상태 코드의 첫 번째 숫자는 **다섯 가지 표준 응답 클래스** 중 하나를 지정(1~5)

### 상태 코드의 종류

- **1xx (Informational)**: 요청이 수신되어 처리
- **2xx (Successful)**: 요청 정상 처리
- **3xx (Redirection)**: 요청을 완료하려면 추가 행동이 필요
- **4xx (Client Error)**: 클라이언트 오류, 잘못된 문법등으로 서버가 요청을 수행할 수 없음
- **5xx (Server Error)**: 서버 오류, 서버가 정상 요청을 처리하지 못함

### 클라이언트가 인식할 수 없는 상태코드를 서버가 반환한다면

- 클라이언트는 상위 상태코드로 해석해서 처리
- 미래에 새로운 상태 코드가 추가되어도 클라이언트를 변경하지 않아도 됨
- 예)
    - 299 ??? -> 2xx (Successful)
    - 451 ??? -> 4xx (Client Error)
    - 599 ??? -> 5xx (Server Error)

---

## [Informational 1xx](https://httpwg.org/specs/rfc9110.html#status.1xx)

- **요청이 수신되어 처리중**
- 요청된 작업을 완료하고 최종 응답을 전송하기 전에 연결 상태 또는 요청 진행 상황을 전달하기 위한 중간 응답
- HTTP/1.0에는 1xx 상태 코드가 정의되어 있지 않으므로 서버는 HTTP/1.0 클라이언트에 1xx 응답을 보내서는 안됨

---

## [Successful 2xx](https://httpwg.org/specs/rfc9110.html#status.2xx)

- **클라이언트의 요청을 성공적으로 처리**

### 200 OK

- 성공적인 HTTP 요청에 대한 표준 응답

### 201 Created

- 요청 성공해서 새로운 리소스가 생성됨
- 생성된 리소스는 응답의 Location 헤더 필드로 식별

### 202 Accepted

- 요청이 접수되었으나 처리가 완료되지 않았음
- 실행될 수도 있고 실행되지 않을 수도 있으며, 처리가 발생할 때 허용되지 않을 수도 있음
- 배치 처리 같은 곳에서 사용(요청 접수 후 1시간 뒤에 배치 프로세스가 요청을 처리)

### 204 No Content

- 서버가 요청을 성공적으로 수행했지만, 응답 페이로드 본문에 보낼 데이터가 없음
- 예) 웹 문서 편집기에서 save 버튼
    - save 버튼의 결과로 아무 내용이 없어도 된다.
    - save 버튼을 눌러도 같은 화면을 유지해야 한다.
    - 결과 내용이 없어도 204 메시지(2xx)만으로 성공을 인식할 수 있다.

---

## [Redirection 3xx](https://httpwg.org/specs/rfc9110.html#status.3xx)

- 요청을 완료하기 위해 유저 에이전트의 추가 조치 필요
- 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동

### 리다이렉션 종류

- **영구 리다이렉션**: 특정 리소스의 URI가 영구적으로 이동
예) /members -> /users
예) /event -> /new-event
- **일시 리다이렉션**: 일시적인 변경
주문 완료 후 주문 내역 화면으로 이동
**PRG**: Post/Redirect/Get
- **특수 리다이렉션**
결과 대신 캐시를 사용

### 영구 리다이렉션 - 301, 308

- 리소스의 URI가 영구적으로 이동
- 원래의 URL를 사용X, 검색 엔진 등에서도 변경 인지
- **301 Moved Permanently**
    
    리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음(MAY)
    
- **308 Permanent Redirect**
    
    리다이렉트시 요청 메서드와 본문 유지(처음 POST를 보내면 리다이렉트도 POST 유지)
    

### 일시 리다이렉션 - 302, 307, 303

- 리소스의 URI가 일시적으로 변경
- 따라서 검색 엔진 등에서 URL을 변경하면 안됨
- **302 Found**
    
    리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음(MAY)
    
- **307 Temporary Redirect**
    
    리다이렉트시 요청 메서드와 본문 유지(요청 메서드를 변경하면 안된다. MUST NOT)
    
- **303 See Other**
    
    리다이렉트시 요청 메서드가 GET으로 변경
    

### PRG: Post/Redirect/Get

- PRG 미적용
    - POST로 주문후에 웹 브라우저를 새로고침
    - 새로고침으로 **POST 재요청 → 중복 주문**이 될 수 있다.
    
- PRG 적용
    - POST로 주문 후에 **주문 결과 화면을 GET 메서드로 리다이렉트**
    - 새로 고침으로 인한 **중복 주문 방지**
    - 중복 주문 대신에 결과 화면만 GET으로 다시 요청

### 기타 리다이렉션 - 300, 304

- **300 Multiple Choices**: 안쓴다
- **304 Not Modified**
    - 캐시를 목적으로 사용
    - 클라이언트에게 리소스가 수정되지 않았음을 알려줌 → 클라이언트는 로컬에 저장된 캐시를 재사용(= 캐시로 리다이렉트)
    - 304 응답은 응답에 **메시지 바디를 포함하면 안됨** (로컬 캐시를 사용해야 하므로)
    - 조건부 GET, HEAD 요청시 사용

---

## [Client Error 4xx](https://httpwg.org/specs/rfc9110.html#status.4xx)

- **클라이언트에 의해 발생한 오류로** 서버가 요청을 수행할 수 없음
- 클라이언트가 이미 잘못된 요청을 보내고 있기 때문에 **똑같은 재시도가 실패**함

### 400 Bad Request

- **클라이언트가 잘못된 요청을 해서 서버가 요청을 처리할 수 없음**
- 요청 구문, 메시지 등등 오류
    
    예) 요청 파라미터가 잘못되거나, API 스펙이 맞지 않을 때
    

### 401 Unauthorized

- **클라이언트가 해당 리소스에 대한 인증이 필요함**
- 인증(Authentication) 되지 않음
- 401 오류 발생시 응답에 WWW-Authenticate 헤더와 함께 인증 방법을 설명
- 참고
    - **인증(Authentication)**: 본인이 누구인지 확인(로그인)
    - **인가(Authorization)**: 권한부여(Admin 권한처럼 특정 리소스에 접근할 수 있는 권한,
    인증이 있어야 인가가 있음)
    - 오류 메시지가 Unauthorized 이지만 ‘인증 되지 않음’ 이다.

### 403 Forbidden

- **서버가 요청을 이해했지만 승인을 거부함**
- 주로 인증 자격 증명은 있지만, 접근 권한이 불충분한 경우
    
    예) 어드민 등급이 아닌 사용자가 로그인은 했지만, 어드민 등급의 리소스에 접근하는 경우
    

### 404 Not Found

- **요청 리소스를 찾을 수 없음**
- 요청 리소스가 서버에 없거나 클라이언트가 권한이 부족한 리소스에 접근할 때 해당 리소스를 숨기고 싶을 때

## [Server Error 5xx](https://httpwg.org/specs/rfc9110.html#status.5xx)

- **서버 오류**
- 서버에 문제가 있기 때문에 **재시도 하면 성공할 수도 있음**

### 500 Internal Server Error

- 서버 내부 문제로 오류 발생
- 애매하면 500 오류

### 502 Bad Gateway

- 게이트웨이 또는 프록시 역할을 하는 서버가 통신 체인의 다른 서버로부터 유효하지 않거나 잘못된 응답을 받을 때 발생
- + 504 Gateway Timeout: 시간 초과
- 주로 nginx 기반 서버에서 많이 발생

### 503 Service Unavailable

- **서비스 이용 불가**
- 서버가 일시적인 과부하 또는 예정된 작업으로 잠시 요청을 처리할 수 없음
- Retry-After 헤더 필드로 얼마뒤에 복구되는지 보낼 수도 있음

---

## 출처

- [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC)
- https://httpwg.org/specs/rfc9110.html
- https://en.wikipedia.org/wiki/List_of_HTTP_status_codes
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Status
