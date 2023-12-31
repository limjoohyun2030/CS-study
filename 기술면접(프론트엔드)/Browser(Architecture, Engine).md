## Browser 란?

![Browser](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbujQhW%2FbtqIarpq4zn%2F4nFWF3eyLMstYCWRKptTrk%2Fimg.png)

Web Server 와 양방향 통신을 하면서, 

Client 가 Request 한 파일을 Server 에서 Response 해서 보여주는 

Graphical User Interface(GUI) 기반 SW입니다.

![Browser Market Share](https://api.backlinko.com/app/uploads/2021/07/desktop-browser-market-share.png)

통계를 보면 알 수 있듯이 과반수 이상 유저들이 크롬을 사용하고 있습니다.


Browser 는 7가지 요소로 구조를 설명할 수 있습니다

![Browser Architecture](https://browserstack.wpenginepowered.com/wp-content/uploads/2022/12/Architecture-of-Web-Browsers-700x564.png)

1. User Interface
URL 표시줄, 뒤로가기 버튼, 북마크 버튼등 요청한 페이지를 제외한 부분에 해당됩니다.

2. Browser Engine
User Interface 와 Rendering Engine 사이의 동작을 제어합니다.
Data Storage를 참조하여 로컬에서 데이터를 쓰고 읽을 수 있습니다.
Chrome 같은 경우엔 Blink 를 사용중입니다.

3. Rendering Engine
요청한 Contents 를 화면에 출력하는 역할입니다.
HTML,CSS 등을 Parsing 해서 최종적으로 화면에 그려줍니다.

4. Networking
HTTP 요청과 같은 네트워크 호출을 할 수 있습니다

5. JS Engine
Javascript 코드를 해석하고 실행할 수 있습니다.

6. UI Backend
기본적인 Widget 을 그리는 Interface 입니다.

7. Data Storage 
Local Storage, Session Storage, Cookie 등 Browser 의 메모리를 활용하여 저장하는 영역입니다.



## Browser Engine or Rendering Engine (WebKit, Blink)

![Web Browser Engines](https://i1.sndcdn.com/artworks-tgPN71cfvxJdy8Dz-xhRQCA-t500x500.jpg)

Rendering Engine이란, Web Server 에 있는 HTML, CSS, XML 같은 파일들을

받아와서 Parsing 하고, Client 가 읽을 수 있는 document 로 표현하는 

Web Browser 의 핵심 기능입니다. 

<br>

WebKit 은 Apple 에서 개발하는 오픈소스 프로젝트이며,

Web 과 OS 에서 작동하는 Browser Engine 입니다.

애플의 정책에 따라 iOS, iPadOS 용 Web Browser 는 WebKit 가 강제되는데

Edge, Whale, Chrome 등 원래 WebKit를 쓰지않는 Web Browser 들도 

iOS, iPadOS 환경에서는 WebKit를 사용해야 합니다

<br>

Blink 는 Chrome 28버전부터 적용된 WebKit 기반 Web Browser Rendering Engine 입니다.

Google은 이걸 라이센스를 침해하지 않는 선에서 소스코드를 포크했고

여기서 탄생한데 Blink Engine 입니다.

코드 베이스에서 약 7000 개의 파일들을 제외했기 때문에 프로세스 처리와 구조가 완전히 다르다고 합니다.

Blink 가 포크돼서 나온거라 그런지, WebKit 과 Blink 의 차이점에 대해 검색하니

Blink 에 대한 장점들이 주로 나왔습니다.

<br>
<br>

-CSS

Blink는 LayoutNG라는 새로운 CSS 레이아웃 엔진을 사용하는데

이를 통해, WebKit 에서 구현하기 어려운 레이아웃을 잘 처리한다고 합니다

또,Blink는 CSS 사용자 정의 속성을 지원해서 css속성을 변수로 사용할 수 있는데

WebKit 에서는 동작하지 않습니다.

CSS Animation Rendering 에도 미묘한 차이가 있다고 합니다.

<br>
<br>

-JS

Blink에는 JavaScript 코드를 즉시 최적화 할 수있는 Just-in-Time ( JIT ) 

컴파일러가 있어서 복잡한 프로그램의 성능을 향상시킬 수 있습니다.

반면, WebKit은 Blink의 JIT 컴파일러보다 효율성이 떨어지는 

바이트 코드 인터프리터를 사용한다고 합니다

<br>

Blink 는 Chrome 용으로 개발된 V8 JS Engine 을 사용합니다.

V8 은 Node.js 와 Chromium Browser 에서도 사용됩니다.

반면 WebKit는 JavaScriptCore 엔진을 사용하며 Apple의 Safari Browser 에서 사용됩니다

나쁘진 않으나 V8 만큼 효율이 나오진 않습니다.

<br>

차이점이 많은 것 같았고, Apple 과 Google 이 서로 경쟁하는 관계이지만 

HTML5 라는 표준이 있기 때문에 엔진간 호환성 문제는 크지 않다고 합니다.

<br>

## Chromium 은 뭐지?

혹시 Chrome 말고, Chromium 이란 존재를 알고 계신가요?

회사에서 처음 이 단어를 들었는데, 이 기회에 Chromium 에 대해 간략하게 

알아보려 합니다.

![Chromium](https://mblogthumb-phinf.pstatic.net/MjAyMTAzMTRfMjM3/MDAxNjE1NzI5ODE1NzI1.6wZiGfjXBDnxBhXrP5o9B2SDA4dTH43MUZHQ6hOT7VAg.iah5xa8pR2o_XsFY5yNbkl4gqwVEC1nyiFaABFqe2GYg.JPEG.erke2000/chromium-image.jpg?type=w800)

Chromium 은 구글이 만든 오픈소스이고, Chrome 의 조상격이라고 볼 수 있습니다.

Chromium 에서 이것 저것 살을 붙혀 만든게 Chrome 이고

Edge, Opera, Naver Whale, Brave 같은 Browser 들 모두 Chromium 기반입니다.

<br>

Chromium 자체는 버그도 많고 불안전 하지만 Browser 완성을 위한 틀이라고 이해하면 됩니다

Chrome 이 아닌 Chromium 으로 Browser 를 만들어서 쓰는 이유는

Chrome 보다 훨씬 더 적은 양의 사용자 정보를 수집해서 구글에 전송하기 때문이라고 합니다.

<br>

### Brave 란?

![Brave](https://upload.wikimedia.org/wikipedia/commons/8/83/Brave_Browser_Welcome_Page.png)

짧게 제가 쓰고 있는 Open Source Browser 인 Brave 에 대해 소개해드리려 합니다.

Brave Browser 는 Blockchain 기반 Web3 DApp(Decentralized applications) 입니다.

<br>

Chrome 이 광고를 게시하고 이를 통해 구글이 돈을 버는 구조인데, 

이 때문에 사용자는 웹사이트 속도가 느려지는 경험을 하게 됩니다.

현재 온라인 광고 시장은 낚시성 이나 어그로성 기사 제목, 뜬금없는 팝업 광고, 

불명확한 광고 수익 배분 방식 등으로 왜곡 돼있습니다.

<br>

여기에 불만을 제기한게 Brave Browser 입니다. 

Brave Browser 에서는 광고를 볼지 말지 사용자가 선택을 하도록 하고, 

광고를 본다면 BAT(Basic Attention Token) 라는 이더리움 기반의 디지털 광고 플랫폼 

암호화폐를 보상으로 제공합니다.

<br>

Brave 의 장점을 체감할 수 있는 예시가 있는데요,

Brave 를 이용하면 광고 스킵, 백그라운드 음악 재생등

Youtube Premium 이 할 수 있는 기능들을 할 수 있습니다.

이런 신기술 트렌드가 있다~ 정도만 기억해주시면 감사하겠습니다.

<br>

## 아직 작성해야하는 부분
https://velog.io/@remon/V8-%EC%97%94%EC%A7%84%EC%9D%B4-%EB%8C%80%EC%B2%B4-%EB%AD%90%EC%95%BC
https://velog.io/@gay0ung/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-%EB%A0%8C%EB%8D%94%EB%A7%81-%EC%9B%90%EB%A6%AC
https://javannspring.tistory.com/233


## 참고 링크
https://velog.io/@gay0ung/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-%EB%A0%8C%EB%8D%94%EB%A7%81-%EC%9B%90%EB%A6%AC
https://ykarma1996.tistory.com/72
https://javannspring.tistory.com/233
https://all-young.tistory.com/22
https://namu.wiki/w/Brave
https://namu.wiki/w/%EB%B2%A0%EC%9D%B4%EC%A7%81%20%EC%96%B4%ED%85%90%EC%85%98%20%ED%86%A0%ED%81%B0
https://blog.devgenius.io/webcore-is-the-layout-engine-used-by-both-blink-and-webkit-but-there-are-some-differences-between-a2dccb81236c
