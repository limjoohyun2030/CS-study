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
<br>



<br>
## REST 의 구성요소
1. 자원 (Resource) URL<br>
모든 자원에 고유한 ID가 존재하고, 이 자원은 Server에 존재합니다.
자원을 구별하는 ID는 /orders/order_id/1 와 같은 HTTP URI 입니다.
<br>

2. 행위 (Verb) - Http Method<br>
HTTP 프로토콜은 GET, POST, PUT, DELETE와 같은 메서드를 제공하는데 이걸 사용합니다.
<br>

3. 표현 (Representaion of Resource)<br>
Client가 자원의 상태 (정보)에 대한 조작을 요청하면, Server는 이에 적절한 응답 (Representation)을 보냅니다
REST에서 하나의 자원은 JSON, XML, TEXT, RSS 등 여러 형태의 Representation으로 나타낼 수 있습니다.
<br>

![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/888b2cf0-44a3-4775-9091-e3951e50de14)

<br>

## REST API 설계 규칙
<br>
지루할테니 빨리 훑고 스킵
<br>
REST에서 가장 중요하며 기본적인 규칙은 아래 두 가지 입니다

URI는 정보의 자원을 표현해야 합니다
자원에 대한 행위는 HTTP Method (GET, POST, PUT, DELETE 등)으로 표현합니다

세부 규칙
1. 슬래시 구분자 ( / )는 계층 관계를 나타내는데 사용한다.

2. URI 마지막 문자로 슬래시 ( / )를 포함하지 않는다.

즉 URI에 포함되는 모든 글자는 리소스의 유일한 식별자로 사용되어야 하며 URI가 다르다는 것은 리소스가 다르다는 것
역으로 리소스가 다르면 URI도 달라져야 한다.
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
클라이언트는 유저와 관련된 처리를, 서버는 REST API를 제공함으로써 각각의 역활이 확실하게 구분되고 일괄적인 인터페이스로 분리되어 작동할 수 있게 한다
REST Server: API를 제공하고 비지니스 로직 처리 및 저장을 책임진다.
Client: 사용자 인증이나 context (세션, 로그인 정보) 등을 직접 관리하고 책임진다.
서로 간 의존성이 줄어든다.
<br>
2.stateless
REST는 HTTP의 특성을 이용하기 떄문에 무상태성을 갖는다.
즉 서버에서 어떤 작업을 하기 위해 상태정보를 기억할 필요가 없고 들어온 요청에 대해 처리만 해주면 되기 때문에 구현이 쉽고 단순해진다.
<br>
3.cache
HTTP라는 기존 웹표준을 사용하는 REST의 특징 덕분에 기본 웹에서 사용하는 인프라를 그대로 사용 가능하다.
대량의 요청을 효율적으로 처리하기 위해 캐시가 요구된다.
캐시 사용을 통해 응답시간이 빨라지고 REST Server 트랜잭션이 발생하지 않기 때문에 전체 응답시간, 성능, 서버의 자원 이용률을 향상 시킬 수 있다.
<br>
4.layered system
클라이언트와 서버가 분리되어 있기 때문에 중간에 프록시 서버, 암호화 계층 등 중간매체를 사용할 수 있어 자유도가 높다
<br>
5.code-on-demand(optional)
?
<br>
6.uniform interface
Uniform Interface는 Http 표준에만 따른다면 모든 플랫폼에서 사용이 가능하며, URI로 지정한 리소스에 대한 조작을 가능하게 하는 아키텍쳐 스타일을 말한다
URI로 지정한 Resource에 대한 조작을 통일되고 한정적인 인터페이스로 수행한다.
즉, 특정 언어나 기술에 종속되지 않는다.

<br>
6-1 identification of resources
리소스가 uri로 식별되야 함
<br>
6-2 manipulation of resources through representations
representations을 통해서 자원을 조작해야 한다.
<br>
6-3 self-descriptive messages
스스로 설명하는 메시지가 되어야 한다.
<br>
6-4 hypermedia as the engin of application state (HATEOAS)
애플리케이션의 상태가 Hyperlink를 통해서 전이되어야 한다.
<br>
특히 self-descriptive messages와 HATEOAS가 안지켜진다고 합니다.
<br>

우리가 부르는 대부분의 "REST API"는 REST를 따르지 않고 있다.
특히 REST의 제약조건인 Self-descriptive와 HATEOAS를 잘 만족시키지 못한다.
REST를 따르겠다면, Self-descriptivedhk와 HATEOAS를 만족시켜야 한다.
Self-descriptive : custom meadia-type이나 profile link relation 등으로 만족시킬 수 있다.
HATEOAS : HTTP 헤더나 본문에 Link를 담아 만족시킬 수 있다.

<br>
추가작성 필요
https://velog.io/@somday/RESTful-API-%EC%9D%B4%EB%9E%80
https://velog.io/@nyong_i/%EB%A1%9C%EC%9D%B4-%ED%95%84%EB%94%A9%EC%9D%B4-%EC%9D%B4-%EA%B8%80%EC%9D%84-%EC%A2%8B%EC%95%84%ED%95%A9%EB%8B%88%EB%8B%A4
<br>

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
