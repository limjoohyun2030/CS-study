![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/0f8b4222-ede9-434b-b4e0-f04c2dff5264)
## Rendering 

![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/1b59e269-f54b-4e6f-b594-c962e7a5d609)

<ol>
  <li>
    Browser 가 url을 입력받으면, DNS를 통해 ip주소로 변환하고, 해당 ip 주소를 가진 서버에게 Resources 를 요청
    <br>
<br>
  </li>
  <li>
    Webkit, Blink 같은 Browser Rendering Engine 이 Server로부터 받은 HTML, CSS 를 Parsing 
    <br>
<br>
  </li>
  <li>
    HTML Parser가 HTML을 Parsing 해서 DOM(Document Objecy Model) tree 를 만듬
    <br>
<br>
  </li>
  <li>
    CSS Parser가 CSS를 Parsing 해서 CSSOM(CSS Object Model) tree 를 만듬
    <br>
    <br>
HTML,CSS 모두 서버로부터 바이트 형태로 전달을 받습니다. 
이 바이트 형태를 문자열로, 문자열을 토큰으로, 토큰을 노드로 바꾸고, 
이 노드들을 DOM tree, CSSOM tree로 만들고, 이 두개의 tree를 합쳐서 Render tree로 만듭니다.
<br><br>
이때 알아야할 점은, 화면에 렌더링되는 노드들만 구성이 된다는 점인데
화면에 렌더링 되지않은 display:none 이나 meta 태그등은 렌더 트리에 포함되지 않습니다
<br><br>
  </li>
  
  <li>
    HTML Parser가 Parsing 할때 link 나 style 같은 CSS 태그를 만나면 블로킹되고, CSSOM 생성쪽으로 넘어감
    CSSOM 생성이 마무리되면, 블로킹 되었던 시점부터 다시 HTML Parsing을 진행합니다.
    <br>
<br>
  </li>
  <li>
    HTML Parser가 Parsing 할때 script 태그를 만나면 제어권한을 JS Runtime Engine으로 넘김. 
    <br>
    JS Engine은 JS를 Parsing 해서 AST(Abstract syntax tree)를 만듭니다.
    AST는 JS에만 적용되는 단어는 아닙니다. 특정 프로그래밍 언어로 작성된 프로그램 소스를
의미별로 분리해서 컴퓨터가 이해할 수 있는 구조로 변경시킨 트리입니다.

![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/b9fcb6c7-54e9-4247-b99c-4f1881fc936a)

이런식으로 컴퓨터가 이해할 수 있도록 바꾼다는데...자세히 파고드는건 다음 기회로..
<br>
<br>
아무튼, JS를 Parsing 해서 AST를 만들면, Interpreter 가 AST를 기반으로, 
Virtual Machine이 이해할 수 있는 ByteCode를 만듭니다.

<br>
<br>
  </li>
  <li>
    Script 태그는 동기적 파싱. 해결법은 async defer?
<br>
<br>
Script 태그는 동기적으로, 즉 위에서 아래로 읽어내려가면서 Parsing 을 하기 때문에
script 태그의 위치에 따라 HTML Parsing 이 지연될 수 있습니다.
그래서 script 태그는 가능하면 body 태그의 맨 아래쪽에 위치하는게 좋다고 생각할 수 있습니다.
<br>
<br>

그러나 script 태그를 body 태그 맨 아래에 두어도 문제가 있습니다.
이미 화면상에 보이는 HTML 태그에서 만든 컴포넌트들의 상호작용이 작동하지 않을 수 있습니다(버튼 클릭, 텍스트입력등)

async, defer 속성을 이용해서 JS코드를 비동기적으로 불러옴으로서, DOM 렌더링의 블로킹을 방지할 수 있습니다.
즉, 렌더링을 막지않고  스크립트를 실행하기 때문에 사용자 경험이 좋아지게 됩니다.
async 와 defer 는 차이점이 있습니다

<br>

- defer
  
      defer 는 script 태그가 여러개 있다면 순차적으로 실행됩니다.
      defer 는 DOMContentLoaded 라는 이벤트가 발생하기 전에 실행됩니다
<br>
예를들어

