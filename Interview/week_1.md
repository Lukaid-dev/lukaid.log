부캠을 수료하고, 성공적인 면접을 위해 면접스터디를 진행하기로 했다. 할 게 많기도 하다.

<br>
<br>

# 1. 브라우저 렌더링 과정을 설명해 주세요. (브라우저는 어떻게 동작하나요?)

<br>

> 사용자는 로드가 빠르고 상호작용이 원활한 컨텐츠로 이루어진 웹 경험을 원한다. 이 두 가지 목표를 달성하기 위해 브라우저가 어떻게 동작하는지 이해하는 것이 중요하다.

<br>

> 대부분 브라우저는 싱글 쓰레드이다. 메인 쓰레드가 요청된 모든 작업을 수행하면서도 사용자와의 상호작용에 반응할 수 있도록 보장하기 위해서는 렌더링 시간이 가장 중요하다. 브라우저가 싱글 쓰레드로 동작한다는 점을 이해하고 가능한 메인 쓰레드의 책임을 줄여주는 방식으로 웹 성능 향상을 이룰 수 있다.

<br>

브라우저의 렌더링 과정을 이해하기 위해서는 Critical Rendering Path를 알아야 하는데, CRP는 브라우저가 하나의 화면을 그려내는 과정 또는 순서를 말한다.

<br>

웹페이지에서 HTML을 요청하면, 서버는 HTML response headers와 data를 반환한다. 그러면 브라우저는 HTML 구문 분석을 시작하여 수신된 바이트를 DOM 트리로 변환한다. 브라우저는 스타일시트, 스크립트 또는 임베디드 이미지 참조 등 외부 리소스에 대한 링크를 찾을 때마다 요청을 시작한다. (requests every time it finds) 이때 일부 요청들은 차단(blocking)될 수 있는데, asset 들기 위해 처리될 때까지 나머지 HTML의 구문 분석이 중단된다. DOM을 구축이 끝나면, CSS Object Model을 빌드한다. DOM과 CSSOM이 완성되면 브라우저는 렌더 트리(builds the render tree)를 빌드하여 표시되는 모든 콘텐츠의 스타일을 계산한다. 렌더 트리가 완성되면, 모든 렌더 트리 요소의 위치와 크기를 정의하는 레이아웃과정이 실행된다(layout occurs). 레이아웃이 완료되면 페이지가 렌더링되거나 혹은 화면에 '페인팅'된다.

<br>

이 과정을 간략히 하면 다음과 같다.

**1. HTML parsing** - 웹 브라우저는 HTML 파일을 다운로드하고 이를 파싱하여 페이지의 구조를 파악하여 DOM(Document Object Model)트리를 만든다.

**2. CSS parsing** - 웹 브라우저는 CSS 파일을 다운로드하고 이를 파싱하여 HTML 요소의 스타일을 지정하는 방법을 파악하여 CSSOM(CSS Object Model) 트리를 만든다.

**3. JavaScript parsing** - 웹 브라우저는 자바스크립트 파일을 다운로드하고 이를 파싱하여 페이지에 인터랙션을 추가하는 방법을 파악한다.

**4. Layout** - 웹 브라우저는 CSS 스타일링 및 문서 흐름에 따라 페이지에서 각 HTML 요소의 크기와 위치를 결정한다.

**5. Paint** - 웹 브라우저는 레이아웃 및 스타일링 정보에 따라 화면의 픽셀을 최종적으로 그린다.

<br>

<img src="https://media.geeksforgeeks.org/wp-content/uploads/20210728231006/111.png" alt="CRP">

<br>

출처: <br>
https://developer.mozilla.org/en-US/docs/Web/Performance/Critical_rendering_path <br>
https://developer.mozilla.org/ko/docs/Web/Performance/How_browsers_work <br>
https://www.geeksforgeeks.org/critical-rendering-path-flow/ <br>
https://wonism.github.io/critical-rendering-path/ <br>

<br>
<br>

# 2. HTML 렌더링 도중 JavaScript가 실행되면 렌더링이 멈추는 이유가 뭔가요?

<br>

바로 위에서 서술한 브라우저 렌더링 과정을 이해하면 자연스럽게 대답 할 수 있는 질문이다. 대부분의 웹 브라우저는 렌더링 엔진과 JavaScript 엔진을 하나의 메인 스레드에서 실행한다. 그렇다. 모든 원흉은 싱글 쓰레드에 있다.

<br>

렌더링 엔진은 HTML 한 줄씩 순차적으로 파싱하며 DOM을 열심히 만들다가 JavaScript를 만나면 DOM 생성을 중단한다. 자바스크립트 코드를 파싱하기 위해 자바스크립트 엔진에 제어권을 넘기고 파싱이 끝나면 다시 렌더링 엔진에 제어권을 넘겨 중단된 부분부터 HTML 파싱을 재개하며 DOM 트리를 생성한다.

<br>

추가로 자바스크립트는 DOM API 통해서 DOM 트리를 변경시킬 수 있다. 만약에 싱글스레드가 아니라고 한다면, 렌더 트리가 미처 구성되지 못했을 때, 자바스크립트 엔진이 DOM 트리 변경시킬 수도 있고 아직 그려지지 않은 DOM 트리의 노드에 접근한다면 오류를 뱉을 수도 있다.

<br>

그래서 `<script>` 태그를 `<body>` 태그 끝에 두거나, async를 사용하거나, defer속성을 추가하여 HTML을 모두 그리고 자바스크립트 엔진이 실행되도록 할 수 있다.

<br>
<br>

# 3. 클로저(Closure)에 대해 설명해주세요.

<br>

