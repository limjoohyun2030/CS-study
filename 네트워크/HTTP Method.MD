# HTTP Method

## HTTP Method 종류

### GET

- **리소스 조회**
- 서버에 전달하고 싶은 데이터는 query(쿼리 파라미터, 쿼리 스트링)를 통해서 전달
- 메시지 바디를 사용해서 데이터를 전달할 수 있지만, 지원하지 않는 곳이 많아서 권장하지
않음

```json
GET /search?q=hello&hl=ko HTTP/1.1
Host: www.google.com
```

### POST

- 요청 데이터 처리 (주로 등록에 사용)
- 메시지 바디를 통해 서버로 요청 데이터 전달
- 서버는 요청 데이터(메시지 바디를 통해 들어온)를 처리하는 모든 기능을 수행
- 주로 전달된 데이터로 **신규 리소스 등록**, **프로세스 처리**에 사용
    - **새 리소스 생성(등록)**
        
        예) 신규 유저 회원 가입
        
    - **요청 데이터 처리**
        
        예) 주문에서 결제완료 -> 배달시작 -> 배달완료 처럼 단순히 값 변경을 넘어 프로세스의 상태가 변경되는 경우
        
    - **다른 메서드로 처리하기 애매한 경우**
        
        예) JSON으로 조회 데이터를 넘겨야 하는데, GET 메서드를 사용하기 어려운 경우
        

```json
POST /members HTTP/1.1
Content-Type: application/json
{
 "username": "hello",
 "age": 20
}
```

### PUT

- 리소스를 대체, 해당 리소스가 없으면 생성 (일종의 덮어쓰기)
- 클라이언트가 리소스를 식별
    - 클라이언트가 리소스 위치를 알고 URI 지정함
    - POST와 차이점

```json
PUT /members/100 HTTP/1.1
Content-Type: application/json
{
 "username": "hello",
 "age": 20
}
```

### PATCH

- 리소스 부분 변경

```json
PATCH /members/100 HTTP/1.1
Content-Type: application/json
{
 "age": 50
}
```

### PUT vs PATCH

- 서버에 저장된`/members/100` 에 해당하는 데이터
    
    ```json
    {
     "username": "young",
     "age": 20
    }
    ```
    
- PATCH
    
    ```json
    // 요청
    PATCH /members/100 HTTP/1.1
    Content-Type: application/json
    {
     "age": 50
    }
    // 결과
    {
     "username": "young",
     "age": 50
    }
    ```
    
- PUT
    
    ```json
    // 요청
    PUT /members/100 HTTP/1.1
    Content-Type: application/json
    {
     "age": 50
    }
    // 결과
    {
     "age": 50
    }
    ```
    

### DELETE

- 리소스 삭제

```json
DELETE /members/100 HTTP/1.1
Host: localhost:8080
```

---

### 기타 메서드

- HEAD: GET과 동일하지만 메시지 부분을 제외하고, 상태 줄과 헤더만 반환
- OPTIONS: 대상 리소스에 대한 통신 가능 옵션(메서드)을 설명(주로 CORS에서 사용)
- CONNECT: 대상 리소스로 식별되는 서버에 대한 터널을 설정
- TRACE: 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수행

## HTTP 메서드의 속성

| HTTP 메소드 | RFC | 요청에 Body가 있음 | 응답에 Body가 있음 | 안전 | 멱등(Idempotent) | 캐시 가능 |
| --- | --- | --- | --- | --- | --- | --- |
| GET | https://datatracker.ietf.org/doc/html/rfc7231 | 아니요 | 예 | 예 | 예 | 예 |
| HEAD | https://datatracker.ietf.org/doc/html/rfc7231 | 아니요 | 아니요 | 예 | 예 | 예 |
| POST | https://datatracker.ietf.org/doc/html/rfc7231 | 예 | 예 | 아니요 | 아니요 | 예 |
| PUT | https://datatracker.ietf.org/doc/html/rfc7231 | 예 | 예 | 아니요 | 예 | 아니요 |
| DELETE | https://datatracker.ietf.org/doc/html/rfc7231 | 아니요 | 예 | 아니요 | 예 | 아니요 |
| CONNECT | https://datatracker.ietf.org/doc/html/rfc7231 | 예 | 예 | 아니요 | 아니요 | 아니요 |
| OPTIONS | https://datatracker.ietf.org/doc/html/rfc7231 | 선택 사항 | 예 | 예 | 예 | 아니요 |
| TRACE | https://datatracker.ietf.org/doc/html/rfc7231 | 아니요 | 예 | 예 | 예 | 아니요 |
| PATCH | https://datatracker.ietf.org/doc/html/rfc5789 | 예 | 예 | 아니요 | 아니요 | 예 |

https://ko.wikipedia.org/wiki/HTTP

### 안전(Safe Methods)

호출해도 리소스를 변경하지 않는다.

### 멱등(Idempotent Methods)

한 번 호출하든 두 번 호출하든 100번 호출하든 결과가 똑같다.

- 멱등 메서드
    - GET: 한 번 조회하든, 두 번 조회하든 같은 결과가 조회된다.
    - PUT: 결과를 대체한다. 따라서 같은 요청을 여러번 해도 최종 결과는 같다.
    - DELETE: 결과를 삭제한다. 같은 요청을 여러번 해도 삭제된 결과는 똑같다.
