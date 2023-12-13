# Virtual DOM

### Browser Rendering

![Untitled](https://github.com/limjoohyun2030/CS-study/assets/39722436/fc04bc9d-42a3-495a-aa9e-5068b3833cec)

1. 개발자가 작성한 HTML 을 브라우저 렌더 엔진이 해석하고 파싱해서 DOM tree 로 만듭니다.
2. css 스타일도 파싱해서 CSSOM tree 를 만듭니다.
3. 이 두 개의 트리를 합쳐서 Render tree 를 만듭니다
4. Render tree 가 생성된 이후에 Layout 과정이 이루어지는데, 이때 각 노드들이 화면에 그려질 크기와 위치를 계산합니다.
5. Layout 에서 계산한 대로, Painting 하면서 화면에 요소들을 그려줍니다.

위처럼 DOM rendering 과정을 설명한 이유는,

DOM 을 직접 조작하여 화면을 업데이트 하는건

시간과 비용이 많이 드는 일이라는걸 보여주기 위해서입니다.

### SSR  vs  CSR

![Untitled 1](https://github.com/limjoohyun2030/CS-study/assets/39722436/d4481ef1-24d7-4d38-bd23-f6a4a41ca166)

옛날에는 정적인 페이지를 많이 사용했지만, 

최근에는 동적으로 반응하는 CSR 페이지들이 많아졌습니다

예를 들어, 인스타그램에서 하트 표시를 누르면

하트 색깔만 바뀌어도 충분하고, 이 페이지 전부 리로드 될 필요는 없습니다.

### Virtual DOM

![Untitled 2](https://github.com/limjoohyun2030/CS-study/assets/39722436/b34f0a68-fe6d-4aa3-9b3f-e3e90052c022)

**Virtual DOM 은 UI 의 이상적인 또는 가상적인 표현을 메모리에 저장하고**

**ReactDOM 과 같은 라이브러리에 의해 실제 DOM 과 동기화하는** 

**프로그래밍 개념입니다.**

```jsx
1. Virtual DOM 의 장점 중 하나는 직접 DOM을 조작하지 않아도 된다는 점이다.
2. DOM 의 update를 Batch로 처리로 실제 DOM의 리렌더링 연산을 최소화 할 수 있다는 점이다. 
즉, 연쇄적으로 Reflow, Repaint가 발생하는 것을 줄이고, 
필요한 연산을 한번에 묶어서 처리하게 전달하게 된다.
3. state가 변경되었을 때, 어느 부분이 변경되었는지 추적하는 것 보다 
Virtual DOM을 새로 생성하여 비교하는 것이 효율적이다.
```

Virtual DOM 은 DOM 노드 트리를 복제한 자바스크립트 객체이며

class, style 등의 속성들은 있지만, 화면에 직접적인 변화를 줄 수 있는,

document.getElementById 같은 DOM api 메서드는 없습니다.

### Virtual DOM 동작

페이지가 랜더링 되면, DOM 노드 트리를 복사해서 virtual dom 을 생성합니다.

그 이후에 state 에 변화가 생길 때마다 새로운 virtual dom tree 를 만드는데

diffing 알고리즘을 통해, 기존 DOM tree 와 비교하고

(Reconciliation(재조정) 단계랑 비슷하게 봐도 무방)

```jsx
diff(previous:VTree, current:VTree) -> PatchObject
```

(위에 PatchObject 가 아래에 적용됨)

달라진 점을 실제 DOM 에 업데이트 합니다.

```jsx
patch(rootNode:DOMNode, patches:PatchObject) -> DOMNode newRootNode
```

patching 알고리즘을 통해, 변경사항(PatchObject) 만을 실제 노드(DOMNode) 에 반영합니다.

이렇게 변화가 생길 때마다 DOM tree 를 만드는 게 비효율적이라고 생각할 수 있는데

기존 DOM 조작에서 비싼 시간과 비용이 드는건 랜더링 과정에서 드는 것입니다

하지만 virtual dom 은 매번 다시 랜더링을 하는 것이 아니라 

메모리 상에서, 변경된 부분에 대해 트리를 변경하는 일이기 때문에 빠른 작업이 가능합니다.

결론적으로, virtual dom 을 통해

DOM 조작을 할 때마다, 랜더링 하는 비효율적인 방법 대신

변화를 전부 diff 로 감지해서 virtual dom 에 적용시키고

patch 로 이 변경 점을 실제 Node 에 적용을 시킵니다

변경 점 때문에 다시 랜더링을 해야 한다면 

그 부분만 랜더링을 해줌으로서 불필요한 랜더링을 줄이고 효율을 높입니다

### React JSX 에서 virtual DOM

React 에서 쓰는 JSX 는 JS 가 아니기 때문에, 

```jsx
const element = <h1 title="foo">Hello</h1>;
```

브라우저가 해석하기 위해 Babel 같은 툴로 JS 로 변환이 됩니다.

```jsx
const element = React.createElement(
  "h1",
  {title: "foo"},
  "Hello"
);
```

createElement 를 통해 객체로 변환이 되는데

```jsx
const element = (
  type: "h1",
  props: {
    title: "foo",
    children: "Hello",
  },
);
```

type 은 DOMNode 의 태그 이름이 되고

props 는 JSX 태그에 포함된 모든 속성이 됩니다.

children 은 하위 노드가 됩니다.

이렇게 생성된 객체를 이용해서 virtual DOMTree 를 만들고

render 를 통해, element 를 실제 DOM(container) 에 적용합니다

```jsx
const container = document.getElementById("root");
ReactDOM.render(element, container);
```

## **Reconcilation(재조정)**

```jsx
const element = (
  type: "h1",
  props: {
    title: "foo",
    children: "Hello",
  },
);
```

type 이 같으면? 

→ 속성만 변경

type 이 다르면?

→ 트리 재생성

재조정은 Key prop 와 관련이 있습니다

```jsx
//before
<ul>
  <li>A</li>
  <li>B</li>
</ul>

//after
<ul>
  <li>C</li>
  <li>A</li>
  <li>B</li>
</ul>
```

위처럼 <li> 가 맨 위에 추가가 되면

React 는 모든 요소가 제자리에 있다고 생각하지 않아서

모든 자식 요소를 다시 그리게 되는 비효율이 발생합니다.

이런 비효율을 막기 위해 React 는 key prop 을 사용합니다.

### Key prop

자식 노드들이 key prop 을 가지고 있으면

**React 는 key 값으로, 변경 이전 트리와 변경 이후 트리를 비교합니다.**

key 를 쓰면 위 예시에서 <li>C</li> 가 맨 위에 추가돼도

자식 노드를 전부 그리지 않게 됩니다.

**key 는 ‘변경되지 않는 유일한 값’ 이어야 합니다.**

그러므로, 값이 달라질 수 있는 배열의 index 는 key 값으로 사용하면 안됩니다.

```jsx
이렇게 하면 안됩니다.
//before
<ul>
  <li key="0">A</li>
  <li key="1">B</li>
</ul>

//after
<ul>
  <li key="0">C</li>
  <li key="1">A</li>
  <li key="2">B</li>
</ul>
```

key 값으로는 변하지 않는 값을 넣어줘야 하고

이래야 React 가 제대로 처리를 하고, 불필요한 DOM 업데이트를 하지 않습니다.

```jsx
이렇게 key 는 변하지 않는 값으로
//before
<ul>
  <li key="id0">A</li>
  <li key="id1">B</li>
</ul>

//after
<ul>
  <li key="id2">C</li>
  <li key="id0">A</li>
  <li key="id1">B</li>
</ul>
```

참고 링크

[https://www.youtube.com/watch?v=PN_WmsgbQCo&t=536s](https://www.youtube.com/watch?v=PN_WmsgbQCo&t=536s)

[https://www.youtube.com/watch?v=1ojA5mLWts8](https://www.youtube.com/watch?v=1ojA5mLWts8)

[https://www.youtube.com/watch?v=6rDBqVHSbgM&t=9s](https://www.youtube.com/watch?v=6rDBqVHSbgM&t=9s)

[https://velog.io/@namezin/CSR-SSR](https://velog.io/@namezin/CSR-SSR)

[https://velog.io/@whow1101/Virtual-Dom이란-Reconciliation](https://velog.io/@whow1101/Virtual-Dom%EC%9D%B4%EB%9E%80-Reconciliation)
