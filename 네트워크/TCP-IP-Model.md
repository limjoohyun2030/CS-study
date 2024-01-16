### 네트워크 인터페이스 계층 (Network Interface Layer)

네트워크 인터페이스 계층은 노드 간 신뢰성 있는 데이터 전송을 담당하는 계층이다. 이 계층은 OSI 7 계층의 물리 계층과 데이터 링크 계층의 역할을 수행한다. Mac 주소가 이 계층에서 사용되며, 이를 통해 데이터 링크 계층까지 담당한다. 네트워크 인터페이스 계층에서는 네트워크 인터페이스 카드(NIC)가 필요하다. LAN에서는 Ethernet, Token Ring, FDDI 등이 사용되며, WAN에서는 X.25, Frame Relay, PPP 등이 사용된다.

처음엔 출발지의 Mac 주소와 가장 **가까운 라우터의 Mac 주소**를 넣는다.

처음에 송신자 A는 수신자 B의 Mac 주소를 알지 못하기 때문이다.

DHCP, ARP를 통해 Mac 주소를 획득한 후 **라우터에 대한 도착지 Mac주소를 만든** 후 헤더에 넣어준다. 

이러한 **Mac 주소 정보**와 **트레일러**라는 오류제어를 위한 정보를 data에 붙인 후 캡슐화  시킨것을 **프레임**이라 부른다.

### 인터넷 계층 (Internet Layer)

인터넷 계층은 OSI 7 계층의 네트워크 계층을 담당하는 계층이다. 이 계층은 호스트 간의 라우팅을 담당한다. 주요한 프로토콜로는 IP(Internet Protocol), ARP(Address Resolution Protocol), RARP(Reverse ARP), ICMP(Internet Control Message Protocol), IGMP(Internet Group Message Protocol) 등이 있다.

**Source IP & Destination IP 정보**를 헤더에 넣어서 data에 붙인 후 캡슐화 = **패킷** 

### 전송 계층 (Transport Layer)

전송 계층은 OSI 7 계층의 전송 계층과 동일한 역할을 수행한다. 이 계층은 프로세스 간의 신뢰성 있는 데이터 전송을 담당한다. 전송 계층에서는 논리적인 주소로서 포트 번호가 사용된다. 대표적인 프로토콜로는 TCP(Transmission Control Protocol)와 UDP(User Datagram Protocol)가 있다.

Encapsulation 과정에서 **TCP or UDP 인지에 대한 정보**와

**Source port & Destination Port 정보**를 헤더에 넣어서 data에 붙인 후 캡슐화 = **세그먼트**

### 응용 계층 (Application Layer)

응용 계층은 사용자와 가장 가까운 계층으로, OSI 7 계층의 5~7 계층 기능을 담당한다. 서버나 클라이언트 응용 프로그램이 이 계층에서 동작하며, 전송 계층의 주소인 포트 번호를 사용한다. 일반적으로 사용되는 프로토콜로는 HTTP(Hypertext Transfer Protocol), Telnet, SSH(Secure Shell), FTP(File Transfer Protocol), SMTP(Simple Mail Transfer Protocol), POP3(Post Office Protocol Version 3), DNS(Domain Name System) 등이 있다.

## 캡슐화(Encapsulation)와 역캡슐화(Decapsulation)

