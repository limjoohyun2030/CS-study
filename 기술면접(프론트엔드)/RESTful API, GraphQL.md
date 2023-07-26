# RESTful API 

![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/9eb01b09-6f3f-416d-b209-d417bb12c1a0)

    Restful API 란 Rest(Representational State Transfer) 한 API 인데, 
    자원을 '이름'으로 구분하고, 해당 자원의 '상태' 를 주고 받는 소프트웨어 아키텍쳐입니다.

<br>

## RESTful API 개요?

    HTTP 프로토콜에서 주로 사용하는데 HTTP URL 을 통해 자원을 명시하고, 
    HTTP Method (POST, GET, PUT, DELETE)를 통해 
    해당 자원에 대한 CRUD 를 수행하는걸로 알려져있습니다.

제일 위에 쓴 정의보다 이쪽이 덜 추상적이고 와닿을겁니다.

그러나 HTTP 계의 유명인사 Roy Fielding 에 따르면

본인이 쓴 논문에 CRUD 는 존재하지 않는다고 합니다.

개발자들이 주로 알고있는 RESTful API 는 자원을 URL 로 받고 CRUD 를 하는건데

이건 RESTful 한 방법중 하나인셈입니다.

<br>

## REST API 사용이유?

옛날에는 Client 가 PC Browser를 사용했기 때문에 
Server Side 에서 이에 맞춰서 데이터를 전송하면 됐습니다.

그러나 요즘 시대엔 Web Browser 뿐 아니라 아이폰, 안드로이드폰, 태블릿 등
Client 가 사용하는 플랫폼이 다양해졌습니다

다양한 디바이스들을 고려해서 각각 Server 를 구축하기엔 너무 비효율적인 상황이 돼었습니다.

그래서 Client 쪽 플랫폼이 어떤것인지 고려하지 않고,
범용적인 사용이 가능하도록 Server 를 디자인하는게 중요해졌습니다.

Server Side 에서 XML, JSON 같은 데이터 형식으로 보내고
Client Side 에서 이 데이터를 객체로 바꾸고 UI 쪽으로 그려주는 식으로
Server 와 Client 를 분리해서 개발하는게 중요해졌습니다.




<br>

## REST 의 구성요소
1. 자원 (Resource) URL<br>
모든 자원에 고유한 ID가 존재하고, 이 자원은 Server에 존재합니다.
자원을 구별하는 ID는 /orders/order_id/1 와 같은 HTTP URI 입니다.
<br>

2. 행위 (Verb) - Http Method<br>
HTTP 프로토콜은 GET, POST, PUT, DELETE와 같은 메서드를 제공하는데 이걸 사용합니다.
CREATE READ UPDATE DELETE 이렇게 있을때
UPDATE에 해당하는 메소드는 PUT(전체수정) 과 PATCH(부분수정) 가 있습니다.

```
//백엔드
{
    "topics": // Collection (Element의 모음)
    { // Element (한건 한건의 데이터 )
        "id": 1,
        "title": "REST",
        "body": "REST is..."  
    }
}
```

<br>

```
//프론트

fetch({'topics/1', { // Collection(복수형으로 s 붙임)/Element의 식별자(identifier)
        method:'PATCH', 
        /* PATCH 일땐 title 값만 바뀌지만, 
        PUT 을 하면 identifier 와 바꾸려는 Element를 제외한 Element 가 삭제됩니다.
        이 경우엔 identifier 인 id 와 title은 남지만
        body 가 사라지게 됩니다.
        */
        header: {'content-type':'application/json'},
        body:JSON.stringify({
            title:'fetch - patch'
        })
}
})

```
<br>



4. 표현 (Representaion of Resource)<br>
Client가 자원의 상태 (정보)에 대한 조작을 요청하면, Server는 이에 적절한 응답 (Representation)을 보냅니다
REST에서 하나의 자원은 JSON, XML, TEXT, RSS 등 여러 형태의 Representation으로 나타낼 수 있습니다.
<br>

![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/888b2cf0-44a3-4775-9091-e3951e50de14)

<br>

## REST API 설계 규칙
<br>
REST에서 가장 중요하며 기본적인 규칙은 아래 두 가지 입니다

        URI는 정보의 자원을 표현해야 합니다. 자원에 대한 행위는 HTTP Method (GET, POST, PUT, DELETE 등)으로 표현합니다

### 세부 규칙
1. 슬래시 구분자 ( / )는 계층 관계를 나타내는데 사용한다.

2. URI 마지막 문자로 슬래시 ( / )를 포함하지 않는다.  URI에 포함되는 모든 글자는 리소스의 유일한 식별자로 사용되어야 하며 URI가 다르다는 것은 리소스가 다르다는 것

3. 하이픈 ( - )은 URI 가독성을 높이는데 사용한다.

4. 밑줄 ( _ )은 URI에 사용하지 않는다.

5. URI 경로에는 소문자가 적합하다.
URI 경로에 대문자 사용은 피하도록 한다.

