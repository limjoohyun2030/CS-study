# CORS
## **CORS(Cross-Origin Resource Sharing)란?**

- “**다른-출처 자원 공유”** 권한을 부여하도록 브라우저에 알려주는 **HTTP-header 기반** 메커니즘
- 한 출처에서 실행 중인 웹 애플리케이션이 **다른출처의 리소스에 접근할 수 있는 권한을 부여**하도록 브라주저에 알려주는 체제 → “다른 출처라도 요청을 보내게 해줄게”
- Resource가 자신의 **Origin(출처)**과 다를 때 **cross-origin HTTP requests**를 실행

### 왜 **다른 출처 자원 공유가 안될까?** SOP(Same-Origin Policy)

- 웹 브라우저에서 동작하는 프로그램은 **로딩된 위치에 있는 리소스만 접근 할 수 있다는 정책** → “같은 출처끼리만 요청을 보낼 수 있어”
- 악성 웹사이트가 브라우저에서 JS를 실행하여 제3자 서비스에서 데이터를 읽는 것을 방지

### 출처(Origin)가 뭔데?

Origin : `Protocol` + `Host` + `Port`

- 서버의 위치를 찾아가기 위해 필요한 가장 기본적인 요소들
- 출처 내의 포트 번호는 **`HTTP`**, **`HTTPS`** 등 기본 포트번호가 정해져 있는 프로토콜을 사용하는 경우 생략 가능
- 출처에 포트 번호가 명시적으로 포함되어 있다면 이 포트 번호까지 모두 일치해야 같은 출처라고 인정됨

## CORS 동작원리

1. Request: `Origin` Header를 통해 요청을 보내는 출처를 밝힌다.
2. Respone: `Access-Control-Allow-Origin` Header를 통해 이 리소스에 접근이 허용된 출처들을 알려준다.
3. 브라우저가 `Origin` 헤더와 `Access-Control-Allow-Origin` 헤더를 비교하여 유효한 응답인지 결정한다.

## CORS 시나리오

### Simple Request

CORS preflight 를 트리거하지 않는 요청이며 조건이 까다로워 거의 마주치기 어렵다.

- 요청 메서드는 `GET`, `HEAD`, `POST` 중 하나
- user agent가 자동으로 설정하는 헤더를 제외하고, [Fetch 명세에서 "CORS-safelisted request-header"로 정의한 헤더](https://fetch.spec.whatwg.org/#cors-safelisted-request-header)만 사용(`Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, `Range`)
- `Content-Type` 헤더를 사용하는 경우 `application/x-www-form-urlencoded`, `multipart/form-data`, `text/plain`만 허용

### Preflight Request

- 쿠키나 세션같은 정보가 없는 Request에 대해 동작
- `OPTIONS` Method를 통해 동작

### Credentialed Request

- `OPTIONS` Method를 통해 동작
- 쿠키나 세션같은 정보가 담긴 Request에 대해 동작
- `Access-Control-Allow-Origin: "*"` 불가능
- `Access-Control-Allow-Credentials: true` 필수

---

### 출처

- https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS
- https://evan-moon.github.io/2020/05/21/about-cors/
- [[10분 테코톡] 성하의 CORS](https://youtu.be/4iXWkBi2hCA?feature=shared)

---
