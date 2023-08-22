# 3-Way, 4-Way Handshake(TCP 연결 설정과 해제)

## Background

기본적인 TCP의 개념과 정의를 알고 있는 상태라고 가정하고 설명한다.  

### TCP 연결 상태(connection status)

- `CLOSED`: 포트가 닫힌 상태
- `LISTEN`: 포트가 열린 상태로 연결 요청 대기 상태
- `SYN_RCV`: SYNC 요청을 받고 상대방의 응답 대기 상태
- `ESTABLISHED`: 포트 연결 상태

### TCP 플래그(Flag)

- `SYN`: 연결 설정 (Synchronize Sequence Number)
- `ACK`: 응답 확인 (Acknowledgement)
- `FIN`: 연결 해제 (Finish)

### TCP 세그먼트(Segment)

TCP에서의 패킷으로, 전송할 데이터 바이트와 TCP에 의해 데이터에 추가되는 헤더로 구성된다.

---

## 3-Way Handshake

네트워크 프로토콜 TCP(Transmission Control Protocol)에서 사용되는 연결 설정 방법

데이터를 주고받을 수 있는 두 호스트 간의 연결을 설정하는 과정

### **The Three Steps of a 3-Way Handshake**

**Step 1**: 클라이언트가 서버에 연결 요청 (SYN)

- 클라이언트는 서버에 연결하고자 `SYN`(Synchronize) 패킷 송신
- 패킷에 클라이언트의 초기 시퀀스 번호를 포함

**Step 2**: 서버가 클라이언트 요청에 응답 (SYN-ACK)

- 서버는 클라이언트의 `SYN` 패킷을 수신
- 클라이언트에 대한 응답으로 `SYN-ACK`(Synchronize-Acknowledgment) 패킷 송신
- 패킷에 서버의 초기 시퀀스 번호와 클라이언트의 시퀀스 번호에 1을 더한 값을 포함

**Step 3**: 클라이언트가 서버에 응답 (ACK)

- 클라이언트는 서버로부터 받은 `SYN-ACK` 패킷에 대해 `ACK`(Acknowledgment) 패킷 송신
- 패킷에 서버의 시퀀스 번호에 1을 더한 값을 포함  

![TCP-Connection-Establishment.png](./3-Way,%204-Way%20Handshake/establishment.png)

http://www.tcpipguide.com/free/t_TCPConnectionEstablishmentProcessTheThreeWayHandsh-3.htm

### Why does TCP use 3-way handshake?

- TCP는 연결 지향적 프로토콜로 안정적인 데이터 전송을 위해 신뢰성을 확보하는데 중점을 둠
- TCP는 양방향 통신이기 때문에 양쪽 모두 상대방에게 패킷을 보낼 수 있는지 확인
- 클라이언트와 서버 모두 한 번씩 `SYN` 와 `ACK`를 주고받으며 **데이터 수신 여부를 확인**
    
    논리적으로 총 네 단계가 필요한데, 서버의 `SYN + ACK`를 한 단계로 축약하여 세 단계를 가짐
    

### Why initial sequence number required to be random?

동일한 연결의 두 인스턴스가 동일한 시퀀스 번호를 너무 빨리 재사용하는 것, 즉 이전 연결의 세그먼트가 이후 연결의 인스턴스를 방해할 가능성(이전 연결로부터 오는 패킷으로 인식할 가능성)이 있는 경우를 방지하기 위함이다. 