6. 파일확장자는 URI에 포함하지 않는다.
REST API 에서는 메시지 바디 내용의 포맷을 나타내기 위한 파일 확장자를 URI 안에 포함시키지 않는다.
대신 Accept Header 를 사용한다.
ex) GET: http://restapi.exam.com/orders/2/Accept: image/jpg

7. 리소스 간에 연관 관계가 있는 경우
/리소스명/리소스ID/관계가 있는 다른 리소스 명
ex) GET: /users/2/orders (일반적으로 소유의 관계를 표현할 때 사용)


### 노마드 코더가 말한부분
영화 사이트를 예로 들겠습니다.

- URL 에서는 동사를 사용하지 않습니다.
ex) /createmovie , /getmovie 같은 url 은 사용하지 않습니다.
- 대신 명사를 씁니다.
ex) /movie/lalaland
- 여기에 HTTP methods를 결합할수도 있고
ex) GET /movie/lalaland
- 이 뒤에 query parameter 를 추가해서 사용할 수도 있습니다.
ex) GET /movie?min_rating=8.8
이러면 매번 검색할때마나 새로운 URL을 만들지 않아도 됩니다.
![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/92611b41-1413-4463-9c62-d9b7bb5d03fc)

<br>

## REST API 의 탄생배경

Web 이 처음 등장했을때로 돌아가봅시다.

팀 버너스리가 WWW 를 만들고 웹이 탄생했을때는 정보들을 하이퍼텍스트 방식으로 전송했습니다.

사람들이 Web 의 장점을 더 잘 활용하길 바랐던 사람이 있었는데, 그의 이름은 Roy Fielding 입니다

Roy Fielding 은 2000년도에 REST 라는 아키텍쳐를 박사논문으로 발표합니다.

<br>

2000년대 중반에는 CMIS 라는 아키텍쳐도 있었는데, Roy 이건 REST 가 아니라고 했고

2016년에 MS 가 쓴 REST API Guidelines 이 있었는데 

Roy 가 이번에도 이건 REST API 가 아니라 HTTP API 라고 선을 그었습니다.

이러는 이유가 있습니다. 

    REST API 는 아키텍쳐이며, 아키텍쳐는 제약 조건의 집합이기 때문입니다. 
    이 제약들을 지켜야 REST API 라 부를 수 있습니다

<br>

## REST API 의 6가지 제약
<br>
1.client-server
클라이언트는 유저와 관련된 처리를, 서버는 REST API를 제공함으로써 각각의 역할을 확실하게 구분하고, 
일괄적인 인터페이스로 분리시켜 작동할 수 있게 한다. 즉 서로 간 의존성이 줄어듭니다.

REST Server: API를 제공하고 비지니스 로직 처리 및 저장을 책임집니다.
Client: 사용자 인증이나 context (세션, 로그인 정보) 등을 직접 관리하고 책임집니다.

<br>

2.stateless
REST는 HTTP의 특성을 이용하기 떄문에 무상태성을 갖습니다.
서버에서 어떤 작업을 하기 위해 상태정보를 기억할 필요가 없고 
들어온 요청에 대해 처리만 해주면 되기 때문에 구현이 단순해집니다.

<br>

3.cache
HTTP를 사용하기 때문에 기본 웹에서 사용하는 인프라를 그대로 사용합니다.
캐시 사용을 통해 대용량 데이터를 처리하기 때문에
REST Server 트랜잭션이 발생하지 않고 전체 응답시간, 성능, 서버의 자원 이용률을 향상 시킬 수 있습니다.

<br>

4.layered system
클라이언트와 서버가 분리되어 있기 때문에 중간에 
프록시 서버, 암호화 계층 등 중간매체를 사용할 수 있어 자유도가 높습니다

<br>

5.code-on-demand(optional)
서버가 네트워크를 통해 클라이언트에 전달한 javascript 등과 같은 프로그램들은 그 자체로 실행이 될 수 있어야 합니다. 
이것은 사전 구현에 필요한 기능의 수를 줄임으로써 클라이언트를 단순화합니다.
<br>

### 6.uniform interface

Uniform Interface는 Http 표준에만 따른다면 모든 플랫폼에서 사용이 가능하며, 
URI로 지정한 리소스에 대한 조작을 가능하게 하는 아키텍쳐 스타일을 말합니다
특정 언어나 기술에 종속되지 않습니다.

<br>

6-1 identification of resources
URL (Uniform Resource Locator) 으로 내가 어떤 자원을 제어하려고 하는지 알 수 있어야 합니다
주로 Server Side 에서 HTTP body에 JSON 이나 XML 형태로 전송 시킵니다.

<br>

6-2 manipulation of resources through representations
representations을 통해서 자원을 조작해야 합니다.
자원은 다양한 방식으로 표현 가능한데, 자원의 현재 상태에 대해 표현을 할 수 있어야 합니다.
<br>
ex)
```
HTTP/1.0 200 OK
Content-type: text/plain
Content-length: 17

Hello World!
```

