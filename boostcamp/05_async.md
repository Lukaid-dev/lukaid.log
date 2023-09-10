# 비동기 프로그래밍

드디어 올것이 왔다. 함수형 프로그래밍과 함께, 나에게는 정말정말 어려웠던 개념이다. 후... 쉼호흡을 다시하고 정리를 시작해보자.

우선 js에서 비동기 프로그래밍을 알기 위해서는 다음의 키워드를 알아야 한다.

- 비동기 처리 문법
- 병렬처리와 스레드
- 스레드 풀
- 이벤트 큐
- 이벤트 핸들러
- Promise
- Event Emitter

몇가지는 들어봤고 몇가지는 뭔지 모르겠다..

<br>

## 1. javascript 언어의 특징

javascript는 Single-Threaded, non-blocking, asynchronous, concurrent language이다. (싱글 스레드, 논 블로킹 비동기적 동적 언어)
그리고 call stack, event loop, callback queue, web api(libuv) 등의 개념이 있다.

js는 싱글스레드 언어이다. js는 놀랍게도, 다른 코드를 실행시키는 동안 Ajax 요청을 할 수 없다(?!). setTimeout도 물론 마찬가지다. 근데 우리는 브라우저를 사용하는동안, js로 만들어진 서버를 사용하는 동안 이런 동시성(concurrency)를 경험해 왔다. 그렇다면 js는 어떻게 동시성을 경험을 제공 할 수 있었을까? 이를 위해 js는 비동기성(asynchronous)을 사용한다.

js가 동기적 코드를 실행할 동안, 비동기적 코드는 다른 스레드(web api 등)에서 실행된다. 그리고 비동기적 코드가 완료되면, 콜백함수가 호출되어 동기적 코드가 실행된다. 이러한 과정을 관리해주는 주체가 Event Loop이다. Event Loop의 역할은 Call Stack과 Task Queue(Callback Queue)를 관리하는 것이다.

브라우저는 그렇다고 치고 Node는 어떨까? Node.js는 C++로 작성된 런타임이고 그 내부에 V8 Engine를 가지고 있다. 그 덕분에 크롬과 같은 브라우저에서 실행하던 자바스크립트를 로컬에서 실행할 수 있다. 그런데 그 내부에는 V8 Engine 말고도 libuv 라는 라이브러리가 존재한다. (최근에 Node.js 백엔드 개발자 되기 라는 책에서 배운 내용이다.)

libUV란 C++로 작성된, Node.js가 사용하는 비동기 I/O 라이브러리다. 이는 사실 운영체제의 커널을 추상화한 Wrapping 라이브러리로 커널이 어떤 비동기 API를 지원하는지 알고있다.

다시 말해 우리가 libuv 에게 파일 읽기와 같은 비동기 작업을 요청하면 libuv는 이 작업을 커널이 지원하는지 확인한다. 만약 지원한다면 libuv가 대신 커널에게 비동기적으로 요청했다가 응답이 오면 그 응답을 우리에게 전달해준다. 만약 요청한 작업을 커널이 지원하지 않는다면 어떻게 할까? 바로 자신만의 워커 스레드가 담긴 스레드 풀을 사용한다.

<br>

## 2. 비동기 처리 문법