TCP 표준 문서 [RFC793](https://www.rfc-editor.org/rfc/rfc793#section-3.3) Page.27에 따르면, “To avoid confusion we must prevent segments from one incarnation of a connection from being used while the same sequence numbers may still be present in the network from an earlier incarnation” (혼란을 피하기 위해서는 이전 연결의 동일한 시퀀스 번호들이 아직 네트워크에 남아 있을 수 있는 동안, 한 연결의 인스턴스로부터 온 세그먼트들이 사용되는 것을 막아야 합니다) 라고 한다.


(ps. tcp 요청 자체는 하나에서 계속 나가기 때문에, 하나의 것에서 여러 번 요청을 한다는 것을 의미하려고 ‘instance’나 ‘implementation` 대신 ‘incarnation’로 표현한 듯 하다.)

---

## 4-Way Handshake

TCP 연결을 단계적으로 해제(Connecntion Termination)하는 절차 

### **The Four Steps of a 4-Way Handshake**

**Step 1**: 클라이언트가 서버에 연결 종료 요청 (FIN)

- 클라이언트는 서버에 연결을 종료하고자 `FIN`(Finishing) 패킷 송신
- 클라이언트는 더 이상 데이터를 전송하지 않을 것임을 알림

**Step 2**: 서버가 클라이언트 요청에 응답 (ACK)

- 서버는 클라이언트의 `FIN` 패킷 수신
- 수신을 확인하기 위해 `ACK`(Acknowledgment) 패킷 송신
- 클라이언트는 아직 서버 쪽에서 데이터를 받을 수 있음

**Step 3**: 서버가 클라이언트에 연결 종료 요청 (FIN)

- 서버는 자신의 데이터 전송이 끝났음을 알리기 위해 `FIN`패킷을 송신

**Step 4**: 클라이언트가 서버 요청에 응답 (ACK)

- 클라이언트는 서버의 `FIN` 패킷 수신
- 수신을 확인하기 위해 `ACK`패킷 송신
- 클라이언트는 더 이상 서버로 데이터를 받지 않을 것임을 인지

![TCP-Connection-Termination.png](./3-Way,%204-Way%20Handshake/termination.png)

http://www.tcpipguide.com/free/t_TCPConnectionTermination-2.htm

## TIME_WAIT

- Connection을 먼저 종료하는 **Active Closer**가 Connection 종료 후 도달하는 상태
    
    ※ Close 요청을 먼저한 주체가 누구냐에 따라 **Active Close**, **Passive Close** 대상이 달라진다, 먼저 close 요청한 쪽이 Active Close.
    
- Network에 남아 있을 수 있는 종료된 Connection의 Packet이 완전히 제거될 때까지 대기
- 이후에 생성되는 새로운 Connection에 영향을 미치지 않기 위한 용도로 이용되는 상태

### Problems if TIME_WAIT is short

1. **Packet Delay**에 의해서 새로운 Connection이 이전 Connection의 영향을 받는 상황
    
    ![[그림 1] TCP Connection Issue with Packet Delay](./3-Way%2C%204-Way%20Handshake/TCP%20Connection%20Issue%20with%20Packet%20Delay.png)
    
    [그림 1] TCP Connection Issue with Packet Delay
    
    - 붉은 선으로 표시 된 Client가 전송한 SEQ=3 Packet이 지연됨
    - Client와 Server가 기존의 Connection을 종료하고 새로운 Connection을 맺음
    - 이후에 이전 Connection의 지연된 SEQ=3 Packet이 Server에게 전달
    - Server가 받아야 하는 SEQ와 지연된 Packet의 SEQ가 다르면 Server에서는 Drop하기 때문에 문제가 발생하지 않음
    - 하지만 **Server가 수신해야 하는 SEQ와 지연된 Packet의 SEQ가 우연히 동일한 경우**에는 Server는 지연된 Packet을 Drop하지 않고 수신하여 Data 무결성이 깨진다.
2. 원격 종단의 연결이 닫혔는지 확인해야할 경우 (TCP 4-Way Handshake의 마지막 `ACK` Flag가 Server(**Passive Closer**)에게 전달되지 않아 Server가 `LAST_ACK` 상태를 유지하는 상황)
    
    ![[그림 2] TCP Connection Issue with Lost Last ACK in 4Way Handshake](./3-Way%2C%204-Way%20Handshake/TCP%20Connection%20Issue%20with%20Lost%20Last%20ACK%20in%204Way%20Handshake.png)
    
    [그림 2] TCP Connection Issue with Lost Last ACK in 4Way Handshake
    
    - Server의 `LAST_ACK` 상태가 **Timeout**에 의해서 `CLOSED` 상태로 변경 되기전, Client는 새로운 Connection을 위해서 동일한 Local IP/Port를 이용하여 Server에게 `SYN` Flag를 전송
    - `LAST_ACK` 상태의 Server는 `SYN` Flag를 받을 경우 `RST` FLAG를 전송하여 Connection 생성을 막기 때문에 새로운 Connection 생성 실패

### ****Avoiding TIME_WAIT****

- `tcp_timestamps`
- `tcp_tw_reuse`
- `tcp_tw_recycle`
- `Socket Lingering`
---

## 3-Way Handshake vs 4-Way Handshake

연결 설정 시에는 클라이언트의 연결 요청 시점과 서버의 연결 요청 시점이 거의 동시에 진행된다. 따라서 서버는 클라이언트에 대한 응답과 연결 요청을 `SYN + ACK` 로 압축하여 보낼 수 있다.  

연결 종료 시에는 클라이언트가 데이터 전송을 마쳤다고 하더라도 서버는 아직 보낼 데이터가 남아있을 수 있기 때문에, `FIN`에 대한 `ACK`만 우선적으로 보낸다. 데이터를 모두 전송한 이후에 `FIN` 메시지를 보낸다.

---

참고

- https://www.rfc-editor.org/rfc/rfc793
- https://www.ibm.com/docs/en/zos/2.1.0?topic=SSLTBW_2.1.0/com.ibm.zos.v2r1.halu101/constatus.html
- http://www.tcpipguide.com/free/t_TCPConnectionEstablishmentProcessTheThreeWayHandsh-3.htm
- http://www.tcpipguide.com/free/t_TCPConnectionTermination-2.htm
- [https://velog.io/@averycode/네트워크-TCPUDP와-3-Way-Handshake4-Way-Handshake#3-way-handshake의-기본매커니즘](https://velog.io/@averycode/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCPUDP%EC%99%80-3-Way-Handshake4-Way-Handshake#3-way-handshake%EC%9D%98-%EA%B8%B0%EB%B3%B8%EB%A7%A4%EC%BB%A4%EB%8B%88%EC%A6%98)
- https://ssup2.github.io/theory_analysis/TCP_TIME_WAIT_State/
- https://docs.likejazz.com/time-wait/
- https://serverframework.com/asynchronousevents/2011/01/time-wait-and-its-design-implications-for-protocols-and-scalable-servers.html
