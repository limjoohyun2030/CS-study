# Scope & Closure

## Scope란?

식별자(변수) 가 유효한 범위

## Scope 의 종류

1. Glocal scope
    
    어디에서든 참조 가능
    
2. Local scope
    
    자신의 지역 scope, 하위 scope 에서 사용 가능
    
    - Function Level Scope
    - Block Level Scope

### Function Level Scope

- **자바스크립트는 함수 스코프를 따르는 언어이고,**
    
    **var 변수와 함께 새 함수를 만들면 새 함수 스코프가 생성됩니다.**
    
- var 키워드 생략 허용하므로, 암묵적 전역 변수를 양산할 가능성이 크다.
- 변수 중복 선언 허용하므로, 의도하지 않은 변수값의 변경이 일어날 가능성이 크다.
- 변수를 선언하기 이전에 참조할 수 있는, 함수 호이스팅이 발생할 수 있다.
- 함수 내에서 선언된 변수는 함수 내에서만 유효하며 함수 외부에서는 참조할 수 없습니다.
    
    즉, 함수 내부에서 선언한 변수는 지역 변수이며 함수 외부에서 선언한 변수는 모두 전역 변수입니다.
    

### Block Level Scope

- **JS ES6 이전에 쓰던 var 대신, let 과 const 를 이용해서 Block Level Scope 사용 가능해짐**
- 대부분의 프로그래밍 언어들에서 지역 스코프를 생성하는 방법입니다.
- 모든 코드 블록(함수,if문,for문,while문,try/catch문 등 중괄호)내에서 선언된 변수는 코드 블록 내에서만 유효한 지역변수이며, 코드 블록 외부에서는 참조할 수 없습니다.

let 은 블록 안쪽 변수에 접근 불가능.

```jsx
var foo = 123; // 전역 변수

console.log(foo); // 123

{
  var foo = 456; // 전역 변수
}

console.log(foo); // 456
```

```jsx
let foo = 123; // 전역 변수

{
  let foo = 456; // 지역 변수
  let bar = 456; // 지역 변수
}

console.log(foo); // 123
console.log(bar); // ReferenceError: bar is not defined
```

var 는 중복선언 허용, let 은 불허

```jsx
var foo = 123;
var foo = 456;  // 중복 선언 허용

let bar = 123;
let bar = 456;  // Uncaught SyntaxError: Identifier 'bar' has already been declared
```

Hoisting. 선언을 맨 위로 올린것 같은 상황. var는 호이스팅으로 인해 선언 전 참조 가능. let 은 선언 이전에 참조 불가능

```jsx
console.log(foo); // undefined
var foo;

console.log(bar); // Error: Uncaught ReferenceError: bar is not defined
let bar;
```

### → 변수의 유효 범위가 함수인지, 블록인지에 대한 차이

---

### Dynamic Scope

Lexical Scope 와 반대 개념. 함수를 어디서 호출했는지에 따라 함수의 상위 스코프를 결정하는 방식

### Lexical Scope

**함수를 어디에 정의 했는지에 따라 상위 스코프를 정적으로 결정** 방식하는 방식

JS에서는 함수를 정의하고, 함수 객체 생성한 그 순간의 상위 스코프 기억합니다.

이렇게 되는 이유는, 함수가 호출될 때마다 함수 상위 스코프를 참조할 필요가 있기 때문이다.

```jsx
var x = 1;

function foo(){
  var x = 10;
  bar();
}

function bar(){
  console.log();
}

foo(); // 1 
bar(); // 1
```

### → 변수의 유효 범위가 함수를 호출 시점인지, 정의 시점인지에 대한 차이

# Closure

JS 에만 있는 개념은 아닙니다

클로저는 함수와 함수가 선언된 어휘적 환경의 조합- by MDN

```jsx
const x = 1;

function outer(){
  const x = 10;
  const y = '안녕';
  const inner = function(){
    console.log(x);
  };
  return inner;
}

const result = outer();
result(); // 10
```

outer 함수가 실행되고나서 실행 컨텍스트에서 제거가 되고, 생명주기가 끝났으니 x = 10 이라는 것도 끊길 걸로 보였다

그러나, outer 함수가 제거되어도 그 안에 있던 지역변수 x 를 inner 함수가 참조할 수 있다면 이때 inner 함수를 클로저 라고 할 수 있다. 이 때의 x 는 자유변수라고 합니다.

반면, y 는 inner 함수에 의해 참조되지도 않고, outer 함수가 종료됨에 따라 가비지 컬렉터가 수집을 해갑니다. x 는 inner 쪽에서 참조를 하므로 가비지 컬렉터가 수집해 가지 않습니다. 