<script defer ...a.js></script>
<script defer ...b.js></script>

이렇게 있다면 a.js 다음에 b.js 가 실행됩니다.
그리고 아래와 같은 코드가 봅시다

<script>
  document.addEventListener('DOMContentLoaded', () => alert('hi'));
</script>
<script defer ...c.js></script>

DOMConetentLoaded 이벤트가 실행되면 hi 를 alert 창으로 띄우는 코드입니다.
말 그대로 DOM content 들이 모두 로드가 되고 나서 진행이 되는데,
defer 는 DOMConetentLoaded 이전에 실행되기 때문에
alert hi 보다  c.js 가 먼저 실행이 됩니다.


<br>

- async

      async 는 defer 와 달리 독립적으로 실행됩니다.
순차적으로 실행되지도 않고, DOMConetentLoaded 의 영향을 받지도 않습니다.

<script async ...a.js></script>
<script async ...b.js></script>

앞에서 본 것과 같은 코드가 있다고 할때
defer 라면 a.js 이후에 b.js 가 실행되지만
async 를 쓰면 둘 중 먼저 다운로드 된 js 가 실행됩니다.

<br>
이러한 특징 때문에
defer 는 초기 페이지의 렌더링 과정에서 꼭 필요한 처리지만, 무거운 작업일 경우 사용하고, 
async는 페이지와 관련없이 독립적인 기능을 하기 위해 사용하는 것이 좋습니다.(Ex. 광고 클릭 카운트)
<br>
<br>
  </li>

  <li>
    DOM tree 와 CSSOM tree 를 합쳐서 Render tree 를 만듦
    <br>
    <br>
    Render tree를 기반으로 
- HTML 요소의 Layout(컴포넌트들의 위치와 크기)을 계산하고
- Browser 화면에 HTML 요소를 Painting 합니다.
  
JS에 의해 노드가 추가되거나, Browser 창이 Resizing 된다면
reflow 와 repaint 이 발생합니다.

<br><br>
  </li>
  
  <li>
    reflow, repaint
    <br><br>
      - reflow
  
생성된 DOM 노드의 레이아웃 변경 시 영향을 받는 모든 노드(부모, 자식)의 수치를 다시 계산하여 레이아웃 트리(렌더 트리)를 재생성하는 작업
width, height, padding, margin 등 레이아웃에 영향을 주는 속성들을 변경하면 발생합니다.

- repaint

 reflow 과정이 끝나고 재생성된 레이아웃 트리(렌더 트리)를 다시 레이어에 그리는 작업
 color, border-radius, background, box-shadow 등등, 레이아웃에는 영향을 안 주지만, 시각적인게 바뀔때 발생

즉, reflow 는 틀을 그린다고 생각하고, repaint 는 말 그대로 구조를 바꾸는게 아니라 
물감으로 덧칠하는 부분이라고 생각하면 될 것 같습니다.


reflow 와 repaint 를 알아야하는 이유는, 이 두 요소가 렌더링 속도에 영향을 주기 때문입니다.
두 단어 앞에 're' 라는 단어가 붙혀진 이유는 다시 그려준다는 의미인데
레이아웃이 변경될때마다, 시각적인게 변경될때마다 많은 변화가 일어난다면 그만큼
브라우저 렌더링 속도가 떨어져서 사용자 경험이 안 좋아지게 됩니다.

- 해결책?

1. CSS Transform 속성을 사용하면 CPU 가 아닌 GPU 로 렌더링 하기 때문에, 애니메이션 처리를 빠르게 가능
2. JS requestAnimationFrame 함수를 사용
    <br><br>
  </li>
</ol>


## Protocol

![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/0b2d9d89-cbb2-4738-896b-5bfed858794e)


Protocol 이란 통신규약 이라는 의미를 가지며, 
컴퓨터나 전자기기 사이에 메세지를 주고 받는 양식과 규칙의 체계를 의미입니다.

웹 통신 쪽에서는 HTTP 를 많이 쓰지만, Protocol 의 종류에는 여러가지가 있습니다.

