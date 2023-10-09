# props and state

리액트는 컴포넌트로 이루어져 있고, 컴포넌트들 간의 데이터를 전달하면서 UI를 그린다.

이때 사용되는 두가지 중요한 개념이 `props`와 `state`이다.

<br>
<br>

## Props

Props는 Properties의 줄임말로, 속성이라고 번역된다. 컴포넌트의 속성을 나타내는 객체이다.

```jsx
function example(props) {
  return <div>{props.name}</div>;
}
```

Props는 상속하는 부모 컴포넌트로부터 자녀 컴포넌트에 데이터등을 전달하는 방법이며, Props는 읽기 전용(immutable)으로 자녀 컴포넌트 입장에서는 변하지 않는다. (변하게 하고자 하면 부모 컴포넌트에서 state를 변경시켜줘야한다.)

```jsx
function Parent() {
  return <Child name="example" />;
}

function Child(props) {
  return <div>{props.name}</div>;
}
```

정리하면 다음과 같다.

<br>

### Props (Properties):

1. props는 컴포넌트 간 데이터를 전달하는 데 사용된다.
2. props는 부모 컴포넌트에서 자식 컴포넌트로 읽기 전용으로 전달된다.
3. props는 컴포넌트의 속성이나 매개변수와 유사하게 동작한다.
4. 주로 정적 데이터나 부모 컴포넌트로부터 동적으로 받은 데이터를 표시하는 데 사용된다.
5. props는 컴포넌트 내에서 변경할 수 없으며, 컴포넌트가 렌더링될 때 값이 설정되고 이후에 변경되지 않는다.

<br>
<br>

## State

State는 컴포넌트의 **렌더링 결과물에 영향을 주는 데이터를 갖고 있는 객체**이다. State가 변경되면 컴포넌트는 리랜더링(Re-rendering)되며, State는 컴포넌트 안에서 관리된다.

```jsx
function Example() {
  const [name, setName] = useState("example");
  return <div>{name}</div>;
}
```

정리하면 다음과 같다.

<br>

### State:

1. state는 컴포넌트 내에서 관리되는 데이터의 상태를 나타낸다.
2. state는 컴포넌트의 내부 데이터로 사용되며, 컴포넌트 내에서 변경 가능하다.
3. 주로 컴포넌트 내부의 동적 데이터나 사용자 상호작용으로 인해 변경되는 데이터를 관리하는 데 사용된다.
4. state를 변경하면 React는 자동으로 컴포넌트를 다시 렌더링하여 변경된 데이터를 화면에 반영한다.
5. state는 클래스 컴포넌트에서 사용되며, 함수 컴포넌트에서는 React Hooks를 통해 state를 사용할 수 있다.

```jsx
import React, { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  const increaseCount = () => {
    setCount(count + 1);
  };

  const decreaseCount = () => {
    setCount(count - 1);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increaseCount}>Increase Count</button>
      <button onClick={decreaseCount}>Decrease Count</button>
    </div>
  );
}

export default Counter;
```

<br>
<br>

## props vs state

1. props와 state는 모두 객체이다.
2. props와 state는 모두 컴포넌트의 렌더링 결과물에 영향을 준다.
3. 부모 컴포넌트에서 자식 컴포넌트로 데이터를 전달할 때는 props를 사용한다.
4. 컴포넌트 내부에서 데이터를 변경할 때는 state를 사용한다.
5. props는 읽기 전용이며, state는 변경 가능하다.
6. state가 변경되면 컴포넌트는 리랜더링된다.

<br>
<br>

더 많은 내용이 있을 수 있지만, 우선은 이정도만 비교 정리하고 넘어가자.
