# event delegation (이벤트 위임)

프로젝트를 진행하다가 이벤트 위임에 관한 지적을 몇번 받은 적이 있다. 그래서 공부하기 위해 구글링을 해보니, 버블링과 캡쳐링이라는 키워드를 알아야 이해할 수 있었다. 그래서 이번에는 이벤트 위임과 버블링, 캡쳐링에 대해서 공부해보자.

<br>

[출처\_1](https://ko.javascript.info/bubbling-and-capturing)
[출처\_2](https://www.youtube.com/watch?v=0jtalJxrxhs)

<br>

## 버블링과 캡쳐링

<br>

### 버블링

버블링은, 이벤트가 발생한 요소에서 시작하여 상위 요소로 이벤트가 전파되는 방식을 말한다. 예를 들어, `<div>` 요소 안에 `<button>` 요소가 있는 경우 `<button>`에서 클릭 이벤트가 발생하면 이 이벤트는 `<button>` 요소부터 시작해서 상위 요소인 `<div>`까지 순차적으로 전파된다. 이때 div에 클릭고 관련한 이벤트가 걸려있다면, 이 이벤트도 같이 발생한다.

<br>

javascript.info의 설명은 다음과 같다.

> 한 요소에 이벤트가 발생하면, 이 요소에 할당된 핸들러가 동작하고, 이어서 부모 요소의 핸들러가 동작합니다. 가장 최상단의 조상 요소를 만날 때까지 이 과정이 반복되면서 요소 각각에 할당된 핸들러가 동작합니다.

<br>

우리가 자주 사용하는 addEventListener는 기본적으로 버블링으로 동작한다. (말이 정확한지는 모르겠지만, 아무튼 상위요소로 이벤트를 전파시킨다.) 예를들어 다음과 같은 html/js가 있다고 하자.

<br>

```html
<body>
  <div>
    <p>
      <span> test </span>
    </p>
  </div>
</body>
```

```js
const $body = document.querySelector("body");
const $div = document.querySelector("div");
const $p = document.querySelector("p");
const $span = document.querySelector("span");
```

<br>

이렇게 선언되었을 때, 각각의 요소에 다음과 같이 이벤트를 걸어보자.

<br>

```js
$body.addEventListener("click", () => {
  console.log("body clicked by bubbling");
});

$div.addEventListener("click", () => {
  console.log("div clicked by bubbling");
});

$p.addEventListener("click", () => {
  console.log("p clicked by bubbling");
});

$span.addEventListener("click", () => {
  console.log("span clicked by bubbling");
});
```

<br>

모든 요소에 클릭 이벤트에 해당하는 핸들러를 달아주었다. 이때 span을 클릭하면, `span clicked by bubbling`만 출력할 것 같지만 실제로는 다음과 같이 span -> p -> div -> body 순으로 이벤트가 발생하고 출력된다.

<br>

```bash
>>> span clicked by bubbling
>>> p clicked by bubbling
>>> div clicked by bubbling
>>> body clicked by bubbling
```

<br>
<br>

### 캡쳐링

버블링과는 다르게 캡쳐링은 자주 쓰이지는 않지만, 그래도 알아는 두자.

캡처링은 버블링과는 반대로 이벤트가 가장 상위 요소(일반적으로 문서의 루트 요소인 `<html>` 또는 `<body>`)에서 시작하여 하위 요소로 전파되는 방식을 말한다. 캡처링 단계에서는 이벤트가 가장 상위 요소에서부터 시작해서 하위 요소로 이동한다.

캡처링 단계에서 이벤트를 잡을 수 있는 이점 중 하나는 이벤트가 실제로 발생하기 전에 상위 요소에서 이벤트를 중단시킬 수 있어서, 특정 이벤트를 가로채고 처리하는 데 사용할 수 있다.

<br>

addEventListener의 세번째 파라미터에 true를 입력하면, 버블링이 아니라 캡쳐링으로 이벤트가 발생한다.

<br>

```js
$body.addEventListener(
  "click",
  () => {
    console.log("body clicked by capturing");
  },
  true
);

$div.addEventListener(
  "click",
  () => {
    console.log("div clicked by capturing");
  },
  true
);

$p.addEventListener(
  "click",
  () => {
    console.log("p clicked by capturing");
  },
  true
);

$span.addEventListener(
  "click",
  () => {
    console.log("span clicked by capturing");
  },
  true
);
```

<br>

이렇게 하면, span을 클릭했을 때, body -> div -> p -> span 순으로 이벤트가 발생한다.

<br>

```bash
>>> body clicked by capturing
>>> div clicked by capturing
>>> p clicked by capturing
>>> span clicked by capturing
```

<br>

그럼 이런 이벤트의 전파를 막고싶으면 어떠한 방법이 있을까?

<br>

```js
$body.addEventListener("click", () => {
  console.log("body clicked by bubbling");
});

$div.addEventListener("click", () => {
  console.log("div clicked by bubbling");
});

$p.addEventListener("click", () => {
  console.log("p clicked by bubbling");
});

$span.addEventListener("click", () => {
  console.log("span clicked by bubbling");
  event.stopPropagation();
});
```

<br>

이렇게 하면, span을 클릭했을 때, span tag에서 이벤트 전파를 막았기 때문에 span만 출력된다.
(다만, 이 방법은 추천하지 않는다. 지금은 상위 요소에서 이벤트가 어떻게 쓰일지 확실치 않더라도, 추후에 버블링이 필요한 경우가 생기기 때문)

<br>

```bash
>>> span clicked by capturing
```

### 정리

1. 이벤트가 발생하면 이벤트가 발생한 가장 안쪽 요소가 '타깃 요소(event.target)'가 된다. 위의 예시에서 span을 클릭하면 span이 타깃 요소가 된다.

2. 이벤트는 document에서 시작해 DOM 트리를 따라 event.target까지 내려간다. 이벤트는 트리를 따라 내려가면서 addEventListener(..., true)로 할당한 핸들러를 동작시킨다.

3. 마지막에 타깃 요소에 설정된 핸들러가 호출된다.

4. 이후엔 이벤트가 event.target부터 시작해서 다시 최상위 노드까지 전달되면서 각 요소에 on`<event>`로 할당한 핸들러와 addEventListener로 할당한 핸들러를 동작시킨다. addEventListener로 할당한 핸들러 중, 세 번째 인수가 없거나 false, {capture: false}인 핸들러만 호출된다.

<br>

각 핸들러는 아래와 같은 event 객체의 프로퍼티에 접근할 수 있습다.

event.target – 이벤트가 발생한 가장 안쪽의 요소
event.currentTarget (=this) – 이벤트를 핸들링 하는 현재 요소 (핸들러가 실제 할당된 요소)
event.eventPhase – 현재 이벤트 흐름 단계(캡처링=1, 타깃=2, 버블링=3)

<br>

js의 이벤트가 상위와 하위 요소들로 전파가 된다는건 오늘 처음 알았다... 앞으로 핸들러를 달아서 이벤트를 관리할 때는 조심해야겠다..

<br>
<br>

### 이벤트 위임

버블링을 이용한 이번트 위임에 대해서도 공부해보자. 이전에 이벤트위임을 사용하라는 리뷰를 받았을 때 이해가 안되었는데, 버블링을 알게 되니 이해가 되는 것 같다.

이벤트 위임이란, 다수의 하위 요소에 이벤트 리스너를 개별적으로 추가하는 대신 상위 요소에 하나의 이벤트 리스너를 추가하여 하위 요소들에서 발생한 이벤트를 상위 요소에서 처리하는 방식을 말한다. 이렇게 해서 얻을 수 있는 장점은 다음과 같다.

<br>

1. **성능 개선**: 하위 요소의 수가 많을 때, 각 요소에 개별적인 이벤트 리스너를 추가하는 것은 성능 문제를 일으킬 수 있다고 한다. 상위 요소에 이벤트 리스너를 하나만 추가하면 이벤트 핸들링의 효율성을 높일 수 있다.

2. **동적 요소 처리**: 새로운 하위 요소가 동적으로 추가될 경우에도 이벤트를 처리할 수 있다. 즉, 페이지가 처음 로드될 때 없던 요소가 추가되어도 작동하게 된다. 하위에 새로운 요소가 추가될 때 마다 이벤트를 달아주려면 매번 이벤트를 추가해줘야 하지만, 상위 요소에 이벤트를 달아주면 상위 요소에만 이벤트를 달아주면 되기 때문에 편리하다.

3. **메모리 관리**: 하위 요소에 직접 이벤트 리스너를 추가할 경우, 해당 요소가 삭제되더라도 메모리에서 해제되지 않을 수 있다고 한다. 이벤트 위임을 사용하면 애초에 부모요소에 핸들러가 하나 존재하는 것이기 때문에 메모리를 낭비할 일이 없다.

<br>

내가 리뷰에서 지적받은 부분은 2번이었다. 학습을 마친 뒤 리팩터링하러 가야지... 아무튼 다음의 예시를 보면서 이해도를 높여보자!

이벤트 위임을 구현하려면, 우선 적절한 상위 요소를 선택해야하고, 선택한 상위 요소에 원하는 이벤트에 대한 핸들러를 추가해주면 된다.

<br>
<br>

```html
<ul id="myList">
  <li>아이템 1</li>
  <li>아이템 2</li>
  <li>아이템 3</li>
</ul>

<script>
  // 상위 요소인 ul에 클릭 이벤트 리스너를 추가
  document.getElementById("myList").addEventListener("click", function (event) {
    // 클릭 이벤트가 발생한 요소 확인
    if (event.target.tagName === "li") {
      // 클릭된 li 요소에 대한 동작 수행
      console.log("클릭된 아이템:", event.target.textContent);
    }
  });
</script>
```

<br>

이렇게 하면 각각의 리스트 아이템에 개별적인 이벤트 리스너를 추가하는 대신, 상위 ul 요소에서 클릭 이벤트를 처리하여 어떤 아이템이 클릭되었는지 확인할 수 있다. 이제 리팩토링 하러 가야지~~~