- FTP(File Transfer Protocol) : 컴퓨터 사이에 파일 전송을 하기 위한 프로토콜
- SMTP(Simple Mail Tranfer Protocol) : 전자 우편 전송 프로토콜
- DHCP(Dynamic Host Configuration Protocol) : 클라이언트가 동적인 IP 주소를 할당 받아 인터넷을 사용하도록 도와주는 프로토콜

### TCP(Transmission Control Protocol) , UDP(User Datagram Protocol)
![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/4c06be5d-6aeb-416b-a73e-0fe8eba27b3c)

<br>
OSI 7 레이어에서 Transport Layer에는 양 끝단(End to end)의 사용자들이 신뢰성있는 데이터를 주고 받을 수 있도록 해 주어, 상위 계층들이 데이터 전달의 유효성이나 효율성을 생각하지 않도록 해줍니다
TCP UDP 모두 Transport Layer(전송 프로토콜) 인데 Application 프로세스들 간의 논리적인 통신을 제공하는 Layer 입니다.

- TCP
  
        인터넷상에서 데이터를 메세지의 형태로 보내기 위해 IP와 함께 사용하는 프로토콜
  TCP는 연결형 서비스로, 신뢰적인 전송을 보장하기에 hanshaking하고 데이터의 흐름제어와 혼잡제어를 수행하지만 속도가 느립니다.
  <br>
  
- UDP
  
        데이터를 데이터그램 단위로 처리하는 프로토콜

UDP는 비연결형 프로토콜입니다. 할당되는 논리적인 경로가 없고 각각의 패킷이 다른 경로로 전송되고 이 각각의 패킷은 독립적인 관계를 지니게 됩니다. 네트워크 부하가 적다는 장점이 있지만 데이터 전송의 신뢰성이 낮기 때문에, 연속성이 중요한 실시간 서비스(스트리밍)에 좋다고 합니다.


![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/fed51ba5-6709-4711-9e66-1fe412a0cea1)

(추상적인 설명들이 많으므로 차이점을 느낌으로 기억합시다)

<br>

### 3-Way Handshake와 4-Way Handshake
        3-Way Handshake 는 TCP의 접속,4-Way Handshake는 TCP의 접속 해제 과정입니다.

Handshake와 관련된 많은 복잡한 용어들이 나오는데, 대략적인 느낌만 먼저 이해합시다.
3-Way Handshake , 4-Way Handshake 는 연결하고자 하는 두 장치끼리
연결이 잘 됐는지 확인하는 과정이라고 이해하면 됩니다.

![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/9ce4a2c9-5091-4dff-85d6-82cac90c5332)

        A -> B : 안녕! 거기 있니?
        B -> A : 응 안녕! 나 여기있어. 내 말 들려?
        A -> B : 웅 잘 들려!

관련 용어
- CLOSED	연결 수립을 시작하기 전의 기본 상태 (연결 없음)
- LISTEN	포트가 열린 상태로 연결 요청 대기 중
- SYN-SENT	SYN 요청을 한 상태
- SYN-RECEIVED	SYN 요청을 받고 상대방의 응답을 기다리는 중
- ESTABLISEHD	연결의 수립이 완료된 상태, 서로 데이터를 교환할 수 있다.

![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/348b7b8c-b012-4d00-aab4-9761a1a13eaa)


        A -> B : 이제 통화 끊고 내일 보자~
        B -> A : 알겠어. 잠시만 기다려 
        B -> A : 그래 이제 끊을게!
        A -> B : 그래 내일 봐 

관련 용어
- CLOSE	연결 수립을 시작하기 전의 기본 상태 (연결 없음)
- ESTABLISHED	연결의 수립이 완료된 상태, 서로 데이터를 교환할 수 있다.
- CLOSE-WAIT	상대방의 FIN(종료 요청)을 받은 상태. 상대방 FIN에 대한 ACK를 보내고 애플리케이션에 종료를 알린다.
- LAST-ACK	CLOSE-WAIT 상태를 처리 후 자신의 FIN요청을 보낸 후 FIN에 대한 ACK를 기다리는 상태.
- FIN-WAIT-1	자신이 보낸 FIN에 대한 ACK를 기다리거나 상대방의 FIN을 기다린다.
- FIN-WAIT-2	자신이 보낸 FIN에 대한 ACK를 받았고 상대방의 FIN을 기다린다.
- CLOSING	상대방의 FIN에 ACK를 보냈지만 자신의 FIN에 대한 ACK를 못받은 상태
- TIME-WAIT	모든 FIN에 대한 ACK를 받고 연결 종료가 완료된 상태. 새 연결과 겹치지 않도록 일정 시간 동안 기다린 후 CLOSED로 전이한다.