- 자동 복구 메커니즘(서버가 TIMEOUT 등으로 정상 응답을 못주었을 때, 클라이언트가 같은 요청을 다시 해도 되는가? 에 대한 판단 근거) 등으로 활용한다.
- 멱등은 외부 요인으로 중간에 리소스가 변경되는 것 까지는 고려하지는 않는다.

### 캐시가능(Cacheable Methods)

- 응답 결과 리소스를 캐시해서 사용해도 되는가?
- GET, HEAD, POST, PATCH 캐시가능
- 실제로는 GET, HEAD 정도만 캐시로 사용 (POST, PATCH는 본문 내용까지 캐시 키로 고려해야 하는데, 구현이 쉽지 않음)

## URI 설계

### URI 설계 개념

- 문서(document)
    - 단일 개념(파일 하나, 객체 인스턴스, 데이터베이스 row)
    - 예) /members/100, /files/star.jpg
- 컬렉션(collection)
    - 서버가 관리하는 리소스 디렉터리
    - 서버가 리소스의 URI를 생성하고 관리
    - 예) /members
- 스토어(store)
    - 클라이언트가 관리하는 자원 저장소
    - 클라이언트가 리소스의 URI를 알고 관리
    - 예) /files
- 컨트롤러(controller), 컨트롤 URI
    - 문서, 컬렉션, 스토어로 해결하기 어려운 추가 프로세스 실행
    - 동사를 직접 사용
    - 예) /members/{id}/delete

### URI 설계 예시

**HTTP API - 컬렉션**

- POST 기반 등록
- 서버가 리소스 URI 결정
- **HTTP API - 스토어**
    - PUT 기반 등록
    - 클라이언트가 리소스 URI 결정
- **HTML FORM 사용**
    - 순수 HTML + HTML form 사용
    - GET, POST만 지원

## API 설계 예시

### HTTP API 데이터 전송

- 서버 to 서버
    - 백엔드 시스템 통신
- 앱 클라이언트
    - 아이폰, 안드로이드
- 웹 클라이언트
    - HTML에서 Form 전송 대신 자바 스크립트를 통한 통신에 사용(AJAX)
    - 예) React, VueJs 같은 웹 클라이언트와 API 통신
- POST, PUT, PATCH: 메시지 바디를 통해 데이터 전송
- GET: 조회, 쿼리 파라미터로 데이터 전달
- Content-Type: application/json을 주로 사용 (사실상 표준)
    - TEXT, XML, JSON 등등

### HTTP API 설계

- HTTP API - 컬렉션
    - POST 기반 등록
    - 예) 회원 관리 API 제공
- HTTP API - 스토어
    - PUT 기반 등록
    - 예) 정적 컨텐츠 관리, 원격 파일 관리
- HTML FORM 사용
    - 웹 페이지 회원 관리
    - GET, POST만 지원

### **POST 기반 등록 API 설계**

 **회원 관리 시스템**

- 회원 목록 /members -> GET
- 회원 등록 /members -> POST
- 회원 조회 /members/{id} -> GET
- 회원 수정 /members/{id} -> PATCH, PUT, POST
- 회원 삭제 /members/{id} -> DELETE

**POST - 신규 자원 등록 특징**

- 클라이언트는 등록될 리소스의 URI를 모른다.
    - 회원 등록 /members -> POST
    - POST /members
- 서버가 새로 등록된 리소스 URI를 생성해준다.
    - HTTP/1.1 201 Created
    Location: /members/100
- 컬렉션(Collection)
    - 서버가 관리하는 리소스 디렉토리
    - 서버가 리소스의 URI를 생성하고 관리
    - 여기서 컬렉션은 /members

### PUT **기반 등록 API 설계**

**파일 관리 시스템**

- 파일 목록 /files -> GET
- 파일 조회 /files/{filename} -> GET
- 파일 등록 /files/{filename} -> PUT
- 파일 삭제 /files/{filename} -> DELETE
- 파일 대량 등록 /files -> POST

**PUT - 신규 자원 등록 특징**

- 클라이언트는 등록될 리소스의 URI를 모른다.
    - 회원 등록 /members -> POST
    - POST /members
- 서버가 새로 등록된 리소스 URI를 생성해준다.
    - HTTP/1.1 201 Created
    Location: /members/100
- 컬렉션(Collection)
    - 서버가 관리하는 리소스 디렉토리
    - 서버가 리소스의 URI를 생성하고 관리
    - 여기서 컬렉션은 /members

### HTML FORM 사용

- HTML FORM은 GET, POST만 지원
- AJAX 같은 기술을 사용해서 해결 가능
- 컨트롤 URI
    - GET, POST만 지원하므로 제약이 있음
    - 이런 제약을 해결하기 위해 동사로 된 리소스 경로 사용
    - POST의 /new, /edit, /delete가 컨트롤 URI
    - HTTP 메서드로 해결하기 애매한 경우 사용(HTTP API 포함)

- 회원 목록 /members -> GET
- 회원 등록 폼 /members/new -> GET
- 회원 등록 /members/new, /members -> POST
- 회원 조회 /members/{id} -> GET
- 회원 수정 폼 /members/{id}/edit -> GET
- 회원 수정 /members/{id}/edit, /members/{id} -> POST
- 회원 삭제 /members/{id}/delete -> POST
