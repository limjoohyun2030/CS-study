# WebSocket
![Untitled](https://github.com/limjoohyun2030/CS-study/assets/39722436/7e129d28-7ebd-4ebe-92d0-cf2533bbbed3)

## WebSocket 이란?

클라이언트의 요청과 서버의 응답이 끝나면 연결이 종료되는(**Connectionless**) HTTP 통신과 달리

응답 이후에도 Connection이 계속 유지되도록 만든 프로토콜. 이를 WebSocket 이라고 합니다. 

웹 환경에서 널리 쓰이고 있으며, 브라우저에서 웹소켓 프로토콜을 지원합니다.

## WebSocket 의 특징

1. 양 방향 통신 (Full-Duplex)
    
    클라이언트가 요청을 해야 서버가 응답을 하는 기존의 HTTP 방식과 달리
    
    WebSocket 은 서로가 원할때 데이터를 주고 받을 수 있습니다
    
2. 실시간 네트워킹(Real Time-Networking)
    
    채팅, 주식, 비디오 같이 연속된 데이터를 일방적으로 빠르게 여러 단말기에 뿌려줄 수 있습니다.
    
3. 최초 접속은 일반 HTTP 로
    
    WebSocket 이 기존의 일반 TCP Socket과 다른 점은, 
    
    최초 접속이 일반 HTTP Request를 통해 이뤄지기 때문에, 
    
    기존의 http 80, https 443 포트로 접속이 가능합니다. 
    
    최초 접속에만 HTTP 프로토콜 위에서 Handshaking을 하기 때문에 HTTP Header를 사용합니다.
    
    - HTTP : 헤더의 비중이 너무 큼. 실시간으로 많은 데이터를 주고 받을땐 매우 부담이 되는 요소 중 하나
    - WebSocket : 핸드쉐이크 과정에서는 헤더의 비중이 크지만, 한번 연결이 되면 간단한 메시지들만 오고 가므로 효율적임
    
    추가적으로 방화벽을 열어주지 않고도 양방향 통신이 가능하며 
    
    HTTP 규격인 CORS 적용이나 인증 등의 과정을 기존과 동일하게 가져갈 수 있습니다
    

## HTTP 의 실시간 통신 방식

WebSocket 에서만 실시간 통신이 가능한건 아닙니다. 

HTTP 에서도 아래와 같은 방식으로 가능합니다.

1. polling
    
    ![Untitled 1](https://github.com/limjoohyun2030/CS-study/assets/39722436/6e5a03dd-378c-4964-ace6-f2c4eadba3b7)
    
    일정 주기로 서버에 HTTP 요청을 보내는 방식
    
    real time 통신에서는 언제 통신이 발생할지 예측 불가능 함.
    
    불필요한 요청과 connection 생성으로 서버에 과부하 발생.
    
2. Long polling
    
    ![Untitled 2](https://github.com/limjoohyun2030/CS-study/assets/39722436/1a41929c-8f40-4233-a908-a9d9d5048a6e)

    클라이언트에서 서버로 일단 HTTP Request를 요청하면 
    
    서버에서 해당 클라이언트로 전달할 이벤트가 있을 때까지 대기 상태로 있다가 
    
    이벤트 발생 시 Response 메시지 전송 후 연결이 종료하는 방식
    
    일반 polling 보다는 서버의 부담이 줄어들지만 
    
    업데이트 주기가 짧다면 polling 과 별 차이가 없게 됨
    
3. streaming

    ![Untitled 3](https://github.com/limjoohyun2030/CS-study/assets/39722436/c6d299eb-3029-42ae-9145-742c9d579010)
    
    클라이언트에서 서버로 일단 HTTP Request를 보낸 후 
    
    서버에서 끊임없이 데이터를 보내는 방식.
    
    클라이언트에서 서버로의 데이터 송신이 어려움
    

## WebSocket 접속방법 - Hand Shaking

![Untitled 4](https://github.com/limjoohyun2030/CS-study/assets/39722436/cb5bae90-12d4-4c78-a2bd-d2cdfbcddb61)

웹소켓도 TCP/IP 위에서 동작하기 때문에

서버와 클라이언트는 TCP/IP 접속이 되어있어야 합니다.

### 웹소켓 열기 HandShake

웹소켓 열기 HandShake는 클라이언트가 먼저 HandShake 요청을 보내면 

서버가 이에 대한 응답을 보내는 구조입니다.

서버와 클라이언트는 HTTP 1.1 프로토콜을 사용하여 요청과 응답을 보냅니다.

ex) **HandShake Request**

```jsx
GET /chat HTTP/1.1
Host: server.gorany.org
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Origin: http://localhost:8080
Sec-WebSocket-Protocol: v10.stomp, v11.stomp, my-team-custom
Sec-WebSocket-Version: 13
```

| Header Name | div | Description |
| --- | --- | --- |
| GET | Require | 요청 명령어는 GET을 사용해야 하며, HTTP 버전은 1.1 이상이어야 한다. |
| Host | Require | 웹소켓 서버의 주소 |
| Upgrade | Require | WebSocket이라는 단어를 사용해야 한다. 대소문자는 구분 X |
| Connection | Require | HTTP 사용 방식을 변경한다는 의미. Upgrade라는 단어를 사용해야 한다. 대소문자는 구분 X |
| Sec-WebSocket-Key | Require | 보안을 위한 요청 키. 길이가 16Byte인 임의로 선택 된 숫자를 base64 인코딩한 값 이다. |
| Origin | Require | 클라이언트로 웹 브라우저를 사용하는 경우 필수항목으로, 클라이언트의 주소 |
| Sec-WebSocket-Version | Require | 13을 사용한다. |
| Sec-WebSocket-Protocol | Option | 클라이언트가 사용하고 싶은 하위 프로토콜 이름을 명시한다. |
| Sec-WebSocket-Extensions | Option | 클라이언트가 사용하고 싶은 추가 옵션을 기술한다. |

ex) **HandShake Response**

```jsx
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=
```

| Header Name | div | Description |
| --- | --- | --- |
| HTTP | Require | HTTP 버전은 1.1이며, 클라이언트로부터의 요청이 이상 없는 경우 101을 상태코드로 사용한다. |
| Upgrade | Require | WebSocket이라는 단어를 사용해야 한다. 대소문자는 구분 X |
| Connection | Require | Upgrade라는 단어를 사용해야 한다. 대소문자는 구분 X |
| Sec-WebSocket-Accept | Require | 클라이언트로부터 받은 Sec-WebSocket-Key를 사용하여 계산된 값이다. 이 값이 클라이언트에서 계산한 값과 일치하지 않으면 연결 수립 x |
| Sec-WebSocket-Protocol | Option | 서버에서 서비스하는 하위 프로토콜을 명시한다. 클라이언트가 요청하지 않는 하위 프로토콜을 명시하면 HandShake는 실패한다. |
| Sec-WebSocket-Extensions | Option | 서버가 사용하는 추가 옵션을 기술한다. 클라이언트가 요청하지 않는 추가 옵션을 명시하면 HandShake는 실패한다. |

### Handshake 완료 (웹 소켓이 열리고, 데이터 전송 시작)

[data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==](data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==)

Handshake 가 끝난 시점부터 서버와 클라이언트는 서로가 살아 있는지 확인하기 위해서 

heartbeat 패킷을 보내며 주기적으로 ping 을 보내 체크합니다. 

이는 서버와 클라이언트 양측에서 설정이 가능합니다. (이를 health check 라고 합니다)

- 프로토콜이 WS로 바뀝니다. WSS 는 보안을 위해 SSL 을 적용한 프로토콜
    
    (Client Web Socket) WS(80) or WSS(443) → WS or WSS (Server WebSocket)
    (Client Web Socket) WS or WSS ← WS or WSS (Server WebSocket)
    

[data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==](data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==)

- 데이터를 주고 받는데 있어서 단위는 Message를 사용합니다.
    
    Message를 웹소켓 프레임이라고 한다.
    
- Message
    
    여러 frame이 모여서 구성하는 하나의 논리적 메시지 단위
    
    Message에 포함될 수 있는 교환 가능한 Message는 text와 binary
    
- frame
    
    communication 에서 가장 작은 단위의 데이터
    
    작은 헤더 + payload로 구성
    
    웹소켓 통신에 사용되는 데이터는 UTF-8 인코딩으로만 지원됨.
    
    ex) 0x00(보내고 싶은 데이터)0xff
    

## Frame (프레임) 헤더 구조

![Untitled 5](https://github.com/limjoohyun2030/CS-study/assets/39722436/e294c5ca-4a60-4320-b483-e21c0645fdc2)


- FIN
이 프레임의 전체 메시지의 끝임을 나타내는 플래그
- OPCODE
Continue ( 0x0 ) : 전체 메시지의 일부
Text ( 0x1 ) : 포함 데이터가 UTF-8 텍스트라는 의미
Binary (0x2) : 포함된 데이터가 이진 데이터라는 의미
close (0x8) : Close 핸드쉐이크를 시작한다는 의미
- LENGTH
이 프레임에 포함된 데이터의 총 길이를 나타낸다.
- RSV 1 ~ 3
프로토콜 별로 사용할 수도 있고 사용할 수도 있지 않은 것들
- 연결 종료
연결 종료 할 때는 Close Frame을 주고 받으며 종료한다.

## ****Socket.io, SockJS, STOMP****

WebSocket은 HTML5 이후에 나온 기술이며, 그 이전에 나온 서비스에서도 Websocket 처럼 사용할 수 있도록 도와주는 기술들이 있습니다.

polling, FlashSocket 등의 기술들을 하나의 API 추상화 한 것들입니다.

### Socket.io, SockJS

[data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==](data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==)

- HTML5 이전 기술로 구현된 서비스에서 웹 소켓처럼 사용할 수 있도록 도와주는 기술
- Javascript를 이용하여 브라우저 종류에 상관없이 실시간 웹을 구현
- 브라우저와 웹 서버의 종류와 버전을 파악하여 가장 적합한 기술을 선택하여 사용하는 방식

### STOMP

[data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==](data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==)

- WS는 형식이 없기 때문에 해석이 어려운데, 따라서 WS 방식은 Sub Protocol을 사용해서 주고 받는 메시지의 형태를 약속합니다.
- 이러한 Sub Protocol 중 하나가 STOMP(Simple Text Oriented Message Protocol) 입니다.
- 문자열들을 주고 받을 수 있게 해줄 뿐 그 이상의 일은 하지 않습니다.
- 주고 받은 문자열의 해독은 애플리케이션이 합니다.

## 주의할 점

![Uploading Untitled 6.png…]()


```jsx
WebScoket 프로토콜은 HTTP 프로토콜처럼 **애플리케이션 계층(Application Layer)에 위치**하며, 
TCP의 양방향 전이중 통신을 사용하기 때문에 **전송 계층(Transport Layer)에 의존**하고 있다고 볼 수 있습니다.

WebScoket 은 HTTP 레이어에서 작동하는 소켓으로, TCP/IP 소켓의 레이어가 다릅니다.
```

참고링크

[https://www.youtube.com/watch?v=LddPLO4bXmQ](https://www.youtube.com/watch?v=LddPLO4bXmQ)

[https://www.youtube.com/watch?v=5EhsjtBE7I4&t=3s](https://www.youtube.com/watch?v=5EhsjtBE7I4&t=3s)

[https://www.youtube.com/watch?v=rvss-_t6gzg](https://www.youtube.com/watch?v=rvss-_t6gzg)

[https://www.youtube.com/watch?v=yXPCg5eupGM](https://www.youtube.com/watch?v=yXPCg5eupGM)

[https://velog.io/@qwd101/WebSocket-공부](https://velog.io/@qwd101/WebSocket-%EA%B3%B5%EB%B6%80)

[https://www.youtube.com/watch?v=MPQHvwPxDUw](https://www.youtube.com/watch?v=MPQHvwPxDUw)

[https://hyena.oopy.io/3ac67291-8cfa-4eb4-90ed-f91460e2bce9](https://hyena.oopy.io/3ac67291-8cfa-4eb4-90ed-f91460e2bce9)

[https://velog.io/@rhdmstj17/소켓과-웹소켓-한-번에-정리-2](https://velog.io/@rhdmstj17/%EC%86%8C%EC%BC%93%EA%B3%BC-%EC%9B%B9%EC%86%8C%EC%BC%93-%ED%95%9C-%EB%B2%88%EC%97%90-%EC%A0%95%EB%A6%AC-2)
