# 알게된 단편적 지식들 네번째

<br>

[리액트 공식문서](https://react.dev/)
[비공식 번역문서](https://react-ko.dev/)
[스터디 영상](https://youtube.com/playlist?list=PLjQV3hketAJkh6BEl0n4PDS_2fBd0cS9v&si=EqsHu_o7l1zGcDZd)

<br>

## [State: A Component's Memory](https://react-ko.dev/learn/state-a-components-memory)

<br>

State: 컴포넌트의 메모리. 내가 지금까지 state는 상태라고만 배웠었는데, 역시 공식문서인가보다. "컴포넌트의 메모리"... 이해가 한층 깊어졌다.

<br>

React에서는 컴포넌트별 메모리를 state라고 부른다. (In React, this kind of component-specific memory is called state.) 컴포넌트를 변경하기 위해서 지역변수 만으로는 충분하지 않다. 두가지 문제점이 있는데, 지역 변수는 렌더링 간에 유지되지 않고, 지역 변수를 변경해도 렌더링을 발동시키지 않는다.

<br>

컴포넌트를 새 데이터로 업데이트하려면, 두 가지 작업이 필요하다.

1. 렌더링 사이에 데이터를 유지한다.
2. 새로운 데이터로 컴포넌트를 렌더링(리렌더링)하도록 React를 촉발한다.

<br>

리액트에서 가장 유명한 훅 `useState`가 제공하는 다음의 두 가지 기능을 사용하여 이 작업을 수행합니다.

1. 렌더링 사이에 데이터를 유지하기 위한 state 변수.
2. 변수를 업데이트하고 React가 컴포넌트를 다시 렌더링하도록 촉발하는 state 설정자 함수.

<br>

```jsx
const [index, setIndex] = useState(0);
```

<br>

### [Meet your first Hook](https://react-ko.dev/learn/state-a-components-memory#meet-your-first-hook)

<br>

React에서는 useState를 비롯해 ”use“로 시작하는 다른 함수를 훅(hook)이라고 부른다. hook이란, React가 **렌더링** 중일 때만 사용할 수 있는 특별한 함수인데, 이를 통해 다양한 React 기능을 “연결”할 수 있다. (공식문서의 설명이다. 음... 뭔말인지 대충 이해는 되는데, 완벽하진 않다. 아마 hook에 대한 이해가 깊어지면 이해가 될 것 같다.)

<br>

> **주의!!**: hook은 **컴포넌트의 최상위 레벨 또는 커스텀 훅에서만 호출할 수 있다**. 조건문, 반복문 또는 기타 중첩된 함수 내부에서는 훅을 호출할 수 없다.
>
> **컴포넌트의 최상위 레벨**: 함수의 가장 바깥 스코프. 그러니까 그냥 컴포넌트 함수의 첫 번째 줄 이라고 생각하자.

<br>

### [State is isolated and private](https://react-ko.dev/learn/state-a-components-memory#state-is-isolated-and-private)

<br>

state는 완전히 격리되어있고 프라이빗하다. 컴포넌트 인스턴스에서 지역적으로 존재하며, 같은 컴포넌트로 두번 렌더링 되었다 하더라도, 각 인스턴스들은 완전히 격리되어있어서 각각의 state를 가지며 서로 영향을 주지 않는다.

<br>

또한 state는 이를 선언하는 컴포넌트 외에는 완전히 비공개이며, 부모 컴포넌트는 이를 변경할 수 없다. 따라서 다른 컴포넌트에 영향을 주지 않으면서, state를 추가하거나 제거할 수 있다.

<br>

## [Render and Commit](https://react-ko.dev/learn/render-and-commit)

<br>

리액트가 컴포넌트를 화면에 표시하기까지 다음의 단계를 따른다.

> Trigger -> Render -> Commit

<br>

이 과정을 짧게 요약하자면 다음과 같다. (여기부분 정리하려고 많은 노력을 해봤는데, 그냥 공식문서를 옮겨놓는 수준에서 벗어나질 못해서... 공식문서를 보는 것을 추천한다.)

<br>

우선 컴포넌트의 첫번째 렌더(initial render)나 state의 변화(state has been updated)가 렌더링을 촉발한다. 일단, 렌더링이 촉발되면, React는 컴포넌트를 호출하여 화면에 표시할 내용을 파악한다. 첫 번째 렌더링의 경우 루트 컴포넌트를 호출하고, 그 이후 렌더링의 경우, state 업데이트에 의해 렌더링이 발동된 함수 컴포넌트를 호출한다. 마지막으로 React는 DOM을 수정하는데, 첫 번째 렌더링의 경우, React는 `appendChild()` DOM API를 사용하여 생성한 모든 DOM 노드를 화면에 표시한다. 그 이후 렌더링의 경우, 필요한 최소한의 작업(렌더링하는 동안 계산된 것!)을 적용하여 DOM이 최신 렌더링 출력과 일치하도록 한다. 이 과정을 **커밋**이라고 한다. 이때 React는 **렌더링 간에 차이가 있는 경우에만** DOM 노드를 변경한다. (더 자세한 내용은 공식문서를 참조!!)

<br>

## State as a Snapshot(https://react-ko.dev/learn/state-as-a-snapshot)

state 변수는 읽고 쓸 수 있는 일반 JavaScript 변수처럼 보일 수 있습니다. 하지만 state는 스냅샷처럼 동작합니다. state 변수를 설정해도 이미 가지고 있는 state 변수는 변경되지 않고, 대신 리렌더링이 실행됩니다.

```jsx
import { useState } from "react";

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button
        onClick={() => {
          setNumber(number + 1);
          setNumber(number + 1);
          setNumber(number + 1);
        }}
      >
        +3
      </button>
    </>
  );
}
```

왜 number는 3이 아니라 1이 될까? setNumber는 동기적으로 동작하지만, number는 스냅샷이기 때문에 number자체가 업데이트 되지 않고, 바뀐 값을 새로운 값으로 반환됩니다.
