## 네트워크 아키텍처란

**네트워크 아키텍처는** 컴퓨터 네트워크에서 서로 다른 장치들이 어떻게 연결되고 통신하는지에 대한 규칙과 절차들의 집합이다. 이는 컴퓨터 네트워크를 구성하는 물리적인 구성 요소와 기능, 동작 원칙, 통신 프로토콜 등을 다루는 프레임워크이다.

과거에는 폐쇄형 네트워크 아키텍처로 IBM의 SNA, DEC의 DNA, Honeywell의 DSA 등이 있었다. 이러한 아키텍처는 제조사별로 독립적으로 개발되어 호환성 문제가 발생하곤 했다. 예를 들어, A라는 사람이 맥을 사용하는데 B라는 사람은 IBM 컴퓨터를 사용한다면, 서로 다른 문자 코드를 사용하기 때문에 데이터 통신에 어려움이 있었다. 이런 문제를 해결하기 위해 Interconnection Network를 위한 프로토콜이 필요하게 되었고, 그 결과로 개방형 네트워크 아키텍처인 OSI 7 계층과 TCP/IP가 등장하였다.

OSI 7 계층과 TCP/IP는 다양한 장치 간의 상호 연결을 위한 프로토콜과 규약을 제공하는 개방형 네트워크 아키텍처이다. 이를 통해 서로 다른 시스템이나 플랫폼 간에 표준화된 통신이 가능해졌으며, 데이터 전송 시에는 적절한 계층에서 데이터를 캡슐화하고 역캡슐화하여 효율적인 통신을 지원한다.

## 개방형 아키텍처란

초기에는 보안상 문제로 자원을 공유하지 않았지만 점차 사용자가 자원 공유의 이점을 발견 후 상호간에 접속을 자유롭게 할 수 있는 개방형 네트워크 아키텍처가 등장하였고 그 대표적인것이 OSI 참조 모델과 TCP/IP 이다.

| 항목 | OSI 참조 모델 | TCP/IP |
| --- | --- | --- |
| 주요 특징 | 개방형 시스템 상호 연결 | 전송제어 및 인터넷 프로토콜 |
| 의미 | 컴퓨터 네트워크의 이론적 모델 | 인터넷을 통해 데이터를 전송하는 데 사용되는 클라이언트-서버 모델 |
| Layer 수 | 7 | 4 |
| 개발 주체 | ISO | 미국 국방부 |