URI 를 통해 식별된 자원은 객체와 같고, 이 자원은 상태가 변화할 수 있으므로 
이걸 잘 표현해야 하고, Client 와 Server 이를 통신해야합니다

<br>

6-3 self-descriptive messages
데이터에 대한 메타정보만 가지고도 이게 어떤 종류의 데이터인지, 
이 데이터를 위해서 어떤 어플리케이션을 실행 해야 하는지 설명할 수 있어야 합니다.
<br>
Client 와 Server 사이에는 많은 컴포넌트들(중개자들) 이 있습니다.
이 컴포넌트들이 자원에 대한 요청들을 잘 이해하도록 전송을 해야합니다.

- Host 헤더에 도메인명 기재 필요
```
GET /user/1 HTTP/1.1
host: example.com
```
HTTP/1.1 부터 Host 헤더에 Domain 명이 필수가 되었습니다. 
이 이유 중 하나는 가상호스트 때문인데요, 한개의 IP 에 복수의 Domain 명이 존재할 수 있기 때문에
IP 주소만으로는 요청 대상을 정확히 찾아낼 수 없습니다.

- 캐시
개별 요청에 대한 응답을 항상 Server 에서 하지 않고
Client 와 Server 사이에 있는 컴포넌트가 캐시된 데이터를 Client 에 전달합니다.
HTTP/1.1 에서 Cache-Control, Age, Etag, Vary 등의 헤더들을 메시지에 포함시켜서
캐시관련 동작들을 커스터마이징 가능하게 됐습니다.

데이터 처리를 위한 정보를 얻기 위해서, 데이터 원본을 읽어야 한다면 self-descriptive 적이지 못한겁니다.

<br>

6-4 HATEOAS(hypermedia as the engin of application state)
애플리케이션의 상태가 Hyperlink를 통해서 전이되어야 합니다.
REST는 서로 다른 컴포넌트들을 유연하게 연결을 하는데, 이때 Hyperlink 를 이용합니다.
Client 는 Hyperlink를 이용해서 전체 네트워크와 연결되며
HATEOAS는 서버가 독립적으로 진화할 수 있도록  서버와 클라이언트를 분리 할 수 있게 합니다.
```
<a href="https://~/email">이메일</a>
```

Server 에서 Client 에게 <a href> 태그를 가진 HTML 을 보낸다면 HATEOAS 에 위배되지 않을겁니다.
하지만 일반적으로 백엔드에서는 주로 JSON 형식으로 Client 에게 리소스를 보내고
프론트에선 이 JSON으로 HTML을 구성합니다.
그렇기 때문에 백엔드 쪽에서 설계하는 API는 HATEOAS를 위반하는 경우가 많습니다.


<br>

우리가 부르는 대부분의 "REST API"는 REST를 따르지 않고 있습니다.
특히 REST의 제약조건인 Self-descriptive와 HATEOAS를 잘 만족시키지 못합니다.
Self-descriptive와 HATEOAS 를 아래와 같은 방식으로 만족시킬 순 있습니다.
<br>
Self-descriptive : custom meadia-type이나 profile link relation 등으로 만족시킬 수 있습니다.
HATEOAS : HTTP 헤더나 본문에 Link를 담아 만족시킬 수 있습니다.

```
{
    "links":
    {
        "href":"https://~/email",
        "action":"GET"
    }
}
```
<br>

## REST API를  꼭 따라야할까?

현실적으로 제약조건들을 모두 준수하기 어려운데 꼭 REST API 를 따라야 할까요? 라는 의문이 들 수 있습니다.
Uniform Interface 의 제약조건들은 어플리케이션이 필요한 정보가 아니라
표준화된 형식으로 데이터를 전달이 강요되므로 비효율적일 수 있습니다.

1.REST API 를 철저히 따를 수도 있고
2.REST API 를 부분적으로 준수하는 HTTP API 를 따를 수도 있고
3.다른 API 표준을 따라도 됩니다(e.g., GraphQL API)

# GraphQL

https://www.youtube.com/watch?v=EkWI6Ru8lFQ

<br>

참고링크
<br>
https://velog.io/@somday/RESTful-API-%EC%9D%B4%EB%9E%80
<br>
https://velog.io/@somday/RESTful-API-%EC%9D%B4%EB%9E%80
<br>
https://www.youtube.com/watch?v=PmY3dWcCxXI
<br>
https://www.youtube.com/watch?v=Nxi8Ur89Akw
<br>
https://www.youtube.com/watch?v=4DxHX95Lq2U
<br>
https://velog.io/@nyong_i/%EB%A1%9C%EC%9D%B4-%ED%95%84%EB%94%A9%EC%9D%B4-%EC%9D%B4-%EA%B8%80%EC%9D%84-%EC%A2%8B%EC%95%84%ED%95%A9%EB%8B%88%EB%8B%A4
<br>
https://velog.io/@seokkitdo/Network-REST%EB%9E%80-REST-API%EB%9E%80-RESTful%EC%9D%B4%EB%9E%80