<br>

### 암호화 기반 프로토콜 (두 프로토콜 모두 클라이언트(브라우저)와 서버(웹서버)가 보안이 향상된 통신 프로토콜)
![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/846d609e-63ab-4fd3-af20-fb8fd1987333)

- SSL(Secure Socket Layer) : 개인정보 보호, 인증, 데이터 무결성을 보장.
  SSL 인증은 SSL 인증서가 있는 웹사이트에서만 가능. 공개 키로 클라이언트, 웹서버 모두 암호화 가능
  대표적으로 HTTPS 는 HTTP 와 SSL 을 결합한 프로토콜
- TLS(Transport Layer Security) : SSL 이후에 나온 버전이지만 SSL 이 더 많이 쓰여서 SSL 과 묶여서 표현함

참고) SSL 도 handshake 를 하는데, SSL은 TCP 기반 프로토콜이므로 TCP 3-way handshake 를 수행하고 나서 SSL handshake 를 수행합니다.

### HTTP(Hyper Text Transfer Protocol)

        웹상에서 HTML 문서나 동영상과 같은 리소스(Resource)를 주고받기 위한 프로토콜

![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/6ae8cb54-b920-41e1-9719-9a78ea6fd242)
        
HTTP 는 Client-Server Protocol Model 을 기반으로 하는데, 
이는 서비스 제공자(서버)와 서비스 요청자(클라이언트)를 구분하는 Network model 입니다.

![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/d415fe8b-b70b-4cb1-8ff3-acc62f99bda3)




<br>

## RESTful API

<br>

## Get, Post

<br>

## 주소창에 www.google.com 을 입력하면?
마지막으로 연결해서 설명
https://velog.io/@soyeon9819/%EC%9B%B9%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%97%90-www.google.com%EC%9D%84-%EC%B9%98%EB%A9%B4-%EC%9D%BC%EC%96%B4%EB%82%98%EB%8A%94-%EC%9D%BC
<br>
<br>


## 아직 참고 못한 링크
https://velog.io/@chappi/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%A5%BC-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90-30%EC%9D%BC%EC%B0%A8-%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-%EB%A0%8C%EB%8D%94%EB%A7%81-%EA%B3%BC%EC%A0%95


## 참고링크
- https://velog.io/@uncyclocity/JavaScript-%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%9D%98-%EB%A0%8C%EB%8D%94%EB%A7%81-%EA%B3%BC%EC%A0%95
- https://prod.velog.io/@championcom/CSSOM-AST
- https://gyujincho.github.io/2018-06-19/AST-for-JS-devlopers
- https://velog.io/@soulee__/Javascript-%EB%A6%AC%ED%8E%98%EC%9D%B8%ED%8A%B8-%EB%A6%AC%ED%94%8C%EB%A1%9C%EC%9A%B0
- https://velog.io/@fepanbr/HTML-script%ED%83%9C%EA%B7%B8-defer-async
- https://velog.io/@moonsun116/async-vs-defer
- https://velog.io/@soulee__/Javascript-%EB%A6%AC%ED%8E%98%EC%9D%B8%ED%8A%B8-%EB%A6%AC%ED%94%8C%EB%A1%9C%EC%9A%B0
- https://velog.io/@dyunge_100/WEB-HTTP-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C
- https://velog.io/@yjw8459/%EC%9B%B9-%EC%82%AC%EC%9D%B4%ED%8A%B8-%EB%B3%B4%EC%95%88-SSL%EC%9D%B4%EB%9E%80
- https://velog.io/@averycode/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCPUDP%EC%99%80-3-Way-Handshake4-Way-Handshake
- https://seongonion.tistory.com/74
