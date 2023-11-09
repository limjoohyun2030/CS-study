# [AWS] S3 cloudfront EC2

## Cloud Computing 이란

편리하게 컴퓨팅 자원에 접근해 데이터를 처리, 연산할 수 있도록 

네트워크, 서버, 스토리지, 애플리케이션을 연결해 놓은 컴퓨팅 제공 방식

### Cloud Computing 을 사용하는 이유?

시공간의 제약을 받지 않고 좋은 컴퓨터를 사용할 수 있습니다.

사용한 만큼만 비용을 지불하면 되므로 효율적이고

짧은 시간을 이용하고도 전세계에 배포할 수 있습니다.

모든 기업이 Tech 위주는 아니다. 

데이터 보관, 인프라, 유지보수, IT 기술 개발 등은 클라우드에 넘기고

각자의 주력 비지니스에 집중을 하는 효율적인 방향으로 가고 있습니다.

![Untitled](https://github.com/limjoohyun2030/CS-study/assets/39722436/5b198a1f-badc-498f-a172-13f0d5769755)

*CSP: ***Cloud*** Service Provider

On-Premises: 서비스를 공급하는 서비스 제공자가 직접적으로 IT 자원을 관리하는 주체

Cloud System: 서비스를 공급하는 서비스 제공자는 IT 자원을 사용할 뿐, 대부분의 IT 자원 관리는 클라우드 서비스 제공자에게 제공받음

## Cloud Computing 유형

### Iaas (Intrastructure as a Service)

Iaas는 서버의 물리적인 부분은 기업에 돈을 내고 이용하며 소프트웨어를 직접 관리하는 방식입니다. 

사용자에게 최신 가상(VM) 컴퓨터 한 대(Instance)를 빌려주는 것과 비슷합니다. 사용자는 받은 컴퓨터에 OS를 설치, 관리하고 애플리케이션을 배포, 유지 관리를 해야 합니다

**대표적인 Iaas 로는 아마존 AWS의 EC2 / S3 + CloudFront, 구글의 GCP, 마이크로소프트의 Azure 등이 있습니다**

트래픽에 따라 서버의 가용량을 직접 증감 시킬 수 있으므로 자유도가 높지만
사용자를 예상하고 그에 맞는 용량의 서비스를 사전에 구매하여 지불해야 하므로 

손실이 날 수도 있습니다.

### PaaS (Platform as a Service)

IaaS에서는 사용자가 각자 원하는 OS (Guest OS)를 설치합니다. 

그런데 한 컴퓨터에 Window 사용자가 5명 Linux 사용자가 10명이라면 

무거운 OS를 4번, 9번이나 중복해서 클라우드 서버에 설치해야 합니다. 

이런 Guest OS 중복을 줄이고 사용자의 Application과 Library(환경) 만 설정하면 되는 Container를 제공하도록 만든 것이 PaaS입니다.

**대표적인 PaaS 로는 Docker나 Kubernetess 등이 있습니다**

### SaaS **( Software as a Service )**

클라우드 서버에 애플리케이션이 설치되고 사용자는 그걸 꺼내 쓰기만 하면 되기 때문에 

데스크탑에 설치하지 않고 쓸 수 있는 것이 SaaS 입니다.

**대표적인 SaaS로는 Microsoft365 같은 것이 있습니다**

### Faas (Function as a Service)

**FaaS는 서버리스(Serverless) 라고도 불립니다.** 

서버리스는 서버가 없어도 된다는 뜻은 아니고, 서버를 개발자가 관리하지 않아도 된다는 의미입니다. 서버리스의 장점은 사용자 트래픽에 따라 CPU, Memory등의 용량을 고민하고 업스케일링, 다운스케일링을 하거나, 서버에 설치된 응용 소프트웨어 관리, 서버 상태 모니터링 등 관리에서 자유로워 진다는 큰 장점이 존재합니다. 

또, 함수 호출 횟수(실제 사용량)에 따라 비용이 청구되기 때문에 경제적이다.

단점은 항상 서버에 애플리케이션이 올라가있는 상태가 아니기 때문에 실행 속도가 느린 Cold Start 문제와, 함수의 메모리와 실행 시간이 제한되어있기 때문에 무거운 함수는 못 돌리는 점, 함수가 독립적, Stateless여야 한다는 점, 프로젝트 규모가 커질 경우 비용이 많이 청구될 수 있다는 점 등이 있습니다.

**FaaS로는 AWS의 lambda가 있습니다**

## AWS

AWS(Amazon Web Service)는 Amazon에서 제공하는 클라우드 서비스입니다. 

서버나 네트워크, 스토리지 등의 인프라 자원을 가상으로 제공해줍니다.

### S3(Simple Storage Service)

S3는 데이터를 저장하거나 추출할 수 있는 온라인 스토리지 웹 서비스입니다

- AWS 콘솔을 통해 파일 업로드, 다운로드가 가능합니다.
- http 프로토콜로 파일에 접근할 수 있습니다

데이터는 버킷 내에 객체로 저장됩니다.
버킷은 S3에서 생성할 수 있는 최상위 디렉토리로, 각 Region 별로 생성이 가능합니다. 
객체는 파일과 해당 파일을 설명하는 메타데이터(option)로 구성됩니다.

### AWS EC2 (Amazon Elastic Compute Cloud)

크기 조정(Elastc) 이 가능한 컴퓨팅 파워를 Amazon Cloud 에서 제공하는 웹 서비스

→ EC2 는 그냥 컴퓨터 1대 라고 생각해도 좋습니다

### CDN

짧은 지연 시간과 빠른 전송 속도로 데이터, 동영상, 애플리케이션 및 API를 전세계 사용자에게 전송하는 시스템

### CloudFront

Cloud Front는 짧은 지연 시간과 빠른 전송 속도로 데이터, 동영상, 애플리케이션 및 API를 전세계 사용자에게 전송하는 CDN (Content Delivery Network) 서비스입니다.

Client에게 빠른 전송 속도를 제공하기 위해 세계 이곳저곳에 중계서버(Edge Location)를 두고 Client에게 가장 가까운 서버에서 웹 자원을 대신 전달하여 빠르게 데이터를 제공한다.

### AWS Region

AWS 가 전 세계의 각 데이터센터를 클러스터링 하는(묶는) 물리적 위치

![Untitled 1](https://github.com/limjoohyun2030/CS-study/assets/39722436/29978503-7f75-42dd-a0a1-71a70292adb2)

### AWS AZ(Availability Zone)

데이터센터와 같은 의미이다.

각 Region 안에 최소 2개 이상의 데이터센터가 있다고 보면 됩니다.

태풍, 지진과 같은 천재지변에서 데이터를 보호하기 위해 여러 개를 둡니다.

![Untitled 2](https://github.com/limjoohyun2030/CS-study/assets/39722436/7fd36dbe-93e2-4162-b75a-3a491a5a3b87)

### VPC(Virtual Private Cloud)

논리적으로 격리된 사용자 전용 가상 네트워크

복수의 AZ에 걸친 상태로 생성가능

물리적으로는 다른 곳에 위치하지만 논리적으로 같은 네트워크

### Subnet

VPC 영역 안에서 망을 더  쪼개는 행위

단일 AZ 에 위치함. 여러 AZ에 걸쳐서 Subnet 을 생성할 순 없음

![Untitled 3](https://github.com/limjoohyun2030/CS-study/assets/39722436/f45d5e20-668a-4f6e-8e5d-1dfcd465561a)


참고링크

[https://velog.io/@jeongs/AWS-간단히-이해하기-S3-EC2-Cloud-Front](https://velog.io/@jeongs/AWS-%EA%B0%84%EB%8B%A8%ED%9E%88-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-S3-EC2-Cloud-Front)

[https://www.youtube.com/watch?v=tkP5u_SrF-8](https://www.youtube.com/watch?v=tkP5u_SrF-8)

[https://velog.io/@eeapbh/클라우드-컴퓨팅](https://velog.io/@eeapbh/%ED%81%B4%EB%9D%BC%EC%9A%B0%EB%93%9C-%EC%BB%B4%ED%93%A8%ED%8C%85)

[https://velog.io/@eeapbh/AWS-구조1글로벌-인프라-Region](https://velog.io/@eeapbh/AWS-%EA%B5%AC%EC%A1%B01%EA%B8%80%EB%A1%9C%EB%B2%8C-%EC%9D%B8%ED%94%84%EB%9D%BC-Region)

[https://velog.io/@eeapbh/AWS-구조2가용영역-AZ](https://velog.io/@eeapbh/AWS-%EA%B5%AC%EC%A1%B02%EA%B0%80%EC%9A%A9%EC%98%81%EC%97%AD-AZ)

[https://velog.io/@eeapbh/AWS-구조3엣지-로케이션-Edge-Location](https://velog.io/@eeapbh/AWS-%EA%B5%AC%EC%A1%B03%EC%97%A3%EC%A7%80-%EB%A1%9C%EC%BC%80%EC%9D%B4%EC%85%98-Edge-Location)

[https://velog.io/@eeapbh/Amazon-EC2](https://velog.io/@eeapbh/Amazon-EC2)

[https://velog.io/@eeapbh/AWS-CloudFront](https://velog.io/@eeapbh/AWS-CloudFront)

[https://velog.io/@anak_2/클라우드-컴퓨팅이란-무엇인가](https://velog.io/@anak_2/%ED%81%B4%EB%9D%BC%EC%9A%B0%EB%93%9C-%EC%BB%B4%ED%93%A8%ED%8C%85%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)

[https://velog.io/@allzeroyou/온프레미스On-premise와-클라우드Cloud의-차이점](https://velog.io/@allzeroyou/%EC%98%A8%ED%94%84%EB%A0%88%EB%AF%B8%EC%8A%A4On-premise%EC%99%80-%ED%81%B4%EB%9D%BC%EC%9A%B0%EB%93%9CCloud%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)

[https://inpa.tistory.com/entry/WEB-🌐-클라우드-컴퓨팅-개념-총정리-IaaS-SaaS-PaaS](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-%ED%81%B4%EB%9D%BC%EC%9A%B0%EB%93%9C-%EC%BB%B4%ED%93%A8%ED%8C%85-%EA%B0%9C%EB%85%90-%EC%B4%9D%EC%A0%95%EB%A6%AC-IaaS-SaaS-PaaS)
