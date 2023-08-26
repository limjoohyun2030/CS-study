# CSR SSR SSG

## 개요

CSR(Client Side Rendering)과 SSR(Server Side Rendering)은 웹페이지를 렌더링하는 대표적인 방법들입니다.

![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/390bff04-4219-4ed0-a849-2f98e5d46b20)

# SPA, MPA, SEO

CSR, SSR 이전에 SPA, MPA, SEO 에 대해 알아보겠습니다.

![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/e5c81b80-81cc-45ba-bac3-17b0dda774b7)

![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/3482cf09-6b2d-4496-acb7-2bdc20b4f918)

<br>

### **SPA (Single Page Application)**

- 하나의 HTML 파일을 기반으로 자바스크립트를 이용해 동적으로 화면의 컨텐츠를 바꾸는 방식의 웹 어플리케이션
- 최초로 한번 페이지를 로딩하고 나면 필요한 부분만 요청해서 로딩하기 때문에 사용자 경험이 좋습니다. 화면 깜빡임 없이 부드럽게 이동합니다.
- 웹에서 제공하는 정보량이 많아지고, PC 보다 성능이 떨어지는 Mobile 환경을 고려해서 등장했습니다.
- Server 에서는 JSON 파일만 보내주고 HTML Rendering 은 Browser 가 합니다.
- 예시로 React, Vue, Angular 등이 있습니다

### **MPA (Multiple Page Application)**

- 사용자가 페이지를 요청할 때마다, 웹 서버가 요청한 UI와 필요한 데이터를 HTML로 파싱해서 보여주는 방식의 웹 어플리케이션
- HTML CSS JS 가 다시 다운되는 전통적인 웹페이지 구성방식이기 때문에 페이지 이동시 전체 페이지를 다시 렌더링합니다.
- 예시로 PHP, JSP 가 있습니다.

<br>

### SPA==CSR?? , MPA == SSR??

아닙니다.

SPA, MPA 는 몇개의 페이지를 쓰는지에 관한 것이고

CSR, SSR 은 어느 쪽에서 렌더링하는지에 관한겁니다.

<br>

### **SEO(Search Engine Optimization)**

검색 엔진 최적화는, 해당 웹사이트가 검색엔진의 선택을 받고 노출이 잘 되게 만드는 기법입니다.

웹사이트도 사업성이 있고 수익이 있어야 하기 때문에 검색엔진의 선택을 받는게 중요합니다.

- 대부분의 웹 크롤러, 봇들은 JS를 실행시키지 못하고 HTML에서만 컨텐츠를 수집하기 때문에
    
    CSR 방식으로 개발된 페이지를 빈 페이지로 인식하게 되고, SEO 에 문제가 생깁니다.
    
- SSR 방식은 View를 서버에서 전부 렌더링하기 때문에 HTML에 모든 컨텐츠가 저장되어 있어 SEO를 사용하는데 문제가 없습니다.

SPA가 사용하는 렌더링 방식은 CSR이고, MPA가 사용하는 렌더링 방식은 SSR입니다. 

각 방식의 동작 방식과 장단점을 알아보고, 

CSR 과 SSR 의 장점들을 사용하는 Next.js 에 대해서도 짧게 알아보려 합니다.

## CSR(Client Side Rendering)

![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/ca440bd2-3e28-4c29-939a-7e132bebb68a)


