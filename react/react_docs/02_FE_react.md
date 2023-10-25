# 알게된 단편적 지식들 세번째

<br>

[리액트 공식문서](https://react.dev/)
[비공식 번역문서](https://react-ko.dev/)
[스터디 영상](https://youtube.com/playlist?list=PLjQV3hketAJkh6BEl0n4PDS_2fBd0cS9v&si=EqsHu_o7l1zGcDZd)

<br>

## [Responding to Events](https://react-ko.dev/learn/responding-to-events)

<br>

JSX에 이벤트 핸들러를 추가하여, click, hover, input의 focus 등과 같은 유저 액션에 반응할 수 있다.

<br>

### [Adding event handlers](https://react-ko.dev/learn/responding-to-events#adding-event-handlers)

<br>

이벤트 핸들러를 추가하려면 먼저 컴포넌트 안에서 적절한 함수를 정의한 다음 이를 적절한 JSX 태그에 prop으로 전달한다.

<br>

```jsx
export default function Button() {
  function handleClick() {
    alert("You clicked me!");
  }

  return <button onClick={handleClick}>Click me</button>;
}
```

<br>

### why not `addEventListener`?

[참조](https://medium.com/hcleedev/web-event%EC%99%80-addeventlistener-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-%EA%B0%9C%EB%85%90-react%EC%97%90%EC%84%9C-%EC%A3%BC%EC%9D%98%ED%95%A0-%EC%A0%90-a581adc49aa4)
[참조2](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Building_blocks/Events)

<br>

여기서 한가지 의문이 들었다. vanilla JavaScript에서는 이벤트 핸들러를 등록할 때, `addEventListener`를 사용하는 것을 더 권장받았는데, 리액트에서는 컴포넌트의 onClick prop에 함수를 직접 전달한다. 우선 vanilla JavaScript에서는 왜 `addEventListener`를 권장할까? 다음의 이유가 있다.

<br>

1. 다중 이벤트 핸들러 등록 가능: addEventListener를 사용하면 한 요소에 여러 개의 이벤트 핸들러를 등록할 수 있다. 반면에 onclick 같은 속성을 사용하면 한 요소에 하나의 이벤트 핸들러만 등록할 수 있어, 기존의 핸들러를 덮어쓰는 문제가 발생할 수 있다.
2. 이벤트 버블링 및 캡처 제어: addEventListener의 세 번째 매개변수를 통해 이벤트의 전파 방식을 제어할 수 있다. 이벤트 버블링 또는 캡처를 활용하면 이벤트의 흐름을 세밀하게 제어할 수 있다.
3. 이벤트 제거: removeEventListener를 통해 등록한 이벤트 핸들러를 제거할 수 있다. 이는 메모리 누수를 방지하고, 불필요한 이벤트 핸들러가 실행되는 것을 막는데 도움이 된다.
4. once 옵션 사용 가능: addEventListener의 옵션 객체에서 once: true를 설정하면, 이벤트를 한 번만 처리하고 이벤트 핸들러를 자동으로 제거할 수 있다.

<br>

이렇게 장점이 많은데 React에서는 왜 `addEventListener`를 사용하지 않을까? React에서는 합성 이벤트(Synthetic Event)이라고 부르는, 가상 DOM개념을 사용한, React 자체의 이벤트 시스템이 있기 때문이라고 한다. 이 합성 이벤트는 실제 DOM 이벤트보다 먼저 발생하며, 실제 이벤트가 발생하기 전에 React에서 정의한 이벤트 핸들러가 실행된다고 한다. 추가적으로 React에서도 이벤트 위임이나 버블링, 캡쳐링등의 개념이 모두 동일하게 적용된다.

<br>

다시 돌아와서, React에서 정의되는 이벤트 핸들러에 대해 더 알아보자. 이벤트 핸들러는 일반적으로 컴포넌트 안에 정의되며(prop이나 state에 직점 접근 할 수 있는 권한이 주어진다. 이벤트 핸들러는 순수 할 필요가 없다는 것을 기억하자.), handle로 시작하는 이름 뒤에 이벤트 이름이 오도록 한다. 예를 들어, onClick 이벤트 핸들러는 handleClick으로 정의한다. 다음으로 주의해야하는 점은, 이벤트 핸들러에 전달되는 함수는 **호출**하는 게 아니라 **전달**되어야 한다.

<br>

```jsx
// Correct
<button onClick={handleClick} />

// Wrong
<button onClick={handleClick()} />

// Correct
<button onClick={() => alert('...')} />

// Wrong
<button onClick={alert('...')} />
```

<br>

### [Naming event handler props](https://react-ko.dev/learn/responding-to-events#naming-event-handler-props)

<br>

`<button>` 및 `<div>`와 같은 기본 제공 컴포넌트는 onClick과 같은 브라우저 이벤트 이름만 지원한다. 하지만 커스텀 컴포넌트를 빌드할 때는 이벤트 핸들러 prop의 이름을 원하는 방식으로 지정할 수 있다. 특히 부모가 여러 자식 컴포넌트를 데리고 있고, 이들 각각에 이벤트를 달아야 할 때 유용하며, 관례상 이벤트 핸들러 props은 on으로 시작하고 그 뒤에 대문자가 와야 한다.

<br>

```jsx
function Button({ onSmash, children }) {
  return <button onClick={onSmash}>{children}</button>;
}

export default function App() {
  return (
    <div>
      <Button onSmash={() => alert("Playing!")}>Play Movie</Button>
      <Button onSmash={() => alert("Uploading!")}>Upload Image</Button>
    </div>
  );
}
```

<br>

### [Event propagation](https://react-ko.dev/learn/responding-to-events#event-propagation)

<br>

리액트에서도 역시 이벤트 버블링이 일어난다. 이벤트 핸들러는 컴포넌트의 모든 하위 컴포넌트의 이벤트도 포착한다. 첨부한 JSX 태그에서만 작동하는 onScroll을 제외한 모든 이벤트는 React에서 전파된다.

<br>

이벤트 핸들러는 이벤트 객체를 유일한 인수로 받고, 보통 “event”를 의미하는 `e`로 선언한다. 이 객체를 사용하여 이벤트에 대한 정보를 읽을 수 있다. (근데 이게 단순한 한글자 `e`라고 작은 객체는 아니다. 정말 많은 정보를 담고 있는 큰 객체라는 걸 생각하고 작업하자.)

<br>

이벤트 객체를 사용하면 전파를 중지할 수도 있다. 이벤트가 상위 컴포넌트에 도달하지 못하도록 `e.stopPropagation()`을 호출하면 된다. `e.preventDefault()`를 사용해서 브라우저의 기본 동작을 방지할 수도 있다.

<br>
<br>
<br>

## 감상

React고 나발이고 JavaScript와 브라우저부터 공부해라...고 했던 부스트캠프의 깊은 뜻을 다시 한번 느끼는 밤이다. 이벤트에 대해서 그리고 js의 함수에 대해서 공부를 해두길 잘한 것 같다.