![](https://velog.velcdn.com/images/leequiett/post/197d1b78-6b83-4889-b23b-5306fa104913/image.png)

**OSI (Open Systems Interconnection)** 모델은 네트워크 프로토콜의 기능과 계층을 분리하여 정의하는 개방형 표준 모델이다. 이 모델은 전체 네트워크 통신을 7개의 계층으로 분할하고, 각 계층은 독립적인 기능과 역할을 수행한다. OSI 모델의 계층은 다음과 같다. 

## OSI 모델

### 물리 계층 (Physical Layer)

물리 계층은 OSI 7 계층 중 가장 하위에 위치하며, 전기적, 기계적, 기능적인 특성을 이용하여 데이터를 전송하는 역할을 한다. 데이터는 0과 1의 비트열, 즉 On과 Off의 전기적 신호로 표현된다. 이 계층은 데이터를 전송하는 것에만 관여하며, 에러탐지 등의 기능은 가지고 있지 않다.

![](https://velog.velcdn.com/images/leequiett/post/3eb608de-4d1f-4df7-8fa2-682f4ffe7410/image.png)

### 데이터 링크 계층 (Data Link Layer)

데이터 링크 계층은 동일한 네트워크 내에서의 전송을 담당하  물리 계층에서 송수신되는 정보의 오류와 흐름을 관리하여 안전한 정보의 전달을 수행하는 역할을 하며 데이터 링크 계층은 Point-to-Point 간의 통신을 담당한다. 안전한 정보의 전달은 오류 검출 및 재전송 기능을 포함한다. 또한, Mac 주소를 통해 통신을 가능하게 한다. 데이터 링크 계층에서는 데이터의 단위로 프레임(Frame)을 사용한다.

**Transport Layer에 오류제어가 있는데 왜 Data Link Layer에서 또 오류제어가 있을까?**

data link layer의 데이터 단위를 frame라고 부르는데  10개의 frame이 있을 때 2개의 frame이 오류나면 data link layer는 버려버린다. 

그에 반면에 transport layer의 오류제어는 해당 데이터가 없으면 다시 보내 줌으로써 오류 복구까지 해주는 셈.

### 네트워크 계층 (Network Layer)

네트워크 계층은 IP나 라우터장비가 속한 계층. 라우팅 기능으로 최적의 경로를 설정하여 목적지까지 데이터를 안전하고 빠르게 전달한다. 또한, 통신을 위해 어느 컴퓨터에 데이터를 전송할지 주소를 가지고 있다. 이러한 주소는 IP 주소로 알려진 네트워크 계층 헤더에 포함된다. 네트워크 계층에서는 데이터의 단위로 패킷(Packet)을 사용한다.

### 전송 계층 (Transport Layer)

서로 다른 두 네트워크간의 전송을 담당하는 계층이다. 전송 계층은 송신자와 수신자 간의 신뢰성 있는 데이터 전송을 지원하기 위해 오류 검출 및 복구, 흐름 제어, 중복 검사(TCP) 등의 기능을 수행한다. 데이터 전송을 위해 포트 번호가 사용되며, 대표적인 프로토콜로는 TCP와 UDP가 있다. 전송 계층에서 사용하는 데이터의 단위는 세그먼트(Segment)(세그멘테이션 작업을 통해)이다.

### 세션 계층 (Session Layer)

세션 계층은 응용 프로세스 간의 통신을 관리하는 방법을 정의한다. 세션 계층은 세션을 생성및 제거그리고 복구하는 역할을 수행한다. 

**세션 복구**는 체크포인트를 통해 동기화 시켜준다.

컴퓨터 A에서 B로 100MB의 데이터를 전송한다고 했을 떄 체크포인트 5MB로 설정했다고 가정하했을 때 48MB 전송 중 연결이 끊겨도 

![](https://velog.velcdn.com/images/leequiett/post/4bb039f1-c6f5-4f2b-b54d-50303dbd8d24/image.png)

![](https://velog.velcdn.com/images/leequiett/post/75c650c8-806f-4a14-a5cf-100743590a1d/image.png)

체크포인트 덕분에 다시 45MB에서 세션을 재개할 수 있게 된다. 

### 표현 계층 (Presentation Layer)

표현 계층은 데이터의 표현 방식을 정의하는 계층으로 데이터의 부호화, 변환, 압축, 암호화(서로 다른 end-point간에 다른 인코딩을 사용할 수도 있기 때문에) 등의 기능을 수행한다. MIME 인코딩이나 암호화 등이 표현 계층에서 이루어진다.

### 응용 계층 (Application Layer)

응용 계층은 사용자와 가장 가까운 계층으로, 사용자가 이용하는 응용 서비스나 프로세스가 동작하는 계층이다. HTTP, FTP, SMTP, Telnet 등의 프로토콜들이 속한 계층이다.


- 참조
    - James F. Kurose, Keith W. Ross. 2011. 4판. *Computer Networking A Top-Down Approach.* 교보문고
    - KNOU. 손진곤. 정보통신망([https://ucampus.knou.ac.kr/ekp/user/course/initUCRCourse.sdo?pageIndex=1&recordCountPerPage=4&sbjtId=KNOU1979001&cntsId=KNOU1979&atlcNo=&tespNo=&lectPldcTocNo=&examApexNo=&burSbjtCd=&tabNo=01&dblMjSbjtYn=N&curSbjtId=&curLectPldcTocNo=&systemDiv=H&searchCntsCateNo=34&searchShgr=&searchSeme=](https://ucampus.knou.ac.kr/ekp/user/course/initUCRCourse.sdo?pageIndex=1&recordCountPerPage=4&sbjtId=KNOU1979001&cntsId=KNOU1979&atlcNo=&tespNo=&lectPldcTocNo=&examApexNo=&burSbjtCd=&tabNo=01&dblMjSbjtYn=N&curSbjtId=&curLectPldcTocNo=&systemDiv=H&searchCntsCateNo=34&searchShgr=&searchSeme=))
    - 인파. TCP / IP 4계층 모델 - 핵심 총정리([https://inpa.tistory.com/329](https://inpa.tistory.com/329))
    - nellholic108. 네트워크 OSI 7계층( [https://velog.io/@nellholic108/네트워크-OSI-7-계층](https://velog.io/@nellholic108/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-OSI-7-%EA%B3%84%EC%B8%B5))
    - reakwon. [네트워크] OSI 7계층(OSI 7 LAYER) 기본 개념, 각 계층 설명([https://reakwon.tistory.com/59](https://reakwon.tistory.com/59))
    - 우아한테크. 파즈. [10분 테코톡] 👍 파즈의 OSI 7 Layer(https://youtu.be/Fl_PSiIwtEo)

