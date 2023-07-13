## 동기와 비동기 (Syncronous/Asyncronous)

### 정의
동기는 요청한 작업에 대해 완료 여부를 따져 순차대로 처리하는 방식이다.
요청한 작업이 끝나야 다음 작업이 이루어지기 때문에 어떤 작업이 수행 중이면 다음 작업은 대기하게 된다.

비동기는 기본 프로그램 흐름과 독립적인 이벤트를 발생하고, 이러한 이벤트를 처리하는 방법을 나타낸다.
동기와 반대로 요청한 작업에 대해 완료 여부를 따지지 않기 때문에 대기하지 않고 바로 다음 작업을 수행한다.

### 장단점
동기 방식  
- 설계가 간단하고 직관적
- 작업 순서가 보장  
- 작업이 완료될 때까지 대기

비동기 방식
- 느린 작업이 발생해도 대기하지 않고 다른 작업을 동시에 처리할 수 있어 효율적
- 복잡한 설계 (멀티 스레드/멀티 프로세스)
- 작업 순서가 보장X

## 블로킹/논블로킹 (Blocking/Non-Blocking)
다른 요청의 작업을 처리하기 위해 현재 작업을 block(차단, 대기) 하냐 안하냐의 유무를 나타내는 프로세스의 실행 방식이다.   

누가 제어권을 가지고 있냐로도 구분할 수 있다.
- 호출된 함수가 자신이 할 일을 모두 마칠 때까지 `제어권을 계속 가지고서` 호출한 함수에게 바로 돌려주지 않으면 `Block`
- 호출된 함수가 자신이 할 일을 채 마치지 않았더라도 `바로 제어권을 건네주어(return)` 호출한 함수가 다른 일을 진행할 수 있도록 해주면 `Non-block`


### 비동기와 논블록킹 차이
- 동기/비동기는 요청한 작업에 대해 완료 여부를 신경 써서 작업을 순차적으로 수행할지 아닌지에 대한 관점
- 블로킹/논블록킹은 단어 그대로 현재 작업이 block(차단, 대기) 되느냐 아니냐에 따라 다른 작업을 수행할 수 있는지에 대한 관점

### 케이스별로 확인하기
![Boost application performance using asynchronous I/O](./SyncronousAndAsyncronous/aio.gif)
- 동기 + 블로킹
   ```javascript
  function run(a, b) {
      return a + b
  }
  
  const result = run(1, 2);
  
  console.log("시작");
  console.log("결과:", result);
  console.log("끝");
  ```

- 비동기 + 논블로킹
  ``` javascript
  function run(a, b) {
      return a + b
  }
  
  let result;
  setTimeout(() => {
      result = run(1, 2);
  }, 1000);
  
  console.log("시작");
  console.log("결과:", result);
  console.log("끝");
  ```

- 동기 + 논블로킹  
 호출되는 함수는 바로 리턴하고, 호출하는 함수는 작업 완료 여부를 신경쓰는 것이다. 신경쓰는 방법이 기다리거나 물어보거나 두 가지가 있는데, Non-Blocking 함수를 호출했다면 사실 기다릴 필요는 없고 물어보는 일이 남는다.  
 즉, Non-Blocking 함수 호출 후 바로 반환 받아서 다른 작업을 할 수 있게 되지만, 함수 호출에 의해 수행되는 작업이 완료된 것은 아니며, 호출하는 함수가 호출되는 함수 쪽에 작업 완료 여부를 계속 묻는다.
  ```javascript
  someTask = asyncFunction();
 
  while(someTask.isDone()){
     // 작업 완료 여부를 계속 묻기
     // 다른 작업 수행 가능
  }
  // 작업 완료되면 작업 결과에 따른 작업 수행
  ```

- 비동기 + 블로킹  
  1. 의도된 동작이 아닌 경우  
     예시) Node.js와 MySQL 조합  
     NonBlocking-Async 방식을 쓰는데 그 과정 중에 하나라도 Blocking으로 동작하는 놈이 포함되어 있다면 의도하지 않게 Blocking-Async로 동작할 수 있다.
  2. 직관적인 코드의 흐름을 유지하면서 작업을 병렬적으로 처리하기 위함  
     예시) Linux/UnixS OS의 I/O 다중화 모델   

---

참고
- https://inpa.tistory.com/entry/%F0%9F%91%A9%E2%80%8D%F0%9F%92%BB-%EB%8F%99%EA%B8%B0%EB%B9%84%EB%8F%99%EA%B8%B0-%EB%B8%94%EB%A1%9C%ED%82%B9%EB%85%BC%EB%B8%94%EB%A1%9C%ED%82%B9-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC  
- https://musma.github.io/2019/04/17/blocking-and-synchronous.html 
- https://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/  
- https://cotak.tistory.com/136  

---

더 알아보기  
- https://incredible-larva.tistory.com/entry/IO-Multiplexing-%ED%86%BA%EC%95%84%EB%B3%B4%EA%B8%B0-1%EB%B6%80