![](https://velog.velcdn.com/images/leequiett/post/a909b068-a859-4287-b1c2-2aef15dfd969/image.png)

![](https://velog.velcdn.com/images/leequiett/post/ccb25403-c65b-42ef-828f-a56fa2b7f340/image.png)

![](https://velog.velcdn.com/images/leequiett/post/11c44687-cb80-48f4-9e0f-be78f6d7c5f4/image.png)

1. 캡슐화(Encapsulation):
캡슐화는 상위 계층에서 하위 계층으로 내려가는 과정에서 데이터에 헤더와 트레일러를 추가하는 것을 의미합니다. 이 과정은 데이터를 패킷으로 분할하고, 각 계층의 헤더 정보를 추가하여 하위 계층으로 전달하는 것을 포함한다. 각 계층은 자신의 헤더 정보를 추가하고, 이전 계층으로부터 받은 데이터를 그 아래에 캡슐화하여 전달한다. 이러한 과정을 통해 패킷은 점진적으로 내려가며, 최종적으로 물리적인 링크로 전송된다.
2. 역캡슐화(Decapsulation):
역캡슐화는 상위 계층으로 올라가는 과정에서 패킷의 헤더를 제거하고 데이터를 추출하는 과정을 의미한다. 수신측은 패킷을 받은 후, 각 계층에서 헤더를 확인하고 해당 계층의 헤더를 제거한 뒤, 데이터를 추출한다. 이 과정은 패킷을 거꾸로 통과하면서 각 계층에서 필요한 정보를 제거하고 데이터를 추출하는 것이다. 역캡슐화가 완료되면 최종적으로 응용 계층에서 사용 가능한 데이터가 얻어지게 된다.

캡슐화와 역캡슐화는 데이터의 흐름을 제어하고 네트워크에서 정확한 전송을 보장하기 위해 중요한 역할을 한다. 각 계층은 자신의 헤더와 필요한 제어 정보를 추가하여 데이터를 캡슐화하고, 역캡슐화 과정에서 이러한 정보를 사용하여 데이터를 추출하고 처리한다. 이를 통해 데이터는 계층적인 구조로 전달되며, 각 계층은 자신의 역할을 수행하여 효율적이고 신뢰성 있는 통신을 가능하게 한다.

## end-point A에서 end-point B로 데이터 전송 시나리오

1. A에서 스위치로 데이터를 보낸다.
    1. 스위치는 데이터링크 계층의 대표적인 하드웨어
    2. 캠 테이블이라는게 있어서 스위치와 연결되어있는 맥주소에 대한 정보를 갖고 있다.
    3. 해당 맥주소에 포함되는 데이터가 오면 해당 end-point로 보내준다.
    4. 스위치는 Decapsulation을 한번 해서 헤더를 살펴본다.
    5. 헤더에 라우터에 대한 맥주소가 있다.
2. 스위치가 라우터로 데이터를 쏴준다. 
    1. 라우터는 L3인 네트워크 계층의 하드웨어이다. 
    2. 라우터에서 Decapsulation해서 도착지에 대한 IP주소를 확인한다.
    3. 라우팅 테이블을 통해 라우팅을 시킨다.
    4. B의 Mac 주소를 파악한다. 
    5.  L2의 헤더를 B에 대한 Mac 주소로 업데이트 시킨다.
    6. 실제 네트워크에서는 라우터가 정말 많으니 패킷 이동에 따라 라우터에서는 L2에 대한 source Mac & destination Mac 정보를 계속 업데이트 시키면서 전달한다.
    7. 스위치로 데이터를 보낸다.
3. 스위치에서 헤더정보를 확인한다.
    1. 캠 테이블을 통해 목적지로 보낸다.
4. Decapsulation 하면서 데이터를 읽어들인다. 

## **네이버 접속 시나리오**

1. 웹 브라우저에 www.naver.com 입력.
2. DNS로 네이버 서버 IP주소 할당.
3. 응용 계층(L4)에서 메세지 데이터 패킹(HTTP 메시지).
4. 전송 계층(L3)에서 PORT정보(출발지, 목적지), 전송제어 정보, 순서 정보, 검증 정보 패킹 (TCP).
5. 인터넷 계층(L2)에서 IP정보(출발지, 목적지) 패킹
6. 네트워크 엑세스(L1) 계층에서 mac주소 패킹
7. 게이트웨이를 통해 인터넷망 접속.
8. 라우터를 통해 목적지(네이버 서버)를 찾아 연결.
9. 네이버 서버에 도착하면 패킷을 하나 하나 까면서 목적 포트에 메세지 데이터 전달하여 다시 응답.

- 참조
    - James F. Kurose, Keith W. Ross. 2011. 4판. *Computer Networking A Top-Down Approach.* 교보문고
    - KNOU. 손진곤. 정보통신망([https://ucampus.knou.ac.kr/ekp/user/course/initUCRCourse.sdo?pageIndex=1&recordCountPerPage=4&sbjtId=KNOU1979001&cntsId=KNOU1979&atlcNo=&tespNo=&lectPldcTocNo=&examApexNo=&burSbjtCd=&tabNo=01&dblMjSbjtYn=N&curSbjtId=&curLectPldcTocNo=&systemDiv=H&searchCntsCateNo=34&searchShgr=&searchSeme=](https://ucampus.knou.ac.kr/ekp/user/course/initUCRCourse.sdo?pageIndex=1&recordCountPerPage=4&sbjtId=KNOU1979001&cntsId=KNOU1979&atlcNo=&tespNo=&lectPldcTocNo=&examApexNo=&burSbjtCd=&tabNo=01&dblMjSbjtYn=N&curSbjtId=&curLectPldcTocNo=&systemDiv=H&searchCntsCateNo=34&searchShgr=&searchSeme=))
    - 인파. TCP / IP 4계층 모델 - 핵심 총정리([https://inpa.tistory.com/329](https://inpa.tistory.com/329))
    - nellholic108. 네트워크 OSI 7계층( [https://velog.io/@nellholic108/네트워크-OSI-7-계층](https://velog.io/@nellholic108/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-OSI-7-%EA%B3%84%EC%B8%B5))
    - reakwon. [네트워크] OSI 7계층(OSI 7 LAYER) 기본 개념, 각 계층 설명([https://reakwon.tistory.com/59](https://reakwon.tistory.com/59))
    - 우아한테크. 파즈. [10분 테코톡] 👍 파즈의 OSI 7 Layer(https://youtu.be/Fl_PSiIwtEo)
