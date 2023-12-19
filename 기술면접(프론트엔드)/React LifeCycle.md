# React LifeCycle
![Untitled](https://github.com/limjoohyun2030/CS-study/assets/39722436/19753448-b1ea-4f65-a769-e620bf0444dc)


## 리액트 라이프 사이클(생명주기)
![Untitled 1](https://github.com/limjoohyun2030/CS-study/assets/39722436/9d993515-d246-4158-af29-b832b62a1c52)

위는 class 방식 라이프 사이클

![Untitled 2](https://github.com/limjoohyun2030/CS-study/assets/39722436/d039e1ac-c81d-4152-9215-c7e4e060c826)

위는 함수 방식의 라이프 사이클(React 16.8 Hooks 업데이트 이후로 함수형에서도 라이프 사이클 다룰 수 있게 됨)

리액트는 컴포넌트 기반의 View를 중심으로 한 라이브러리입니다. 

그러므로 각각의 컴포넌트에는 라이프사이클

즉, **컴포넌트의 수명 주기**가 존재합니다. 

컴포넌트의 수명은 보통 페이지에서 **렌더링되기 전인 준비 과정에서 시작하여** 

**페이지에서 사라질 때 끝**이 납니다.

라이프 사이클을 다루는 것은 

**컴포넌트가 생겨나고, 변화하고, 없어지는 일련의 프로세스를** 

**프로그래머가 통제하는 것**을 뜻합니다.

컴퓨터의 자원은 한정적이기 때문에, 역할이 끝나면 모든 메모리를 반환해야

메모리 누수에 대한 문제가 생기지 않고, 더 좋은 성능을 발휘할 수 있습니다.

---

# Class Component 생명주기

## 🍳마운트(mount)

컴포넌트가 로딩되고, DOM이 생성되고, 웹 브라우저 상에서 나타나는 것을 뜻합니다.

### 🥐constructor(클래스 생성자)

클래스 인스턴스 객체를 생성하고 초기화하는 메서드로 

리액트에서 컴포넌트를 만들 때 처음으로 실행됩니다. 

이 메서드에서는 초기 state를 정할 수 있습니다.

`this.props`, `this.state`에 접근이 가능하고 리액트 요소를 반환합니다.

```jsx
//class
class Example extends React.Component {
	constructor(props) {
      	super(props);
      	this.state = { count: 0 } ;
    }
}

// Hooks
const Example = () => {
  const [count,setCount] = useState(0); // Hooks 에서는 useState 로 쉽게 state설정
}
```

### 🥐shouldComponentUpdate

props나 state를 변경했을 때, **리렌더링을 할지 말지 결정하는 메서드**입니다.

이 메서드에서는 **반드시 `true`나 `false`를 반환**해야합니다.

기본적으로 `true`를 반환하지만, 조건에 바꿔서 `false`를 반환하도록 하면 

해당 조건에는 `render` 함수를 호출하지 않습니다.

이 메서드는 **오직 성능 최적화**만을 위한 것이며 

렌더링 방지 목적으로 사용하게 된다면 버그가 생길 수 있습니다.

```jsx
//Class
class Example extends React.Component {
  shouldComponentUpdate(nextProps) {
    return nextProps.value !== this.props.value
  }
}

// Hooks
const Example = React.memo((prevProps, nextProps) => {
  return nextProps.value === prevProps.value
	}                        
)
```

**Hook**에서 보통`React.memo`,  `useMemo`  등을 이용하면 렌더링 성능을 개선할 수 있습니다.

### 🥐render

실제 로딩이 이뤄지는 단계이며, 컴포넌트를 렌더링하는 메서드입니다.

component에서 작업한 결과물을 return 합니다.

render 메서드가 실행되면 JSX 가 HTML 로 변환이 되고, 웹 브라우저 상에 나타나게 됩니다

`render` 메서드는 컴포넌트가 로딩될 때에도 실행되지만 

**컴포넌트의 데이터 (state, props)가 업데이트 되었을 때에도 동작**합니다. 

`render` 메소드 내부에 setState나 props를 변화시키는 메소드를 쓰면 

무한루프가 일어나게 될 수 있으니 주의가 필요합니다.

```jsx
// Class
class Example extends React.Component {
  render() {
    return <div>컴포넌트</div>
  }
}

// Hooks
const example = () => {
  return <div>컴포넌트</div>
}
render() {
	return (
      <div>
        <header className="title">
          Yuntroll React
        </header>
      </div>
	);
}
```

### 🥐componentDidMount

**Mounting의 마지막 부분**입니다.

**초기 컴포넌트의 로딩 이후에 한번만 실행** 되는 라이프사이클 메소드입니다.

이제 여기서 DOM을 직접 조작 ****할 수 있게 됩니다. 

주로 아래와 같은 경우에 사용되는데

- D3, masonry 처럼 DOM 을 사용해야 하는 외부 라이브러리 연동을 하거나,
- 해당 컴포넌트에서 필요로 하는 데이터를 요청하기 위해 axios, fetch * 등을 통하여 ajax 요청을 하거나,
- DOM 의 속성을 읽거나 직접 변경하는 작업을 진행합니다.
    
    → DOM이 렌더링 되기 전에 DOM에 접근하게 되면 에러가 발생하기 때문입니다.
    

```jsx
// Class
class Example extends React.Component {
	componentDidMount() {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then((response) => response.json())
      .then((users) => this.setState({ users, loading: false }));
  }
	...
}

// Hooks
const Example = () => {
    useEffect(() => {
        ...
    }, []);
}
```

## 🍳업데이트(updating)

컴포넌트가 업데이트되는 시점에 어떤 생명주기 메서드들이 호출되는지 알아봅니다.

업데이트는 **4가지 상황** 에서 발생합니다.

1. Props가 바뀔 때
2. State가 바뀔 때
3. 부모 컴포넌트가 리렌더링 될 때
4. this.forceUpdate 로 강제로 렌더링을 trigger 하는 경우
    
    → input이 달라지니 output도 달라져야 하기 때문입니다.
    
    → 상위 component가 update되면 하위 component도 rendering되고 다시 mount됩니다.
    

### 🥐shouldComponentUpdate

`props`나 `state`를 변경했을 때, **리렌더링을 할지 말지 결정하는 메서드**입니다.

Mount 에서 설명한 메서드와 같은 메서드입니다.

```jsx
shouldComponentUpdate(nextProps, nextState) {
  // return false 하면 업데이트 ❌
  // return this.props.checked !== nextProps.checked
  return true;
}
```

### 🥐componentWillUpdate

**새로운 속성이나 상태를 받은 후 렌더링 직전에 호출**합니다. 

초기 렌더링 시에는 호출되지 않습니다.

shouldComponentUpdate가 false를 반환하면 해당 메서드는 실행되지 않습니다.

```jsx
componentWillUpdate(nextProps, nextState){...}
```

### 🥐componentDidUpdate

**컴포넌트의 변경이 완료되었을 때 수행되는 메소드**입니다. 

처음 렌더링 될 때는 실행되지 않고, 리 렌더링을 완료한 후 실행합니다. 

**`state`나 `props`가 변경되어 DOM 변화가 발생한 뒤 발생됩니다.**

prevProps 또는 prevState를 사용하여 

컴포넌트가 이전에 가졌던 데이터에 접근이 가능합니다.

```jsx
// Class
class Example extends React.Component {
    componentDidUpdate(prevProps, prevState) {
        ...
    }
}
```

## 🍳언마운트(unmount)

Unmounting은 **DOM에서 제거**되는 것을 뜻합니다. 

JSX에 포함되었다가 이후에 제거되는 경우에 발생합니다.

### 🥐componentWillUnmount

타이머를 제거하거나, DOM 요소를 정리하거나, 

`componentDidMount`에서 연결한 이벤트를 제거할 수 있습니다. 

컴포넌트가 화면에서 사라지기 직전에 호출합니다.

만약에 `setTimeout`을 걸은 것이 있다면 `clearTimeout`을 통하여 제거를 합니다.

```jsx
// Class
class Example extends React.Component {
    coomponentWillUnmount() {
        ...
    }
}

// Hooks
const Example = () => {
    useEffect(() => {
        return () => {
            ...
        }
    }, []);
}
```

참고 링크

[https://velog.io/@minbr0ther/React.js-리액트-라이프사이클life-cycle-순서-역할](https://velog.io/@minbr0ther/React.js-%EB%A6%AC%EC%95%A1%ED%8A%B8-%EB%9D%BC%EC%9D%B4%ED%94%84%EC%82%AC%EC%9D%B4%ED%81%B4life-cycle-%EC%88%9C%EC%84%9C-%EC%97%AD%ED%95%A0)

[https://velog.io/@remon/React-리액트-라이프-사이클](https://velog.io/@remon/React-%EB%A6%AC%EC%95%A1%ED%8A%B8-%EB%9D%BC%EC%9D%B4%ED%94%84-%EC%82%AC%EC%9D%B4%ED%81%B4)
