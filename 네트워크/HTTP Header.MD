# HTTP Header

## HTTP Header의 역할

### Header를 사용하는 이유

- 클라이언트/서버가 요청 또는 응답에 필요한 모든 부가적인 정보를 HTTP헤더를 통해 전송
    
    (메세지 바디의 내용과 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시 관리 정보…)
    

### Header VS Body

- **RFC2616 (과거)**
    - 메시지 본문(mesage body)은 엔티티 본문(entity body)을 전달하는데 사용
    - 엔티티 본문은 요청이나 응답에서 전달할 실제 데이터
    - 엔티티 헤더는 **엔티티 본문의 데이터를 해석할 수 있는 정보** 제공
        
        데이터 유형(html, json), 데이터 길이, 압축 정보 등등
        
- **RFC7230~7235**
    - 메세지 본문(message body)을 통해 표현 데이터 전달
    - 메세지 본문 = 페이로드(payload)
    - **표현**은 요청이나 응답에서 전달할 실제 데이터
    - 표현 헤더는 **표현 데이터를 해석할 수 있는 정보** 제공
        
        데이터 유형(html, json), 데이터 길이, 압축 정보 등등
        

## HTTP Header의 구분

### General Header: 메시지 전체에 적용되는 정보

![general](./HTTP%20Header/general.png)

### Request Header: 요청 정보

![request](./HTTP%20Header/request.png)

### Response Header: 응답 정보

![response](./HTTP%20Header/response.png)

### Representation Header: 표현 정보

Entity Header → Representation Header로 용어가 변경됨

Representation = representation **Metadata** + Representation **Data**

![representation](./HTTP%20Header/representation.png)

### Custom Header (사용자 정의 헤더)

- 과거 Custom Header는 일반적으로 `X-`로 시작하는 것을 권장함
    
    `X-`: 표준화되지 않은 헤더라는 것을 의미
    
    ![AWS S3 REST API.png](./HTTP%20Header/AWS%20S3%20REST%20API.png)
    
    ![NCloud REST API.png](./HTTP%20Header/NCloud%20REST%20API.png)
    
- 최신 HTTP 표준에서는 권장하지 않는 방식
    
    **SHOULD NOT** prefix their parameter names with "X-" or similar constructs. ([RFC6648](https://datatracker.ietf.org/doc/html/rfc6648))
    
- `X-` 없이 ***sensibly***(현명하게) Custom Header를 정의
    
    `X-My-Custom-Header: {CUSTOM_VALUE}` → `My-Custom-Header: {CUSTOM_VALUE}`
    
    ![Tosspayments API.png](./HTTP%20Header/Tosspayments%20API.png)
    

---

## 표현(Representation)

- **Content-Type**: 표현 데이터의 형식
    
    `text/html; charset=utf-8`, `application/json`, `image/jpg`
    
- **Content-Encoding**: 표현 데이터의 압축 방식
    - 데이터를 전달하는 곳: 압축 후 인코딩 헤더 추가
    - 데이터를 읽는 쪽: 인코딩 헤더의 정보로 압축 해제
    - `gzip`, `deflate`, `identity`
- **Content-Language**: 표현 데이터의 자연 언어
    
    `ko`, `en`, `en-US`
    
- **Content-Length**: 표현 데이터의 길이
    - 바이트 단위
    - `Transer-Encoding`(전송 코딩)을 사용하면 `Content-Length`를 사용하면 안됨

## 협상(Content negotiation)

### Content negotiation

**클라이언트가 선호하는 표현** 요청 (협상 헤더는 요청시에만 사용)

- `Accept`: 미디어 타입 전달
- `Accept-Charset`: 문자 인코딩
- `Accept-Encoding`: 압축 인코딩
- `Accept-Language`: 자연 언어

### 협상과 우선순위: **Quality Values(q)**

1. **0~1, 클수록 높은 우선순위, 생략하면 1**
    - Ex) `Acceept-Language; ko-KR, ko;q=0.9, en-US;q=0.8, en;q=0.7`
        
        우선순위: `ko-KR` → `ko` → `en-US` → `en`
        
2. **구체적인 것이 우선한다.**
    - Ex) `Accept: text/*, text/plain, text/plain;format=flowed, */*`
        
        우선순위: `text/plain;format=flowed` → `text/plain` → `text/*` → `*/*`
        
