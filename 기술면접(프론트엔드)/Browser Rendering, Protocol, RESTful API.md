
## Rendering 

### 1. Browser 가 url을 입력받으면, DNS를 통해 ip주소로 변환하고, 해당 ip 주소를 가진 서버에게 Resources 를 요청
<br>

### 2. Webkit, Blink 같은 Browser Rendering Engine 이 Server로부터 받은 HTML, CSS 를 Parsing 

<br>

### 3. HTML Parser가 HTML을 Parsing 해서 DOM(Document Objecy Model) tree 를 만듬

<br>

### 4. CSS Parser가 CSS를 Parsing 해서 CSSOM(CSS Object Model) tree 를 만듬

<br>
->HTML,CSS 모두 서버로부터 바이트 형태로 전달을 받습니다. 
이 바이트 형태를 문자열로, 문자열을 토큰으로, 토큰을 노드로 바꾸고, 
이 노드들을 DOM tree, CSSOM tree로 만들고, 이 두개의 tree를 합쳐서 Render tree로 만듭니다.
<br>
<br>
이때 알아야할 점은, 화면에 렌더링되는 노드들만 구성이 된다는 점인데
화면에 렌더링 되지않은 display:none 이나 meta 태그등은 렌더 트리에 포함되지 않습니다
<br>
<br>

### 5. HTML Parser가 Parsing 할때 link 나 style 같은 CSS 태그를 만나면 블로킹되고, CSSOM 생성쪽으로 넘어감 
CSSOM 생성이 마무리되면, 블로킹 되었던 시점부터 다시 HTML Parsing을 진행합니다.

<br>

### 6. HTML Parser가 Parsing 할때 script 태그를 만나면 제어권한을 JS Runtime Engine으로 넘김
JS Engine은 JS를 Parsing 해서 AST(Abstract syntax tree)를 만듭니다.
<br>
<br>
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

### 6.5 Script 태그는 동기적 파싱. 해결법은 async defer?
Script 태그는 동기적으로, 즉 위에서 아래로 읽어내려가면서 Parsing 을 하기 때문에
script 태그의 위치에 따라 HTML Parsing 이 지연될 수 있습니다.
그래서 script 태그는 가능하면 body 태그의 맨 아래쪽에 위치하는게 좋다고 생각할 수 있습니다.

<br>
<br>

그러나 script 태그를 body 태그 맨 아래에 두어도 문제가 있습니다.
이미 화면상에 보이는 HTML 태그에서 만든 컴포넌트들의 상호작용이 작동하지 않을 수 있습니다(버튼 클릭, 텍스트입력등)

async, defer 속성을 이용해서 JS코드를 비동기적으로 불러옴으로서, DOM 렌더링의 블로킹을 방지할 수 있습니다.

<br>
(추가 공부 필요)
https://velog.io/@fepanbr/HTML-script%ED%83%9C%EA%B7%B8-defer-async
https://velog.io/@moonsun116/async-vs-defer
<br>

- async
JS 파일 로딩을 비동기적으로 처리한다.
JS 파일 로딩이 끝난 순서대로 파싱 및 실행이 이루어지며, 이 경우 HTML 파싱이 블로킹 처리되기에 순서가 보장되지 않는다.
- defer
async와 마찬가지로 JS 파일 로딩을 비동기적으로 처리한다.
JS 코드의 파싱 및 실행은 DOM 생성 이후에 실행된다.



### 7.DOM tree 와 CSSOM tree 를 합쳐서 Render tree 를 만듦
Render tree를 기반으로 
- HTML 요소의 Layout(컴포넌트들의 위치와 크기)을 계산하고
- Browser 화면에 HTML 요소를 Painting 합니다.
  
JS에 의해 노드가 추가되거나, Browser 창이 Resizing 된다면
reflow 와 repaint 이 발생합니다.

### 8. reflow, repaint (추가 공부 필요)
https://velog.io/@soulee__/Javascript-%EB%A6%AC%ED%8E%98%EC%9D%B8%ED%8A%B8-%EB%A6%AC%ED%94%8C%EB%A1%9C%EC%9A%B0


6.

<br>
<br>

## Protocol

<br>

## RESTful API

<br>

## Get, Post

<br>

## 주소창에 www.google.com 을 입력하면?
마지막으로 연결해서 설명
https://velog.io/@soyeon9819/%EC%9B%B9%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%97%90-www.google.com%EC%9D%84-%EC%B9%98%EB%A9%B4-%EC%9D%BC%EC%96%B4%EB%82%98%EB%8A%94-%EC%9D%BC
<br>


## 아직 참고 못한 링크
https://velog.io/@chappi/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%A5%BC-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90-30%EC%9D%BC%EC%B0%A8-%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-%EB%A0%8C%EB%8D%94%EB%A7%81-%EA%B3%BC%EC%A0%95


## 참고링크
- https://velog.io/@uncyclocity/JavaScript-%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%9D%98-%EB%A0%8C%EB%8D%94%EB%A7%81-%EA%B3%BC%EC%A0%95
- https://prod.velog.io/@championcom/CSSOM-AST
- https://gyujincho.github.io/2018-06-19/AST-for-JS-devlopers
- https://velog.io/@soulee__/Javascript-%EB%A6%AC%ED%8E%98%EC%9D%B8%ED%8A%B8-%EB%A6%AC%ED%94%8C%EB%A1%9C%EC%9A%B0
- 
