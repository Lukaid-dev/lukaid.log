# promise-callback-async-await 총정리

<br>

JavaScript에서 비동기 프로그래밍을 다루는데 사용되는 여러 가지 개념들이 있는데, 그 중에서도 Promise, Callback, Async, Await는 아주 중요한 개념들이라 한방에 정리해보자!!

<br>

이번 내용은 같이 부캠에서 만난 훌륭한 동료들인 [지은님](https://velog.io/@mayowall), [용현님](https://velog.io/@dongind), [민형님](https://velog.io/@song961003)과 함께하는 면접 스터디에서 같이 공부한 내용을 정리한 것이다. 같이 공부하면서 많은 도움이 되었고, 더 나아가서 같이 성장할 수 있는 좋은 동료들을 만나서 너무 행복하다. 앞으로도 같이 성장해보자!!

<br>
<br>

## Promise의 정의

<br>

먼저 가장 핵심적인 개념인 Promise에 대해 알아보자.

Promise는 비동기 작업을 수행할 때 사용되는 **객체**이다. Promise는 비동기 작업이 맞이할 미래의 완료 또는 실패와 그 결과 값을 나타낸다.

<br>

### Promise가 필요한 이유

<br>

JavaScript는 싱글 스레드 기반의 언어이다. 따라서 한번에 한가지의 일 밖에 하지 못하는데, 서버와 클라이언트간의 통신이나 setTimeout등 시간이 오래 걸리면서, 메인 스레드가 직접적으로 작업에 관여 하는 것이 아닌 작업들은 비동기적으로 처리된다.

<br>

비동기적으로 처리된다는 것은 어떤 의미일까? 해당 작업이 끝나기 전에 다음 작업을 수행한다는 의미이다. 하지만 싱글 스레드 기반인 JavaScript는 한번에 한가지의 일 밖에 하지 못한다. 따라서, 비동기적으로 처리되는 작업들은 메인 스레드가 직접적으로 관여하지 않고, Web API에게 위임한다.

<br>

- Web API : 브라우저에서 제공하는 API로, DOM, Ajax, setTimeout 등이 있다.

<br>

Web API에게 위임된 작업들은 해당 작업이 끝나면, **Callback Queue**에 해당 작업을 담아놓는다. 이 때, 메인 스레드는 다른 작업을 수행하고 있다가, **Callback Queue**에 담겨있는 작업들을 하나씩 꺼내와서 실행한다.

<br>

이때, 비동기 작업의 리턴값을 바로 받아오고자 하면, 비어있는 값을 가지고 오기 때문에 오류가 발생하거나 빈 화면이 뜨게된다. 이를 해결하기 위해 비동기 데이터의 내용을 바로 받아오는 것이 아니라, “미래의 비동기 작업이 끝난 시점에 결과를 제공하겠다는 약속(Promise)”을 대신 반환하게 된다.

<br>

### Promise의 상태

<br>

Promise는 3가지의 상태를 가진다.

<br>

- 대기(pending) : 이행하지도, 거부하지도 않은 초기 상태. 아직 비동기 처리가 완료되지 않아, 미래에 해당 결과를 제공하겠다는 약속을 의미
- 이행(fulfilled) : 연산이 성공적으로 완료됨
- 거부(rejected) : 연산이 실패

<br>

### Promise 사용하는 법

<br>

Promise 생성자를 통해 Promise 객체를 생성할 수 있다. 이 때, 인자로 `resolve`와 `reject`를 전달 받는 `executor(실행함수)`를 전달받는다.

<br>

- `executor`는 프로미스 구현에 의해 resolve와 reject 함수를 받아 즉시 실행(실행 함수는 Promise 생성자가 생성한 객체를 반환하기 전에 호출)
- 실행 함수가 비동기 작업을 포함한 모든 작업을 끝내면 `resolve`를 호출해 프로미스를 이행
- 실행 함수의 작업 중 오류가 발생할 경우 `reject`를 호출해 거부하며, 이 경우 실행 함수의 반환값과 상관없이 Promise는 거부

<br>

```jsx
function fetchData() {
  return new Promise(function (resolve, reject) {
    // 비동기 작업 수행 (보통 서버 통신)
    setTimeout(function () {
      const success = true;

      if (success) {
        const data = "Hello, world!";
        resolve(data);
      } else {
        reject("Error occurred");
      }
    }, 1000);
  });
}

fetchData()
  // 성공 시 resolve의 결과값을 받아옴
  .then(function (result) {
    console.log(result);
  })
  // 실패 시 reject의 결과값을 받아옴
  .catch(function (error) {
    console.error(error);
  });

// Hello, world!
```

<br>

### Promise.all() 에 대해 설명해주세요

<br>

Promise.all()은 인자로 Promise를 요소로 가지는 순환 가능한 객체를 받는다.

Promise.all()이 실행되면, 순환 가능한 객체 내의 모든 Promise를 병렬로 처리하여, 모든 Promise가 성공적으로 처리되거나 객체 내 Promise가 최초로 거부될 때까지 대기

- 모든 Promise가 성공한다면, 각각 Promise의 이행값을 모두 모아놓은 배열을 반환
  - 결과의 배열의 순서는 인자로 전달된 객체 내 프로미스의 순서를 유지
- Promise의 거부가 발생하면, 최초로 거부된 Promise의 사유를 그대로 반환한다.

<br>

```jsx
function fetchData() {
  return new Promise(function (resolve, reject) {
    // 비동기 작업 수행 (보통 서버 통신)
    setTimeout(function () {
      const success = true;

      if (success) {
        const data = "Hello, world!";
        resolve(data);
      } else {
        reject("Error occurred");
      }
    }, 1000);
  });
}

Promise.all([fetchData(), fetchData()])
  .then(function (result) {
    console.log(result);
  })
  .catch(function (error) {
    console.error(error);
  });

// ["Hello, world!", "Hello, world!"]
```

<br>
<br>

## Callback 콜백 함수란?

<br>

콜백은 함수를 다른 함수에 인자로 전달하여 나중에 호출되도록 하는 패턴으로, 주로 비동기 작업에서 사용되어, 작업이 완료되면 해당 콜백 함수를 호출한다.

<br>

### 비동기 함수를 처리할 때, Promise와 Callback의 차이

<br>

사실 Callback은 Promise처럼 특정한 객체나 구조를 의미하는 것은 아니다. 콜백은 함수를 다른 함수의 인자로 전달하여 나중에 호출되도록 하는 것을 의미하며, 이는 비동기적인 작업의 완료나 이벤트 발생 등에 대한 응답을 처리하는 데 사용되는 기법이다.

비동기 함수를 연속적으로 처리하기 위해 Callback과 Promise를 둘 다 사용할 수 있다. 하지만, Callback은 비동기 함수의 연속 처리에 있어서 가독성이 떨어지고, 에러 핸들링이 어렵다는 단점이 있다.

<br>

- **가독성**
  - Callback만을 사용하여 비동기를 처리하게 될 경우 Callback의 인수로 새로운 Callback을 넣게 되면 들여쓰기가 계속해서 생성
    - 흔히 말하는 `콜백 지옥`(혹은 `콜백 피라미드`)가 생성
  - 각 Callback의 결과가 어떤 형태로 어떻게 다음 Callback으로 전달되는지 해석하는데 다소 어려울 가능성이 있음
  - Promise의 Chaining을 사용하여, 각각의 Callback을 하나의 **then**에 할당하여 처리할 수 있다.
    - then : 해당 Promise의 반환값을 통해 새로운 Promise를 생성하는 메서드
  - then을 통하여 각각의 Callback이 어떻게 동작하는지 인지하기 쉬우며, 들여쓰기가 없어 깔끔한 코드를 읽을 수 있음

<br>

```jsx
// Callback Hell
fetchData(function (data1) {
  process1(data1, function (data2) {
    process2(data2, function (data3) {
      // ...
    });
  });
});

// Using Promise
fetchData()
  .then(process1)
  .then(process2)
  .then(function (data3) {
    // ...
  });
```

<br>

- **에러 핸들링**
  - 콜백 함수의 중첩의 경우, 비동기 함수의 에러 처리에 어려움이 있다.
    - 콜백 함수를 매 실행할 때마다, 실패할 경우를 대비하는 에러 핸들링 콜백 함수를 넣어주어야 한다.
    - 또한 상기의 경우, 에러가 발생하게 되면 복수의 에러 핸들링 콜백 함수가 실행
  - Promise의 경우, catch라는 메서드를 통해 에러 핸들링을 간단하게 처리 할 수 있다.
    - catch : then(null, failureCallback)의 축약 메서드
    - catch를 사용할 경우, 연속적인 비동기 콜백 함수들을 하나의 catch만으로 에러 핸들링이 가능하다.
    - 최초에 발생한 에러만이 catch로 이동하여 한번의 에러 콜백 함수만 실행
    - 또한 catch에 새로운 then을 체이닝하여, catch 이후에도 연속적인 작업이 가능하다.

<br>

```jsx
fetchData()
  .then(function (data) {
    // Success
    return processData(data);
  })
  .catch(function (error) {
    // Handle error
    console.error(error);
  });
```

<br>
<br>

## async/await의 정의

<br>

`async/await`는 `Promise` 로직을 더 쉽게 사용하기 위해 ES2017 부터 도입된 문법이다. `async/await`는 내부적으로 Javascript의 `Promise` 를 기반으로 동작하는 사용자가 쉽게 비동기 코드를 작성하도록 하는 문법이지, `Promise`를 대체하기 위한 새로운 기능은 아니다. (이러한 개념을 보통 `Syntactic sugar`라고 한다.)

<br>

### async/await의 사용 방법

<br>

- function 키워드 앞에 `async`를 붙여주어, “**해당 함수에는 `await`를 사용할 것이야!”**라고 선언
- 비동기로 처리하는 부분 앞에 await를 사용하여 “**여기서 이 비동기의 결과를 기다리도록**” 처리
- Promise 코드의 예시를 아래의 async/await 처럼 작성이 가능하다.

<br>

```jsx
function sample(ms) {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log(`${ms} 밀리초가 지났습니다.`);
      resolve();
    }, ms);
  });
}
```

<br>

```jsx
// Promise.then
function main() {
  delay(1000)
    .then(() => {
      return delay(2000);
    })
    .then(() => {
      return Promise.resolve("끝");
    })
    .then((result) => {
      console.log(result);
    });
}

main();
```

<br>

```jsx
// async/await
async function main() {
  await delay(1000);
  await delay(2000);
  const result = await Promise.resolve("끝");
  console.log(result);
}

main();
```

<br>

### async 키워드

<br>

> async를 붙인 함수 내부에 await를 사용하기 위한 선언문

<br>

**async의 리턴값은 Promise**

- async 함수 내에 `return 1`을 할 경우, 일반 함수와 같이 1을 반환하는 것이 아니라 이행 상태(fulfilled)와 결과값으로 1을 가지는 `Promise` 를 반환하는 것을 확인

  ```jsx
  async function func1() {
    return 1;
  }

  const data = func1();
  console.log(data); // Promise { 1 }
  ```

<center>
  <img src="https://file.notion.so/f/f/88d8f7ec-c29e-4286-aad0-9b660662d1a4/f21ec36c-f05d-4e2c-81ad-210a7d52e9f0/Untitled.png?id=e0e2e4b5-c5ea-4b4a-9821-94de11d52970&table=block&spaceId=88d8f7ec-c29e-4286-aad0-9b660662d1a4&expirationTimestamp=1705298400000&signature=NPNz8_Z2uYkJOWOnyOuI7gMrers1BjV3Nu1DSKLqaKQ&downloadName=Untitled.png"
style="width: 50%; height: 50%;">
</center>

<br>

- 만약, `async` function에 아무런 반환값을 작성하지 않더라도 `undefined`의 결과값을 가지는 `Promise`를 반환

  ```jsx
  async function func2() {}

  const data2 = func2();

  console.log(data2);
  ```

<center>
  <img src="https://file.notion.so/f/f/88d8f7ec-c29e-4286-aad0-9b660662d1a4/62a64556-5f2b-4188-bf64-cf87096f6669/Untitled.png?id=86fcca6f-5cf7-4ae6-9e8c-eea2e273422e&table=block&spaceId=88d8f7ec-c29e-4286-aad0-9b660662d1a4&expirationTimestamp=1705298400000&signature=ANM5Wa2ZNp9mgv0kdRe1IUhVvAa4kOtphjGg5zVzQ0Y&downloadName=Untitled.png"
style="width: 50%; height: 50%;">
</center>

<br>

- async 함수의 반환값은 Promise이기 때문에 then 핸들러를 부착할 수 있음

  ```jsx
  async function func1() {
    return 1;
  }

  const data = func1();

  data.then((result) => {
    console.log(result);
  }); // 1
  ```

<br>

### await 키워드

> `await` 연산자는 `async function` 내부에서만 사용할 수 있는 연산자로, `Promise`를 기다리기 위해 사용

<br>

- await 문의 Promise가 fulfill/reject 될 때까지 async 함수의 실행을 일시 정지
- 만약, Promise가 fulfill 되면, async 함수를 일시 정지한 부분부터 실행
  - 이 때, await 문은 Promise의 fulfill 된 값을 반환한다.
- await의 값이 Promise가 아닐 경우, 해당 값을 resolved Promise로 변경

  ```jsx
  async function func1() {
    await 1;
  }

  function func1() {
    return Promise.resolve(1).then(() => undefined);
  }

  // 위의 두 코드는 동일하게 작동
  ```

<br>
<br>

## Promise vs async/await

마지막으로 담백하게 Promise와 async/await의 차이점을 정리해보자.

<br>

### 1. 구문적 차이

- `Promise`는 `.then()`과 `.catch()`를 사용하여 비동기 코드를 처리
- `async/await`는 `async` 함수 내부에서 `await` 키워드를 사용하여 비동기 함수를 처리
  - `await`는 비동기 작업이 완료될 때까지 기다리며, 이를 통해 코드를 동기적으로 처리

<br>

### 2. 가독성 및 유지 보수성

- `Promise`의 `.then()`과 `.catch()`의 경우 `Promise` 체인을 통해 중첩된 구조를 가지게 될 경우 코드의 구조를 이해하기에 어려움이 발생할 여지가 있음
- `async/await` 코드는 동기적으로 코드를 작성할 수 있어 가독성/유지보수성에 용이함을 가짐

<br>

### 3. 오류 처리

- `Promise` 구문은 `.catch()`를 사용하여 오류를 처리
- `async/await` 구문은 동기 구문과 같이 `try/catch` 구문을 통해 오류를 처리

<br>

### 4. 반환값

- `Promise`는 직접 `Promise` 객체를 생성하고, 결과를 반환
- `async` 함수 내부의 return을 통해 `Promise`의 결과를 반환

<br>

### 5. 이벤트 루프 처리 과정에서

- `Promise`의 경우
  1. `Promise` 내부 실행 함수 코드가 콜스택에 적재되어 실행
  2. `.then()` 핸들러의 콜백 함수가 이벤트 루프에 의해 Microtask Queue에 적재
  3. 모든 콜스택이 비게 되면, Microtask Queue 내의 콜백 함수가 콜스택으로 이동하여 실행

<br>

- `async/await`의 경우
  1. `async` 함수의 내부 코드가 `await` 구문을 만날 때까지 콜스택에서 실행
  2. `await` 구문을 만나게 되면 `async` 함수는 동작을 중단하고, 콜스택에서 빠져 나와 나머지 부분은 Microtask Queue로 이동
  3. 콜스택에 모든 함수가 실행될 경우, Microtask Queue에 남아있는 `async` 함수는 콜스택으로 이동하여 마저 실행

<br>

```jsx
const one = () => Promise.resolve("One!");

async function myFunc() {
  console.log("In function!"); // await 구문 전까진 동기적으로 실행
  const res = await one(); // 해당 부분부터 Microtask Queue로 이동
  console.log(res); // 모든 동기 코드 실행 후, 동작
}

console.log("Before function!");
myFunc();
console.log("After function!");

// 실행 결과
// Before function!
// In function!
// After function!
// One!
```

<br>
<br>

## 정리

<br>

- `Promise`는 비동기 작업을 처리하기 위한 객체
- `async/await`는 `Promise`를 더 쉽게 사용하기 위한 문법
- `async/await`는 `Promise`를 대체하는 것이 아닌, `Promise`를 기반으로 동작하는 문법
- `async/await`는 `Promise`의 `.then()`과 `.catch()`를 대체하는 것이 아닌, `try/catch`를 사용하여 오류를 처리

<br>

이렇게 그동안 헷갈렸던 Promise, Callback, Async, Await에 대해 정리해보았다. 부스트캠프 챌린지 과정의 16개 주제 중에 가장 어려웠던 주제 두가지를 꼽으라면, 첫번째가 함수형 프로그래밍이었고, 두번째가 비동기에 관한 내용이었다.

나는 객체지향과 동기적 프로그래밍에 익숙해져 있었는데, 브라우저에서 비동기적으로 처리되는 작업들을 이해하는데는 상당히 시간이 걸렸던 것 같다. 이번에 면접 스터디를 하면서 한번 더 확실하게 정리 한 것 같다!

## 참고자료

<br>

- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise
- https://medium.com/@flqjsl/%EC%BD%9C%EB%B0%B1%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-56c26e1f1bc3
- https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EB%B9%84%EB%8F%99%EA%B8%B0%EC%B2%98%EB%A6%AC-async-await
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/async_function
- https://inpa.tistory.com/entry/%F0%9F%94%84-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84-%EA%B5%AC%EC%A1%B0-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC#async/await_%EC%98%A4%ED%95%B4%EC%99%80_%EC%A7%84%EC%A7%9C_%EB%8F%99%EC%9E%91
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/await
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/async_function