3. **구체적인 것을 기준으로 미디어 타입을 맞춘다.**
    - Ex) `Accept: text/*;q=0.3, text/html;q=0.7, text/html;level=1,`
        
        `text/html;level=2;q=0.4, */*;q=0.5`
        
        | Media Type | Quality |
        | --- | --- |
        | text/html;level=1 | 1 |
        | text/html | 0.7 |
        | text/plain | 0.3 |
        | image/jpeg | 0.5 |
        | text/html;level=2 | 0.4 |
        | text/html;level=3 | 0.7 |

## 전송 방식

- **단순 전송**
    
    Content-Length
    
- **압축 전송**
    
    Content-Encoding
    
- **분할 전송**
    
    Transfer-Encoding
    
- **범위 전송**
    
    Range, Content-Range
    

## 일반 정보

- **From**
    - 유저 에이전트의 이메일 정보
    - 주로 검색 엔진에 사용
    - 요청에서 사용
- **Referer**
    - 이전 웹 페이지 주소 (A→B 이동 시 B에 Referer:A로 요청)
    - 유입 경로 분석 가능
    - 요청에서 사용
- **User-Agent**
    - 클라이언트의 애플리케이션 정보
    - 통계 정보
    - 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
    - 요청에서 사용
- **Server**
    - 요청을 처리하는 **ORIGIN** 서버의 소프트웨어 정보
    - 응답에서 사용
- Date
    - 메세지가 발생한 날짜와 시간
    - 응답에서 사용

## 특별한 정보

- **Host**
    - 요청한 호스트 정보(도메인)
    - **필수 (요청에서 사용)**
    - 하나의 서버가 여러 도메인을 처리해야 할 때
    - 하나의 IP 주소에 여러 도메인이 적용되어 있을 때
- **Location**
    - 페이지 리다이렉션
    - 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동
    - 201 (Created): Location 값은 요청에 의해 생성된 리소스 URI
    - 3xx (Redirection): Location 값은 요청을 자동으로 리디렉션하기 위한 대상 리소스를 가리킴
- **Allow**
    - 허용 가능한 HTTP 메서드
    - 405 (Method Not Allowed)에서 응답에 포함해야함
    - `Allow: GET, HEAD, PUT`
- **Retry-After**
    - 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간
    - 503 (Service Unavailable): 서비스가 언제까지 불능인지 알려줄 수 있음
    - `Retry-After: Fri, 31 Dec 1999 23:59:59 GMT` (날짜 표기)
    - `Retry-After: 120` (초단위 표기)

## 인증

### Authorization

클라이언트 인증 정보를 서버에 전달

- `Authorization: Basic xxxxxxxxxx`

### WWW-Authenticate

리소스 접근시 필요한 인증 방법 정의

- 401 Unathorized 응답과 함께 사용
- `WWW-Authenticate: Newauth realm=”apps”, type=1, title=”Login to \"apps\””, Basic realm=”simple”`

## 쿠키

[https://github.com/limjoohyun2030/CS-study/blob/main/WEB/Cookie And Session.MD](https://github.com/limjoohyun2030/CS-study/blob/main/WEB/Cookie%20And%20Session.MD)

## 캐시

[https://github.com/limjoohyun2030/CS-study/blob/main/운영체제/Cache.MD](https://github.com/limjoohyun2030/CS-study/blob/main/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C/Cache.MD)

---

## 참고

- [모든 개발자를 위한 HTTP 웹 기본 지식](https://inf.run/vyK5)
- [HTTP 헤더로 에러 테스트하기](https://blog.tossbusiness.com/articles/dev-7)