> [CDN](https://ko.wikipedia.org/wiki/%EC%BD%98%ED%85%90%EC%B8%A0_%EC%A0%84%EC%86%A1_%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC) : aws의 cloudflare가 대표적인 예시이며, 엔드 유저의 요청에 '물리적'으로 가까운 서버에서 요청에 응답하는 방식
> 

![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/ad2e35fb-94e9-4f68-adbb-daffe62bfb70)


![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/88cc966d-1df1-4286-b79b-beb254af5072)


CSR 는 말 그대로 Client 쪽에서 Rendering 을 하는 방식입니다

1. Browser 가 Server 에 HTML 과 JS 를 요청합니다
2. Server 는 Browser 에게 JS 를 가지고 있는, 빈 HTML 을 전달합니다.
3. Browser 는 해당 JS 를 다운 받습니다
4. 그동안 Client 는 빈 화면을 보고있습니다.
5. JS 가 다운되면 Browser 가 UI 를 Rendering 합니다
6. 화면이 잘 보이고, JS 도 정상 작동을 합니다.

### Pros

- 초기에 로딩을 한 이후에는 Rendering이 동적으로 빨리 되기 때문에 사용자 경험(UX) 이 좋습니다.
- Server 쪽에서 리소스를 한번에 다 받아왔기 때문에 리소스 요청이 있는 부분만 부분적으로 Server에 요청을 합니다. 따라서 Server 쪽 부하가 줄어듭니다.
- TTV(Time to view) == TTI(Time to interactive)

### Cons

- 초기 로딩시간이 오래 걸립니다. code splitting, tree-shaking chunk 와 같은 분리를 통해 JS번들 크기를 줄이고, 초기 DOM생성속도를 줄이면 속도를 개선할 수 있지만, 완벽히 해결할 수는 없습니다.

> 모듈화했던 파일들을 하나로 묶는 것을 번들링이라고 하는데, 번들링된 JS파일은 너무 커서 다운받는데 오랜 시간이 걸릴 수 있습니다. JS파일을 다운받는데 걸리는 시간동안 화면의 기능(버튼 이벤트 등) 작동하지 않기 때문에 하나로 번들링된 JS 파일을 페이지 별로 필요한 JS 파일로 분리한 것이 code splitting이다.
> 

- 검색엔진의 검색 봇이 크롤링을 하는데 어려움을 겪기 때문에 SEO (Search Engine Optimization)의 문제가 있습니다.
    - 구글 봇의 경우는 JS를 지원하지만, 다른 검색엔진의 경우 그렇지 않기 때문에 문제가 된다. 네이버 같은 경우엔 CSR 사이트를 지원하지만 SSR 를 권장하고 있습니다

## SSR(Server Side Rendering)

![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/8f69954f-59b6-403a-b031-8f8c6c2362b2)

![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/0622dc2c-3d3c-4ee6-8771-1720835b645d)

![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/1b34b6fb-a4ee-4d1a-aec6-6c371593dfee)


SSR 은 Server 에서 이미 Rendering 된 HTML 을 받아오는 방식입니다. 다른 사이트로 이동시, Server 에 리소스를 다시 요청을 해야합니다.

1. Browser 가 Server 에게 리소스를 요청합니다.
2. Server 는 Rendering 이 된 HTML 을 Browser 로 보냅니다
3. 이 HTML 에 script 태그를 연결합니다.
4. Client 는 완성된 화면을 이용하게 됩니다.

### Pros

- HTML 파일이 이미 완성된 채로 전달되기 때문에 SEO 에 좋습니다.
- Crawler는 페이지를 Indexing하기 위해 페이지에 관한 많은 정보가 필요한데, SSR을 활용하여 미리 페이지를 빌드를 하면 Crawler에게 많은 정보를 줄 수 있습니다.
- 초기 로딩 속도가 빠릅니다.

### Cons

- 페이지를 요청할때마다 Server 에 요청을 하기 때문에 Server 에 부하가 큽니다.
- 완성된 HTML을 JS 보다 먼저 받아오기 때문에 완성된 HTML 으로 화면은 확인 되지만 JS 다운로드가 늦어진다면 기능이 먹통일 경우가 발생할 수 있습니다.
- header나 footer처럼 중복되는 내용도 다시 렌더링 하여 받아야 하기 때문에 초기진입은 CSR보다 빠를지언정 페이지 이동은 SSR이 느린편에 속합니다.
- 페이지를 요청할때마다 새로고침 되기 때문에 깜빡임이 생기고, 때문에 사용자 경험이 떨어질 수 있습니다.
- TTV(Time to view) !== TTI(Time to interactive)

## CSR vs SSR 요약

### Rendering

CSR은 Client(Browser) 에서 JS를 사용하여 페이지를 Rendering 하는 방식이고

SSR은 Server 에서 페이지를 Rendering 하고 Client 전달하는 방식입니다. 

### Loading & SEO

CSR은 초기 로딩이 느리지만, 빠른 페이지 전환이 가능하며 서버 부하가 적습니다

반면, SSR은 초기 로딩이 빠르고 SEO(Search Engine Optimization)에 유리하지만, 서버 부하가 많이 발생할 수 있습니다. 

## Next.js

![image](https://github.com/limjoohyun2030/CS-study/assets/39722436/8d1af773-d639-4e4b-b00e-367de0836ddd)


> Next.js ****는 Vercel에서 만든 SSR 을 위해 만든 React Framework 입니다.
> 

React는 SPA를 만들기 위한 라이브러리인데 CSR방식을 채택한 SPA에서 어떻게 SSR을 한다는 것일까요?

### Next.js의 작동방식

1. Clinet 가 초기 페이지에 접속을 한 경우, Server는 SSR 방식으로 렌더링 될 HTML을 보냅니다.
2. Browser 에서 JS를 다운로드 받고 React를 실행합니다.
3. Client 가 페이지와 상호작용을 하며 다른 페이지로 이동할 경우, CSR 방식으로 Server가 아닌 Browser 에서 처리합니다.

초기 페이지 로딩이 빠르다는 SSR의 장점과, 다른 페이지 이동이 빠르다는 CSR의 장점을 혼합한 방식입니다. 

- 개발자가 빌드할 때 사전 렌더링 페이지를 만듭니다.
- 그래서 사이트가 배포 되고나면 사전 렌더링한 페이지는 변경 되지 않습니다.
- 사전 렌더링 페이지를 변경해야 한다면 빌드를 새로해서 재 배포를 해야 합니다.
- Static에 캐시되어 사용되기 때문에 빠른 응답이 가능합니다.

## SSG(Static Side Generation)

Client 에서 필요한 페이지들을 사전에 미리 준비해뒀다가, 요청을 받으면 이미 완성된 파일을 단순히 반환하는 기법입니다.

Next.js 에서 권장하는 Rendering 방식입니다.

> **데이터 가져오는 SSG**
> 
> 
> react에서는 useEffect통해 데이터를 가지고 온다. 그러나 Nextjs에서 useEffect를 사용하면 SSG로 작동하지 않는다. Nextjs에서 SSG를 하려면, Nextjs에서 제공하는 getStaticProps나 getStaticPaths를 사용한다.
> 

### SSR vs SSG

SSR은 요청시 서버에서 즉시 HTML을 만들어서 응답하기 때문에 데이터가 달라져서 미리 만들어 두기 어려운 페이지에 적합합니다.

반면 SSG 미리 다 만들어두고 요청시에 해당 페이지를 응답하기 때문에 바뀔 일이 거의 없어서 캐싱해두면 좋은 페이지에 사용됩니다.

[Next.js의 SSG (Static Site Generation), SSR (Server-Side Rendering)을 하는 이유 및 사용법](https://velog.io/@picpal/Next.js의-SSG-Static-Site-Generation-SSR-Server-Side-Rendering을-하는-이유-및-사용법)

### 참고링크

https://hahahoho5915.tistory.com/52

https://imagineu.tistory.com/57

[https://velog.io/@ksh4820/SPA-CSR과-SSR-SEO](https://velog.io/@ksh4820/SPA-CSR%EA%B3%BC-SSR-SEO)

[https://velog.io/@attosisss_/CSR-과-SSR의-차이-그리고-SEO](https://velog.io/@attosisss_/CSR-%EA%B3%BC-SSR%EC%9D%98-%EC%B0%A8%EC%9D%B4-%EA%B7%B8%EB%A6%AC%EA%B3%A0-SEO)

https://proglish.tistory.com/216

https://www.startupcode.kr/company/blog/archives/12

https://ajdkfl6445.gitbook.io/study/web/csr-vs-ssr-vs-ssg

[https://velog.io/@skypedanny/NextJS-그게-뭔데](https://velog.io/@skypedanny/NextJS-%EA%B7%B8%EA%B2%8C-%EB%AD%94%EB%8D%B0)

https://minsoftk.tistory.com/68

https://parkhyejung-portfolio.oopy.io/b092afb5-8a23-43a7-bf57-9fc49a8d8c3e

[https://velog.io/@picpal/Next.js의-SSG-Static-Site-Generation-SSR-Server-Side-Rendering을-하는-이유-및-사용법](https://velog.io/@picpal/Next.js%EC%9D%98-SSG-Static-Site-Generation-SSR-Server-Side-Rendering%EC%9D%84-%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0-%EB%B0%8F-%EC%82%AC%EC%9A%A9%EB%B2%95)