> 클로저는 주변 상태(어휘적 환경, Lexical scoping)에 대한 참조와 함께 묶인(포함된) 함수의 조합이다. 그러니까, 클로저는 내부 함수에서 외부 함수의 범위에 대한 접근을 제공한다. JavaScript에서 클로저는 함수 생성 시 함수가 생성될 때마다 생성된다.

<br>

무슨 말인지 진짜 어렵다... 처음 이 클로저 개념을 공부할 때 생소한 단어와 이해가 안 되는 문장들로 상당히 곤란했던 기억이 있다. 우선 어휘적 환경(Lexical scoping)은 뭘까? 어휘적 환경은 쉽게 말해서, 변수가 코드 내부 어디에 선언되었는지를 말하는 것이다. 호출되는 방식에 따라 동적으로 결정되는 this와는 다르게, 코드가 작성된 순간 정적으로 정해져 버린다. 클로저는 이러한 "선언된 어휘적 환경"을 조합해서 코딩하는 기법이라고 할 수 있다.

<br>

```js
function init() {
  var name = "Mozilla"; // name은 init에 의해 생성된 지역 변수이다.
  function displayName() {
    // displayName() 은 내부 함수이며, 클로저다.
    console.log(name); // 부모 함수에서 선언된 변수를 사용한다.
  }
  displayName();
}
init(); // Mozilla
```

<br>

위 코드에서 `init()`은 지역 변수 `name`과 함수 `displayName()`을 생성한다. 그렇다고 이들을 반환하는 것은 아니고, 변수 `name`과 함수 `displayName()`은 `init()`함수 본문(어휘적 환경)에서만 사용할 수 있다. `displayName()`은 함수 레벨의 스코프에서 변수를 가지고 있지 않지만, 클로저는 내부 함수에서 외부 함수의 변수에 접근할 수 있기 때문에 선언된 어휘적 환경에서 변수 `name`에 접근할 수 있게 된다.

<br>

당신이 react를 사용한다면, 아마도 거의 매일 이 클로저 개념을 사용할 가능성이 높은데, react의 다양한 hook들이 클로저로 선언되어 있고 가장 대표적으로 `useState`가 그렇다. `useState`를 아주 단순화해 보면 다음과 같다. (실제로 이렇게 구현되어 있지는 않았을 것이다.)

<br>

```js
function useState(initialValue) {
  let value = initialValue;
  const state = () => value;
  const setState = (newValue) => {
    value = newValue;
  };

  return [state, setState];
}
```

<br>

외부 함수 `useState()`가 반환한 내부 함수 `setState()`는 외부 함수 `useState()`의 호출이 끝났음에도 자신이 선언된 외부 함수가 선언된 어휘적 환경을 기억하기 때문에 계속해서 state 값을 사용할 수 있다.

<br>

출처: <br>
https://developer.mozilla.org/ko/docs/Web/JavaScript/Closures#%EC%96%B4%ED%9C%98%EC%A0%81_%EB%B2%94%EC%9C%84_%EC%A7%80%EC%A0%95lexical_scoping <br>
[모던 리액트 딥다이브](https://wikibook.co.kr/react-deep-dive/)

<br>
<br>

# 4. TCP와 UDP를 비교 설명해 주세요.

<br>

둘 다 OSI 7계층 모델에서 4계층, 전송계층 (Transport Layer)에 속하며 Transmission Control Protocol / User Datagram Protocol 이름에서 알 수 있듯 데이터를 전송하는 규약의 일종이다. 두 프로토콜은 모두 패킷을 한 컴퓨터에서 다른 컴퓨터로 전달해 주는 IP 프로토콜을 기반으로 구현되어 있고, 데이터를 효율적으로 전송하는 역할을 한다.

<br>

TCP는 Connection-oriented 즉 연결을 지향한다. 통신을 시작하기 전에 먼저 연결을 설정하고, 데이터 전송 후에는 명시적으로 연결을 해제한다. (3-way handshaking 과정을 통해 연결을 설정)

반면 UDP는 Connectionless하다. 연결을 설정하지 않고 데이터를 전송한다. 각 패킷은 독립적으로 처리되며, 개별적으로 처리된다.

<br>

그래서 TCP는 신뢰성이 있다. 데이터 전송 중 손실된 패킷을 다시 전송하고, 패킷의 순서를 보장하여 신뢰성 있는 통신을 제공한다.

반면 UDP는 데이터를 전송하고 신뢰성 있는 전송을 보장하지 않는다. 손실된 패킷이 있어도 이를 복구하지 않는다.

<br>

TCP는 수신자가 처리할 수 있는 만큼의 데이터만을 전송하여 흐름 제어를 하며, 네트워크 혼잡을 감지하고 조절하여 전체 네트워크의 성능을 최적화하여 혼잡 제어를 한다.

반면 UDP는 그딴거 없고 데이터를 데이터그램 단위로 전송하며, 각각의 데이터그램은 독립적으로 처리하며, 일단 데이터를 보내고 본다.

<br>

그래서 TCP는 대부분의 웹 브라우징이나 이메일 전송 등과 같이 신뢰성이 중요한 응용 프로그램에서 사용되며, UDP는 실시간 음성 및 영상 스트리밍, 온라인 게임 등과 같이 빠른 전송이 중요하며, 손실된 몇몇 패킷이 큰 문제가 되지 않는 경우에 사용된다. 근데, 사실 둘은 독립적으로 사용되는 것이 아니고, 상호 보완적으로 사용되며, 일단 TCP로 동작하다가 필요한 경우에 UDP로 동작하는 등의 방법으로 사용된다.

<br>

<img src="https://i.redd.it/duv11av99nm11.png" alt="TCP vs UDP">