**외부함수의 생명주기가 끝났지만, 상위 스코프의 식별자를 참조하고 있다면, 그 내부함수를 클로저라고 할 수 있습니다.**

## Closure 가 아닌 예시

```jsx
function outer(){
  const x = 10;

  function inner(){
    const z = 3;
    console.log(z);
  }
  return inner;
}

const result = outer();
result(); 
```

inner 함수가 outer 함수보다 더 오래 살아있지만

inner 함수는 상위 스코프의 식별자(x 나 y)를 참조하지 않는 상황. 따라서 클로저가 아니다

참조도 하지 않는데 상위 스코프를 기억하면 메모리 낭비이므로 최적화를 통해 기억하지 않도록 한다

```jsx
function outer(){
  const x = 10;

  function inner(){
    console.log(x);
  }
  inner();
}
outer();
```

inner 함수가 상위 스코프 outer 의 식별자(변수)를 참조하지만 inner 함수의 생명주기가 더 짧다

클로저는 생명주기가 끝난 상위 스코프의 식별자를 내부함수가 참조해야 함

클로저가 아니다

## Closure 의 예시

```jsx
var funcs = [];

// 함수의 배열을 생성하는 for 루프의 i는 전역 변수다.
for (var i = 0; i < 3; i++) {
  funcs.push(function () { console.log(i); });
}

// 배열에서 함수를 꺼내어 호출한다.
for (var j = 0; j < 3; j++) {
  funcs[j](); // 3 이 세번 나온다. 0 1 2 로 나오지 않는다
}
```

```jsx
func[0] = function () { console.log(i); }
func[1] = function () { console.log(i); }
func[2] = function () { console.log(i); }

들어갔다가 for 문이 끝나는 순간 
var i = 3; 이 func[0], func[1], func[2] 에 들어가서
[3,3,3] 이렇게 되는거
```

이를 해결하려면, var 를 계속 쓰되 자유변수를 사용하거나

ES6 에 나온 let 을 쓰면 됩니다.

자유변수 예시

```jsx
var funcs = [];

// 함수의 배열을 생성하는 for 루프의 i는 전역 변수다.
for (var i = 0; i < 3; i++) {
  (function (index) { // index는 자유변수다.
    funcs.push(function () { console.log(index); });
  }(i));
}

// 배열에서 함수를 꺼내어 호출한다
for (var j = 0; j < 3; j++) {
  funcs[j]();
}
```

```jsx
(function (index) { // index는 자유변수다.
    funcs.push(function () { console.log(index); });
  }(0));
(function (index) { // index는 자유변수다.
    funcs.push(function () { console.log(index); });
  }(1));
(function (index) { // index는 자유변수다.
    funcs.push(function () { console.log(index); });
  }(2));
```

```jsx
funcs.push(function () { console.log(0); });
funcs.push(function () { console.log(1); });
funcs.push(function () { console.log(2); });

이때 index 로 받았던 값은, 생명주기가 끝난
function(index) 함수의 var i 값을 참조해서 받은 것이기
때문에 클로저의 활용 예시이다.
```

let 사용

```jsx
var funcs = [];

// 함수의 배열을 생성하는 for 루프의 i는 for 루프의 코드 블록에서만 유효한 지역 변수이면서 자유 변수이다.
for (let i = 0; i < 3; i++) {
  funcs.push(function () { console.log(i); });
}

// 배열에서 함수를 꺼내어 호출한다
for (var j = 0; j < 3; j++) {
  console.dir(funcs[j]);
  funcs[j]();
}
```

참고링크

[https://www.youtube.com/watch?v=PVYjfrgZhtU](https://www.youtube.com/watch?v=PVYjfrgZhtU)

[https://www.youtube.com/watch?v=xJtVVLPxgco&t=230s](https://www.youtube.com/watch?v=xJtVVLPxgco&t=230s)

[https://velog.io/@hyoniii_log/함수-레벨블록-레벨-스코프](https://velog.io/@hyoniii_log/%ED%95%A8%EC%88%98-%EB%A0%88%EB%B2%A8%EB%B8%94%EB%A1%9D-%EB%A0%88%EB%B2%A8-%EC%8A%A4%EC%BD%94%ED%94%84)

[https://poiemaweb.com/es6-block-scope](https://poiemaweb.com/es6-block-scope)

[https://eun-ng.tistory.com/16](https://eun-ng.tistory.com/16)

[https://poiemaweb.com/es6-block-scope](https://poiemaweb.com/es6-block-scope) - Hoisting 시 참고

[https://eun-ng.tistory.com/16](https://eun-ng.tistory.com/16)