[츨처](https://youtu.be/ZOVG7_41kJE)

Javascript는 Single Threaded다! JS는 어느 한 순간에 동시에 단 하나의 작업만을 실행 할 수 있다.

```js
function longWork() {
  const now = new Date();
  const milliseconds = now.getTime();
  const afterTwoSeconds = milliseconds + 2 * 1000;

  while (new Date().getTime() < afterTwoSeconds) {}

  console.log("완료");
}

console.log("Hello");
longWork(); // bound가 걸리는 작업
console.log("World");

>>> Hello
>>> 완료
>>> World
>>> [Done] exited with code=0 in 2.126 seconds
```

이를 비동기로 처리하면

```js
function longWork(){
    // 비동기로 설계된 함수
    setTimeout(()=>{ // setTimeout은 비동기 함수이다. web api로 옮겨진다. 그리고 js의 callstack으로 callback 함수를 옮겨준다.
        console.log('완료');
    }, 2000);
}

console.log('Hello');
longWork();
console.log('World');

>>> Hello
>>> World
>>> 완료
>>> [Done] exited with code=0 in 2.056 seconds
```

JS Engine은 싱글 스레드이고 하나의 Memory Heap과 Call Stack이 존재한다. Call Stack은 함수의 실행을 기록하는 자료구조이다. 함수가 실행되면 Call Stack에 쌓이고 함수가 종료되면 Call Stack에서 제거된다. Call Stack은 LIFO(Last In First Out)의 구조를 가지고 있다.

JS 실행환경에는 JS Engine뿐 아니라 Web API, Task Queue(혹은 Event Queue)가 존재한다. Web API는 브라우저에서 제공하는 API로, DOM, Ajax, Timeout 등이 있다. (주로 시간이 걸리는 작업을 처리한다.) Web API는 비동기 함수를 Call Stack에서 제거하고, 비동기 함수가 종료되면 Task Queue(혹은 Event Queue)로 옮긴다. Task Queue(혹은 Event Queue)는 비동기 함수의 콜백함수를 담고 있다. Task Queue(혹은 Event Queue)는 비동기 함수가 종료되면 Call Stack으로 옮긴다. Node에서 Web API는 Node API로 대체된다.

JS Engine은 싱글 스레드이기 때문에 하나의 Call Stack만을 가지고 있고, 하나의 작업이 실행되면 다른 작업은 대기해야 한다. 이를 해결하기 위해 비동기 처리를 사용한다.

만약에 Call Stack에 비동기 함수가 적재된다면, 해당 함수가 실행될 때 Task Queue(혹은 Event Queue)로 옮겨진다. 따라서 JS의 싱글 스레드에 있는 Call Stack에는 동기적으로 실행 될 함수만 존재하게 된다. 비동기 함수는 Task Queue에 존재하게 되고, Task Queue에 있는 비동기 함수가 종료되었을 때, 그리고 Call Stack이 비어있을 때 비동기 함수가 Call Stack으로 옮겨진다. 즉 비동기 함수는 Call Stack이 비어있을 때 실행된다. 다음의 코드를 확인해보자.

```js
function longWork() {
  const now = new Date();
  const milliseconds = now.getTime();
  const afterTwoSeconds = milliseconds + 2 * 1000;

  while (new Date().getTime() < afterTwoSeconds) {}

  console.log("동기 완료");
}

function aLongWork() {
  setTimeout(() => {
    console.log("비동기 완료");
  }, 2000);
}

console.log("Hello");
aLongWork(); // Task Queue로 옮겨지며 Call Stack을 비워줌
longWork();
console.log("World");

>>> Hello
>>> 동기 완료
>>> World // 동기 함수가 완료되어 Call Stack이 완전히 비었을때
>>> 비동기 완료 // 동작이 완료된 비동기 함수를 Call Stack으로 옮겨서 실행

>>> [Done] exited with code=0 in 2.057 seconds
```

<br>

## 3. Callback Hell

```js
function waitAndRun() {
  setTimeout(() => {
    console.log("1번 콜백 끝");
    setTimeout(() => {
      console.log("2번 콜백 끝");
      setTimeout(() => {
        console.log("3번 콜백 끝");
      }, 2000);
    }, 2000);
  }, 2000);
}

waitAndRun();
```

<br>

## 4. Promise

Promise란, 비동기 작업이 맞이할 미래의 완료 또는 실패와 그 결과 값을 나타내는 객체이다. Promise는 비동기 작업이 끝나고 나서 실행할 작업을 설정할 수 있고, 비동기 작업이 성공하면 성공한 값을, 실패하면 실패한 이유를 알려준다.

```js
const promise = new Promise((resolve, reject) => {
  // 비동기 작업을 수행한다.
  setTimeout(() => {
    resolve("성공"); // 성공 시
    reject("실패"); // 실패 시
  }, 2000);
});

promise
  .then((result) => {
    // promise의 resolve가 실행되면 실행, resolve에서 넘겨준 값이 result로 들어온다.
    console.log(result); // 성공한 경우 실행
  })
  .catch((error) => {
    // promise의 reject가 실행되면 실행, reject에서 넘겨준 값이 error로 들어온다.
    console.log(error); // 실패한 경우 실행
  })
  .finally(() => {
    console.log("무조건 실행");
  });
```

<br>

## 5. Promise를 반환하는 함수

```js
const getPromise = (seconds) =>
  new Promise((resolve, reject) => {
    setTimeout(() => {
      // if(xxx){
      //     resolve('성공')
      // }else{
      //     reject('에러');
      // }
      resolve("성공");
      resolve("에러");
    }, seconds * 1000);
  });

getPromise(3)
  .then((res) => {
    console.log("--- first then ---");
    console.log(res);
  })
  .catch((res) => {
    console.log("--- first catch ---");
    console.log(res);
  })
  .finally(() => {
    console.log("--- finally ---");
  });

// chaining
getPromise(3)
  .then((res) => {
    console.log("--- first then ---");
    console.log(res);

    return getPromise(3);
  })
  .then((res) => {
    console.log("--- second then ---");
    console.log(res);

    return getPromise(3);
  })
  .then((res) => {
    console.log("--- third then ---");
    console.log(res);
  });

// Promise.all : 여러개의 비동기 작업을 동시에 실행, Promise들이 서로 의존하고 있지 않다면 이 방법이 좋다.
Promise.all([getPromise(1), getPromise(4), getPromise(1)]).then((res) => {
  console.log(res);
});
```

<br>

## 6. async/await

Promise도 Call Back Hell 보다는 낫지만 직관적이진 않다.

async/await은 Promise를 더욱 쉽게 사용할 수 있게 해주는 문법이다. async/await은 Promise를 기반으로 동작한다. async/await은 Promise의 상위호환으로 볼 수 있다.

```js
const getPromise = (seconds) =>
  new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("완료");
    }, seconds * 1000);
  });

async function runner() {
  try {
    const result1 = await getPromise(1);
    console.log(result1);
    const result2 = await getPromise(2);
    console.log(result2);
    const result3 = await getPromise(1);
    console.log(result3);
  } catch (e) {
    console.log("---catch e---");
    console.log(e);
  } finally {
    console.log("---finally---");
  }
}

runner();

>>> 완료
>>> 완료
>>> 완료
>>> ---finally---
```

<br>

## 7. Thread Pool

[출처](https://prefer2.tistory.com/entry/Nodejs-nodejs-%EB%8F%99%EC%9E%91-%EB%B0%A9%EC%8B%9D-thread-thread-pool)
[출처](https://www.korecmblog.com/node-js-event-loop/)

위에서 서술한 바와 같이 network bound나 cpu bound등 시간이 많이 걸리는 로직을 처리할때는, js engine의 main thread에서 처리하지 않고, 별도의 thread에서 처리한다. 이때, 별도의 thread를 관리하는 것이 thread pool이다. thread pool은 thread를 미리 만들어 놓고, 필요할 때마다 thread를 가져다 쓰고, 사용이 끝나면 다시 thread pool에 반납한다. 이렇게 thread pool을 사용하면 thread를 생성하고 삭제하는 비용이 줄어들어 성능이 향상된다.

thread pool은 libuv 라이브러리가 node.js에 제공하는 기능으로 네개의 스레드를 추가적으로 제공한다. Event Loop는 무거운 작업들을 thread pool이 처리하게끔 할당한다. libuv 에게 파일 읽기와 같은 비동기 작업을 요청하면 libuv는 이 작업을 커널이 지원하는지 확인한다. 만약 지원한다면 libuv가 대신 커널에게 비동기적으로 요청했다가 응답이 오면 그 응답을 우리에게 전달해준다. 만약 요청한 작업을 커널이 지원하지 않는다면 어떻게 할까? 바로 자신만의 워커 스레드가 담긴 스레드 풀을 사용한다. (실제로 node를 실행하면 그 아래의 여러 개의 node 스레드가 존재하는 것을 확인할 수 있다.) libuv는 기본적으로 4개의 스레드를 가지는 스레드 풀을 생성한다. 물론 uv_threadpool라는 환경 변수를 설정해 최대 128개까지 스레드 개수를 늘릴 수도 있다. 만약 우리가 요청한 작업을 커널이 지원하지 않는다면 libuv는 커널을 호출하는 대신 이 스레드 풀에게 작업을 맡겨버린다.

- libuv는 운영체제의 커널을 추상화해서 비동기 API를 지원한다.
- libuv는 커널이 어떤 비동기 API를 지원하고 있는지 알고 있다.
- 만약 커널이 지원하는 비동기 작업을 libuv에게 요청하면 libuv는 대신 커널에게 이 작업을 비동기적으로 요청해준다.
- 만약 커널이 지원하지 않는 비동기 작업을 libuv에게 요청하면 libuv는 내부에 가지고있는 스레드 풀에게 이 작업을 요청해준다.

```js
const fs = require("fs");
const crypto = require("crypto");

setTimeout(() => console.log("Timer 1 finished"), 0);
setImmediate(() => console.log("Immediate 1 finished"));

const start = new Date();
//process.env.UV_THREADPOOL_SIZE = 1; //change thread pool size

fs.readFile("test-file.txt", () => {
  console.log("I/O finished");

  console.log("------------");
  setTimeout(() => console.log("Timer 2 finished"), 0);
  setTimeout(() => console.log("Timer 3 finished"), 3000);
  setImmediate(() => console.log("Immediate 2 finished"));

  // thread가 4개라서 4개가 비슷한 시간 동안에 수행됨
  crypto.pbkdf2("password", "salt", 100000, 1024, "sha512", () => {
    console.log(Date.now() - start, "password encrypted");
  });
  crypto.pbkdf2("password", "salt", 100000, 1024, "sha512", () => {
    console.log(Date.now() - start, "password encrypted");
  });
  crypto.pbkdf2("password", "salt", 100000, 1024, "sha512", () => {
    console.log(Date.now() - start, "password encrypted");
  });
  crypto.pbkdf2("password", "salt", 100000, 1024, "sha512", () => {
    console.log(Date.now() - start, "password encrypted");
  });
  crypto.pbkdf2("password", "salt", 100000, 1024, "sha512", () => {
    console.log(Date.now() - start, "password encrypted");
  });
});

console.log("Hello from the top-level code");
```

<br>

## 8. libuv

Node.js는 I/O 작업을 자신의 메인 스레드가 아닌 다른 스레드에 위임함으로써 싱글 스레드로 논 블로킹 I/O를 지원한다. 다르게 말하면 Node.js는 I/O 작업을 libuv에게 위임함으로써 논 블로킹 I/O를 지원하고 그 기반에는 이벤트 루프가 있다.

- libuv는 Node.js의 I/O 작업을 위한 멀티플랫폼 C 라이브러리이다.
- libuv는 Node.js의 I/O 작업을 위해 사용되는 스레드 풀을 관리한다.
- libuv는 Node.js의 이벤트 루프를 구현한다.

이벤트 루프는 Node.js가 여러 비동기 작업을 관리하기 위한 구현체다. console.log("Hello World")와 같은 동기 작업이 아니라 file.readFile('test.txt', callback)과 같은 비동기 작업들을 모아서 관리하고 순서대로 실행할 수 있게 해주는 도구이다.

<br>

## 9.Event Handler

Event Handler는 이벤트가 발생했을 때 호출되는 함수다. 이벤트 핸들러는 비동기 프로그래밍의 한 형태로, 이벤트 핸들러 함수는 즉시 호출되지 않고, 이벤트가 발생할 때 호출된다. 작업이 완료되면, 결과가 프로그램에 제시된다. 이를 통해 브라우저에서 제공하는 많은 함수들이 오래 걸릴 수 있음에도 불구하고 비동기적으로 실행될 수 있다. 브라우저의 onClick, onLoad, onKeyPress 등의 함수들이 대표적인 예이다.

<br>

## 10. Event Emitter

[출처](https://velog.io/@ragnarok_code/EventEmitter%EB%9E%80)

EventEmitter는 특정 이벤트에 리스너 함수를 달아서, 이벤트가 발생했을 때 이를 캐치할 수 있도록 만들어진 api이다. 이 동작은 일반적으로 이벤트 리스너가 원래 등록된 이벤트 핸들러보다 나중에 호출되기 때문에 비동기처럼 보인다.

이벤트란 '어플리케이션 내에서 발생한 응답 가능한 사건'이라고 표현할 수 있다. 이벤트는 Node.js에서만 사용되는 개념은 아니지만 Node에서는 특히 아키텍쳐의 근간을 이루는 개념이기에 중요하게 다뤄야 한다.

> Node.js에서 발생하는 이벤트는 두 종류로 나눌 수 있다. 먼저 시스템 이벤트가 있는데, 이것은 libuv 라이브러리가 작용된 C++코어에서 처리하게 된다. 파일을 열고 닫거나 인터넷이 연결되거나 하는 영역이다. 자바스크립트 코어에서 처리되는 보다 상위 단계의 이벤트는 Node.js의 Event Emitter에서 관리된다.

```js
const EventEmitter = require("events");
const myEmitter = new EventEmitter();

myEmitter.on("myEvent", () => console.log("handler1: myEvent was fired!"));
myEmitter.on("myEvent", () => console.log("handler2: myEvent was fired!"));
myEmitter.on("myEvent", () => console.log("handler3: myEvent was fired!"));

myEmitter.emit("myEvent");
console.log("I am the last log line");
```

## 정리

js의 비동기에 대해 비교적 자세히 알아보았다. 정말 몰랐던 개념들도 있었고 어렴풋 알고는 있었지만 다시 한번 알게된 개념도 있었다. 이번 기회를 통해 js가 내부적으로 어떻게 동작하는지 잘 이해 할 수 있게 되었고, 코드를 짤 때나, 디버깅을 할 때 훨씬 더 도움이 될 것 같다.
