# Promise

![Untitled](https://github.com/limjoohyun2030/CS-study/assets/39722436/56f79db5-e1d3-4d10-b2cc-5ec7a7ac6603)

## Promise 란

```jsx
Promise 는 ES6 에서 나온 비동기 처리 전용 객체입니다
```

Promise 는 JS에서 비동기적인 처리를 도와주는 객체입니다.

JS 를 동기적으로 실행하면, 응답이 느릴때 다른 요청들을 처리하지 못하고,

사용자 경험이 떨어질 수 있습니다.

때문에, 자원을 효율적으로 실행하기 위해 비동기 방식이 필요해졌고

그 중에 Promise 가 있습니다.

## Callback 이란

Promise 이전에 사용한 비동기 방식 중 하나는 Callback 입니다

```jsx
function plusNum(n, callback) {
  setTimeout(() => {
    const increased = n + 1;
    console.log(increased);
    if (callback) {
      callback(increased); 
    }
  }, 1000);
}

plusNum(0, n => {
  plusNum(n, n => {
    plusNum(n, n => {
        plusNum(n, n => {
          console.log('finish!');
        });
    });
  });
});
```

Callback 은 JS 함수의 parameter 에 함수 자체를 넘겨주고,
그 함수 내부에서 parameter 함수를 실행하는 방법입니다.

그런데 callback 함수는 길어질수록 가독성이 많이 떨어지고

에러와 예외 처리가 어렵게 됩니다. 이러한 현상을 callback hell 이라고 합니다.

그래서 Promise 를 통해 비동기를 처리하는 쪽을 가게 됩니다

## Promise 예시

```jsx
function plusNum(n) {
  return new Promise((resolve, reject)=>{
    setTimeout(() => {
      const increased = n + 1;
      console.log(increased);
      resolve(increased);
    }, 1000)
  })
}

plusNum(0) // Promise 가 성공하면, resolve 의 매개변수 값이 then 의 인자로 들어감
  .then((n) => plusNum(n))
  .then((n) => plusNum(n))
  .then((n) => plusNum(n)); // chaining. Promise handler then을 연달아 사용
```

## Promise State

![Untitled 1](https://github.com/limjoohyun2030/CS-study/assets/39722436/c022eb32-8268-441c-afd4-a0a073c2acef)

새 Promise 객체를 생성 시

- state: pending(대기)
- result: undefined

실행 성공 시 resolve(value)

- state: fulfilled
- result: value

실행 실패 시 reject(error)

- state: rejected
- result: error

## Promise Object 생성

```jsx
//Promise 객체 생성
const myPromise = new Promise((resolve, reject) => {
    setTimeout(() => {
      const ranNum = Math.random();
      if (ranNum > 0.5) {
        resolve(`Success: ${ranNum}`);
      } else {
        reject(`Error: ${ranNum}`);
      }
    }, 1000);
  });
```

비동기 처리가 성공하면 Promise Object 에서 resolve 를 호출하게 되고

resolve 함수의 매개변수의 값이 then 메소드의 callback 함수 인자로 들어가게 됩니다.

## Promise Object 처리

```jsx
// Promise 처리
myPromise
  .then((result) => { // 성공적으로 수행했을 때 실행되는 코드
    console.log(result); // result 는 resolve(`Success: ${ranNum}`) 에서 `Success: ${ranNum}` 값
  })
  .catch((error) => { // 실패했을 때 실행되는 코드
    console.error(error); // error 는 reject(`Error: ${ranNum}`) 에서 `Error: ${ranNum}` 값
  })
  .finally(() => { // 성공이든 실패든 무조건 실행
    console.log('Done'); 
  });
```

## Promise 함수 등록

```jsx
function createPromise() {
  return new Promise((resolve, reject) => { //Promise Object 를 return
    setTimeout(() => {
      const ranNum = Math.random();
      if (ranNum > 0.5) {
        resolve(`Success: ${ranNum}`);
      } else {
        reject(`Error: ${ranNum}`);
      }
    }, 1000);
  });
}

// Promise 생성. 재사용이 편해짐. Promise Factory function
const myPromise = createPromise();

// Promise 처리
myPromise
  .then((result) => { // 성공적으로 수행했을 때 실행되는 코드
    console.log(result); // result 는 resolve(`Success: ${ranNum}`) 에서 `Success: ${ranNum}` 값
  })
  .catch((error) => { // 실패했을 때 실행되는 코드
    console.error(error); // error 는 reject(`Error: ${ranNum}`) 에서 `Error: ${ranNum}` 값
  })
  .finally(() => { // 성공이든 실패든 무조건 실행
    console.log('Done'); 
  });
```

Promise Object 를 함수로 감싸서 주로 사용합니다.

Promise Object 를 return해서 const 에 값으로 넣음으로서 재사용성을 늘릴 수 있습니다.

## Promise.all()

new Promise((resolve, reject)⇒) 

위처럼 생성자 함수를 만들고 사용할 수도 있지만, 

Promise 는 static method 를 사용할 수도 있습니다. 

static method 중에 유용하게 쓰이는 함수 중 하나가 Promise.all 인데

비동기 작업을 한번에 모두 처리해야 할 때 유용한 Promise static method 입니다. 

모든 Promise 의 비동기 처리가 fulfilled 될 때까지 기다려고

모든 Promise 가 fulfilled 되면, 그 때 then 핸들러가 실행됩니다.

가장 대표적인 사용 예시로 여러 개의 API 요청을 보내고 

모든 응답을 받아야 하는 경우에 사용할 수 있습니다.

```jsx
// 1. 서버 요청 API 프로미스 객체 생성 (fetch)
const api_1 = fetch("https://jsonplaceholder.typicode.com/users");
const api_2 = fetch("https://jsonplaceholder.typicode.com/users");
const api_3 = fetch("https://jsonplaceholder.typicode.com/users");

// 2. 프로미스 객체들을 묶어 배열로 구성
const promises = [api_1, api_2, api_3];

// 3. Promise.all() 메서드 인자로 프로미스 배열을 넣어, 모든 프로미스가 이행될 때까지 기다리고, 결과값을 출력
Promise.all(promises)
    .then((results) => {
      // results는 이행된 프로미스들의 값들을 담은 배열.
      // results의 순서는 promises의 순서와 일치.
      console.log(results); // [users1, users2, users3]
    })
    .catch((error) => {
      // 어느 하나라도 프로미스가 거부되면 오류를 출력
      console.error(error);
    });
```

all 이외에도

Promise.resolve(), Promise.reject(), Promise.any(), Promise.race() 등이 있습니다.

## Promise Hell?

Chaining을 통해 callback 에 비해 가독성이 좋아졌지만

Promise 도 then 이 길어지고, 코드 자체가 많아지면 가독성이 떨어지고, 예외 처리가 어려워집니다.

Promise 도 결국 비동기적인 코드이므로 가독성에 한계가 있습니다.

![Untitled 2](https://github.com/limjoohyun2030/CS-study/assets/39722436/e33f7fba-ddb6-4914-96a9-980222eea9e2)

## Async & Await

Promise 의 이러한 문제점들을 보완하기 위해 ES8 에 등장한게 Async & Await 입니다

내부적으로는 Promise 방식으로 비동기 처리를 하지만

소스 코드에서 간결한 문법으로 보여줘서 가독성을 높여줍니다.

```jsx
함수 앞에 async 를 붙이고, 비동기 처리 부분 앞쪽에 await 를 쓰면 됩니다.
```

```jsx
async function 자체와 await 은 모두 Promise object를 return 합니다
```

```jsx
// async로 비동기 함수 정의
async function fetchData() {
  try {
   // 비동기 처리 앞에 await. 비동기 처리가 완료될때까지 코드실행을 중지하고 기다림.
   // 완료되면 변수에 Promise 형태의 HTTP 응답 객체를 담는다
    const response = await fetch('https://jsonplaceholder.typicode.com/posts/1');
    if (!response.ok) { // HTTP 응답이 200대가 아니면
      throw new Error('Failed to get Data');
    }
    const data = await response.json();
    console.log('data:', data);
  } catch (error) {
    console.error('error:', error);
  }
}

// 비동기 함수 호출
fetchData();
```

참고링크

[https://www.youtube.com/watch?v=wzGX9CTsVBI&t=566s](https://www.youtube.com/watch?v=wzGX9CTsVBI&t=566s)

[https://www.youtube.com/watch?v=fsmekO1fQcw](https://www.youtube.com/watch?v=fsmekO1fQcw)

[https://inpa.tistory.com/entry/JS-📚-비동기처리-Promise](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EB%B9%84%EB%8F%99%EA%B8%B0%EC%B2%98%EB%A6%AC-Promise)

[https://inpa.tistory.com/entry/JS-📚-비동기처리-async-await](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EB%B9%84%EB%8F%99%EA%B8%B0%EC%B2%98%EB%A6%AC-async-await)

[https://inpa.tistory.com/entry/JS-📚-비동기처리-async-await](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EB%B9%84%EB%8F%99%EA%B8%B0%EC%B2%98%EB%A6%AC-async-await)
