# Web Server & WAS

![Untitled](https://github.com/limjoohyun2030/CS-study/assets/39722436/730957ae-0ef2-4966-9a3b-2ceee70eddbb)

왼쪽이 Web Server, 오른쪽이 WAS

# 웹서버와 WAS의 필요성

웹서버와 WAS 를 구분하는 가장 대표적인 이유는 역할 분리입니다

웹서버는 항상 같은 결과를 보여주는 정적인 페이지(Static page) 요청을 주로 처리하고, 

WAS 는 사용자의 입력에 따라 다른 결과를 보여주는 동적인(dynamic) 페이지 요청을 처리하도록 

분리하면 서버의 부담을 줄일 수 있습니다.


## Web Server

![Untitled 1](https://github.com/limjoohyun2030/CS-study/assets/39722436/7d2de719-83cd-4543-b7e4-1d5d2e081c8e)

```jsx
클라이언트로부터 HTTP 요청을 받고, static page(html, css, js 등)을 제공하는 프로그램
```

HTTP 통신은 80번 포트를 사용하고

HTTPS 통신인 경우엔 443번 포트를 사용합니다

대표적인 웹서버에는 Apache Server, Nginx 등이 있습니다.

---

## Apache vs Nginx

### Apache

- 다중 프로세스로(MPM. Multi Process Module) 일을 처리
    
    Client 마다 프로세스를 생성하는 방식(mpm-prefork)
    
    한 프로세스 안에서 스레드를 새로 생성하는 방식(mpm-worker)
    
- 두 방식 모두, 프로세스나 스레드가 이 Context 실행했다 저 Context 실행했다 하는
    
    **context switching 이 발생.**
    
    이 방식은 컴퓨터 자원을 많이 소모합니다.
    
- 보안이 좋고, 다양한 모듈(MPM 등) 제공하기 때문에 좋습니다.

### Nginx

- 이벤트로(event-driven) 일을 처리하기 때문에 효율이 더 좋습니다.
    
    Event-driven 구조는 Clinet 요청을 병렬로 처리해서 
    
    동시에 커넥션 양 & 속도를 대폭 향상 할 수 있습니다.
    
- 시간이 오래 걸리는 작업이 있다면 Thread Pool에게 해당 일을 위임합니다
- SSL 터미네이션 수행. Client 와는 HTTPS 통신을 하고, Server 와는 HTTP 통신을 합니다.
- 모듈 직접 개발 까다롭습니다.

따라서 성능과 가벼움을 중요시 한다면 Nginx,

안정되고 검증된 기능들을 필요로 한다면 Apache

---


## WAS(Web Application Server)

![Untitled 2](https://github.com/limjoohyun2030/CS-study/assets/39722436/911fea38-1c33-4aaf-8e5e-1024dff11713)

웹 어플리케이션과 서버환경을 만들고, 

**DB나 외부 서비스와 상호작용하며 비지니스 로직을 처리할 수 있습니다.**

php jsp 와 같은 언어들을 이용해서 동적인 페이지를 생성할 수 있고

대표적인 WAS로는 Tomcat, JBoss, Jeus, WebSphere 등이 있습니다.

### Tomcat

가장 흔히 쓰이는 Tomcat은 요즘 스프링에 내장돼있어서 직접 접하는 빈도가 줄었지만

JAVA 와 JSP 로 만든 웹 또는 앱을 실행할때 톰캣 같은 WAS가 사용됩니다.

### 그럼 Node.js 와 Go 에선…??

WAS 를 검색하면 죄다 JAVA 관련된 내용이 나옵니다.

다른 진영에서는 WAS 역할을 한다 라고 딱히 구분해서 사용하지는 않는데 

이런게 다 정의하기 나름이고, 진영마다 배포 구조들도 다르기 때문입니다. 

참고로, Node.js 에서는 어플리케이션이 WAS 역할까지 합니다.

# WebServer 와 WAS 를 둘다 사용하는 이유

### Reverse Proxy

웹서버는 Reverse Proxy 기능을 합니다.

Forward Proxy 는 Client 를 숨기고, Reverse Proxy 는 Server 를 숨깁니다.

웹이나 API 를 만들고, 그 앞단에 웹서버를 두고 Client 요청들을 처리합니다.

웹서버는 다양한 **보안기능을** 제공하기 때문에 리소스 위치, 포트번호 같은 정보들을 감춥니다

### load balancing

**지속성을 위함**

스프링으로 만든 웹에서 동적으로 동작하는 새 기능을 추가 했을때, 

돌고 있는 서비스를 종료하고 재실행을 해야 합니다. 

이러면 Client 가 서비스에 접속 중이면 잠깐이나마 작동을 멈춘 상태를 보게 될 수 있습니다.

WAS 서버를 여러개 두면, 한 개가 재부팅 되는 동안

웹서버가 다른 톰캣들로 Client 요청을 보내서 서비스의 지속성을 높일 수 있습니다.

프로세스를 각각의 톰캣으로 분산하면 퍼포먼스가 증가할 수 있습니다.

### Caching

웹서버가 Reverse Proxy 로서 제공하는 **서버단의 캐시입니다.**

서버로 찾아오는 Client 가 자주 요청하는 리소스를 가지고 있다가 응답을 합니다.

### Health Check

서버에 주기적으로 HTTP 요청을 보내, 서버의 상태를 확인(ex 특정 url 요청에 200 응답이 오는지)

```jsx
Nginx예시

location / {
	proxy_pass http://backend;
	health_check interval=10 fails=3 passes=2;
}
```

Interval: health check를 통해 서버 상태를 확인하는 요청을 날리는 주기(default: 5초)

Fails: 요청이 몇번 실패하면 서버가 비정상인지 인지하는 횟수

Passes: 서버가 다시 복구되어 요청을 보낼건데 몇번 연속 성공하면 정상으로 간주할지에 대한 횟수

### fail over, fail back

fail over 와 fail back 처리에 유리합니다.

fail over: 서버, 시스템, 네트워크 등에서 이상이 생겼을 때 예비 시스템으로 자동 전환

fail back: fail over에 따라 전환된 서버, 시스템, 네트워크 등을, 이상이 발생하기 전의 상태로 되돌리는 처리

<br>

참고 링크

[https://www.youtube.com/watch?v=mcnJcjbfjrs](https://www.youtube.com/watch?v=mcnJcjbfjrs)

[https://www.youtube.com/watch?v=Zimhvf2B7Es](https://www.youtube.com/watch?v=Zimhvf2B7Es)

[https://www.youtube.com/watch?v=NyhbNtOq0Bc](https://www.youtube.com/watch?v=NyhbNtOq0Bc)

[https://80000coding.oopy.io/2352c04e-8f98-4695-a5fe-8c789ee94d98#effe83ea-dcc5-433b-9e11-907c7ae94b5c](https://80000coding.oopy.io/2352c04e-8f98-4695-a5fe-8c789ee94d98#effe83ea-dcc5-433b-9e11-907c7ae94b5c)

[https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html](https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html)

[https://lu-coding.tistory.com/101](https://lu-coding.tistory.com/101)

[https://velog.io/@pjy05200/Web-Server와-WAS의-차이](https://velog.io/@pjy05200/Web-Server%EC%99%80-WAS%EC%9D%98-%EC%B0%A8%EC%9D%B4)
