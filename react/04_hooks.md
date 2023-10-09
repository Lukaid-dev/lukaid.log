# Hooks

react를 비롯해서 javascript의 기술을 공부하면서 놀라웠던게, 기술이 참빠르게 발전한다고 느낌과 동시에 이 기능이 이떄는 없었다고? 하는 순간들도 있었다. 예를들어 js에서 class는 ES6에, 그러니까 2015년에 도입되었다고 한다. 그 전에는 그러니까 2015년까지는 프로토타입 기반의 상속을 사용하여 클래스를 사용했다고 한다.

<br>

이번에 알아본 hooks도 마찬가지인데, 나는 react입문 자체를 함수형으로 하였으니 그 전에 어떻게 활용되었는지 이번에 공부를 하면서 알게 되었다. 기술은 항상 불편함에서 발전을 하나보다. 원래 react는 class없이는 state를 사용할 수 없었다고 한다. 이것에 불편함을 느낀 개발자들이 ReactConf2018에 React v16.8부터 React Hooks를 도입하면서, 함수형 컴포넌트에서도 상태(state)와 생명주기(lifecycle) 기능을 사용할 수 있게 해주었다.

<br>

이전에 클래스형 컴포넌트를 사용할 때, componentDidMount, componentDidUpdate, componentWillUnmount를 통해 생명주기를 관리하던 것을, 함수형 컴포넌트에서는 useEffect를 통해 관리할 수 있게 되었다. 또한 useState를 통해 state를 사용할 수 있게 되었다. 클래스형 컴포넌트에 비해 `1. 더 짧고 간결한 코드` 를 작성 할 수 있고, `2. 더 빠른 성능` 을 기대할 수 있으며, `3. 더 나은 가독성` 을 갖출 수 있게 되었다. 이제 자주 사용되는 hooks들에 대해 간단하게 알아보자.

<br>
<br>

## 1. useState : 상태(State) 관리

가장 유명한 react의 hook일 것이다. useState Hook은 함수형 컴포넌트에서 상태를 관리하기 위해 사용되며, 현재 상태 값과 이 값을 업데이트하는 함수를 반환한다.

```jsx
import React, { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increase Count</button>
    </div>
  );
}
```

<br>

### 리액트에서의 불변성

리액트에서는 state가 변경되면 UI가 재 렌더링 된다. 이때, state가 변경되는 것에 집중해보자. 다음과 같이 작성된 react코드가 있다고 하자.

```jsx
const [list, setList] = useState([]);

const addList = (item) => {
  list.push(item);
  setList(list);
};
```

이럴경우 UI는 업데이트가 될까? 정답은... _안된다!_ 왜냐하면 list의 원소들은 변경되었을 지언정 list의 메모리 값은 그대로이기 때문에 리액트는 list가 변경되었는지 어쩐지 전혀 알 수 없다. 그러니까 heap memory의 값은 변경되었지만, call stack의 주소값은 그대로이다. 따라서 다음과 같이 작성하여야 한다.

```jsx
const [list, setList] = useState([]);

const addList = (item) => {
  setList([...list, item]);
};
```

이렇게 작성하면 UI가 업데이트 된다. 새로운 리스트가 반환되어서 완전히 새로운 주소에 저장되기 때문이다. 리액트는 상태의 변경을 감지할 때, 상태의 메모리 주소를 비교한다. 따라서 상태의 메모리 주소가 변경되지 않으면 리액트는 상태가 변경되지 않은 것으로 간주한다. 이때, 상태의 메모리 주소가 변경되었는지를 판단하는 것은 `===` 연산자를 통해 판단한다. 따라서 다음과 같이 작성하면 UI가 업데이트 되지 않는다. 리액트에서는 불변성을 지켜야만 컴포넌트의 성능을 최적화 할 수 있다고 한다.

<br>
<br>

## 2. useEffect : 부수 효과 관리

useEffect Hook은 함수형 컴포넌트에서 부수 효과(Managing Side Effects, 예: 데이터 가져오기, 구독 설정, DOM 조작 등)를 처리하기 위해 사용된다. 이러한 부수 효과는 컴포넌트의 렌더링 이후에 실행되며, 컴포넌트의 상태나 프롭스에 의존적인 작업을 처리하는 데 사용됩니다. useEffect 함수 내부에서 부수 효과를 정의하며, 컴포넌트의 렌더링 후에 실행된다.

```jsx
import React, { useState, useEffect } from "react";

function DataFetching() {
  const [data, setData] = useState([]);

  useEffect(() => {
    // 데이터 가져오는 API 호출 등 부수 효과 처리
    fetch("https://api.example.com/data")
      .then((response) => response.json())
      .then((result) => setData(result));
  }, []); // 빈 배열을 전달하면 컴포넌트가 마운트될 때만 실행

  return (
    <div>
      {data.map((item) => (
        <p key={item.id}>{item.name}</p>
      ))}
    </div>
  );
}
```

추가적으로 useEffect Hook은 함수형 컴포넌트에서 컴포넌트의 생명주기와 관련된 동작을 수행하는 데 사용된다. 클래스 컴포넌트에서의 componentDidMount, componentDidUpdate, componentWillUnmount 등의 생명주기 메서드와 유사한 역할을 한다.

<br>

### 1. componentDidMount

- componentDidMount는 클래스 컴포넌트에서 컴포넌트가 처음으로 렌더링될 때 호출되는 생명주기 메서드이다.
- useEffect의 빈 배열([])을 의존성 배열로 전달하여 컴포넌트가 처음 렌더링될 때만 실행되도록 설정할 수 있다. 이 경우, useEffect의 콜백 함수는 componentDidMount와 유사한 역할을 한다.

```jsx
useEffect(() => {
  // 컴포넌트가 마운트된 후 실행될 코드
}, []); // 빈 배열: 처음 렌더링 시에만 실행
```

<br>

### 2. componentDidUpdate

- componentDidUpdate는 클래스 컴포넌트에서 state나 props의 변경으로 인해 컴포넌트가 다시 렌더링될 때 호출되는 생명주기 메서드이다.
- useEffect의 의존성 배열에 특정 state나 props를 포함하여 전달하면 해당 값이 변경될 때마다 useEffect의 콜백 함수가 실행된다.

```jsx
const [count, setCount] = useState(0);

useEffect(() => {
  // count가 변경될 때마다 실행될 코드
}, [count]); // count를 의존성 배열에 추가
```

<br>

### 3. componentWillUnmount

- componentWillUnmount는 클래스 컴포넌트에서 컴포넌트가 언마운트(제거)될 때 호출되는 생명주기 메서드이다.
- useEffect를 사용할 때, 해당 효과 함수에서 반환하는 함수를 통해 정리(clean-up) 작업을 수행할 수 있습니다. 이는 컴포넌트가 언마운트될 때 실행됩니다.

```jsx
useEffect(() => {
  // 컴포넌트가 마운트될 때 실행될 코드

  return () => {
    // 컴포넌트가 언마운트될 때 실행될 정리 작업
  };
}, []);
```

useEffect를 통해 생명주기와 관련된 동작을 관리하면 함수형 컴포넌트에서도 컴포넌트의 렌더링과 관련된 여러 상황에서 작업을 수행할 수 있으며, 또한 정리 작업을 통해 메모리 누수를 방지할 수 있다. 이러한 점에서 useEffect는 React 함수형 컴포넌트에서 생명주기 관리에 유용한 도구이다.

<br>
<br>

## 3. useContext: 컨텍스트 관리

- useContext Hook은 React 컨텍스트(Context)를 활용하여 컴포넌트 간에 데이터를 공유할 때 사용된다.
- 컨텍스트로부터 데이터를 읽고 업데이트하는 데 사용된다.

```jsx
import React, { useContext } from "react";

// 컨텍스트 생성
const MyContext = React.createContext();

function ParentComponent() {
  const value = "Hello from Context";

  return (
    <MyContext.Provider value={value}>
      <ChildComponent />
    </MyContext.Provider>
  );
}

function ChildComponent() {
  const contextValue = useContext(MyContext);

  return <p>{contextValue}</p>;
}
```

state와 context는 둘 다 컴포넌트 간에 데이터를 공유하는 데 사용되는데, 어떤 차이가 있을까?

주요 차이점:

1. state는 컴포넌트의 로컬 상태를 관리하고 해당 컴포넌트 내에서만 사용된다. 반면에 context는 여러 컴포넌트 간에 데이터를 공유하기 위한 전역 메커니즘이다.
2. state는 주로 컴포넌트의 상태를 나타내고 업데이트하는 데 사용되며 컴포넌트의 렌더링과 관련이 있다. 반면에 context는 주로 데이터 공유와 관련이 있으며 컴포넌트의 렌더링보다는 데이터의 전파와 공유에 중점을 둔다.
3. state는 useState Hook을 사용하여 함수형 컴포넌트 내에서 관리되며, 각 컴포넌트마다 독립적인 상태를 가질 수 있다. 반면에 context는 컨텍스트 객체를 생성하고 여러 컴포넌트에서 공유하기 위해 createContext 함수와 useContext Hook을 사용한다.

두 개념은 각자의 사용 사례와 목적에 따라 선택되며, state는 컴포넌트 내부의 로컬 상태를 다루는 데 주로 사용되고, context는 컴포넌트 간 데이터 공유 및 전파에 사용된다.

<br>
<br>

## 4. useReducer: 리듀서를 사용한 상태 관리

- useReducer Hook은 상태 관리에 리듀서(reducer)를 사용하는 방법을 제공한다.
- useReducer 함수는 현재 상태와 액션(action)을 처리하는 리듀서 함수를 받는다.
- useReducer를 사용하면 컴포넌트의 상태를 더 복잡한 방식으로 관리할 수 있으며, 주로 컴포넌트의 상태를 업데이트하는 로직을 분리하고 재사용 가능한 상태 관리 코드를 작성할 때 사용된다.

<br>

### 1. Reducer 함수:

Reducer 함수는 상태 업데이트 로직을 정의하는 함수이다. 이 함수는 현재 상태와 액션 객체를 받아서 새로운 상태를 반환한다. Reducer 함수는 순수 함수로 작성되며, 이전 상태와 액션 객체를 바탕으로 새로운 상태를 계산한다.

```jsx
const reducer = (state, action) => {
  switch (action.type) {
    case "INCREMENT":
      return { count: state.count + 1 };
    case "DECREMENT":
      return { count: state.count - 1 };
    default:
      return state;
  }
};
```

<br>

### 2. dispatch 함수:

useReducer를 통해 생성된 상태와 함께 반환되는 dispatch 함수를 사용하여 상태를 업데이트할 수 있다. dispatch 함수는 액션 객체를 받아서 Reducer 함수를 호출하고 상태를 업데이트한다.

```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```

<br>

### 3. 초기 상태:

useReducer를 호출할 때 두 번째 매개변수로 초기 상태를 전달한다.

```jsx
const initialState = { count: 0 };
const [state, dispatch] = useReducer(reducer, initialState);
```

<br>

### 4. 액션 객체:

액션 객체는 상태를 업데이트할 때 어떤 종류의 작업을 수행할 지를 나타내는 객체다. 일반적으로 type 프로퍼티를 가지고 있으며, 이를 기반으로 Reducer 함수에서 어떤 동작을 수행할 지를 결정한다. 추가적인 데이터를 담을 수도 있다.

```jsx
const incrementAction = { type: "INCREMENT" };
const decrementAction = { type: "DECREMENT" };
```

<br>

useReducer를 사용하면 상태 관리 로직을 더 구조적으로 관리할 수 있으며, 복잡한 컴포넌트에서 상태 변경을 추적하기 쉽게 만든다. 예를 들어, useState를 사용하여 상태를 직접 업데이트하는 대신 useReducer를 사용하여 관련된 상태 업데이트 로직을 Reducer 함수로 분리할 수 있다.

<br>

전체코드

<br>

```jsx
const initialState = { count: 0 };

const reducer = (state, action) => {
  switch (action.type) {
    case "INCREMENT":
      return { count: state.count + 1 };
    case "DECREMENT":
      return { count: state.count - 1 };
    default:
      return state;
  }
};

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: "INCREMENT" })}>Increment</button>
      <button onClick={() => dispatch({ type: "DECREMENT" })}>Decrement</button>
    </div>
  );
}
```

<br>
<br>

## 5. useCallback: 함수를 기억하여 성능 최적화

- useCallback Hook은 함수를 메모이제이션(기억)하여 불필요한 함수 생성을 방지하는 데 사용된다.
- 일반적으로 React 컴포넌트 내에서 함수를 정의하면 렌더링 시마다 새로운 함수가 생성되는데, 이러한 함수 생성은 성능에 부담을 줄 수 있다.
- useCallback을 사용하면 특정 함수가 변경되지 않는 한 해당 함수를 재사용할 수 있다. (함수의 참조가 변경되지 않는 한)

```jsx
const memoizedCallback = useCallback(() => {
  // 실행할 함수 내용
}, [dependencies]);
```

<br>

- 첫 번째 매개변수에는 메모이제이션할 함수를 전달한다.
- 두 번째 매개변수에는 의존성 배열(dependencies)을 전달한다. 이 배열에 포함된 값들이 변경될 때만 함수가 재생된다. 빈 배열([])을 전달하면 함수는 컴포넌트의 렌더링 사이에 변경되지 않는다.

<br>

### useCallback을 사용하기 좋은 상황

useCallback은 모든 함수에 다 사용하기보다는 사용하는 자체로서 비용이 들기 때문에 정말 필요한 때에 사용하는 게 좋다.

1. 자식 컴포넌트가 React.memo()로 최적화되어 있고
2. 그 자식 컴포넌트에게 해당 함수를 props로 넘겨줄 때 useCallback을 사용하는 것이 유용함

<br>
<br>

## 6. useMemo: 연산을 기억하여 성능 최적화

- useMemo Hook은 계산 비용이 높은 연산의 결과를 메모이제이션하여 불필요한 재계산을 방지하는 데 사용된다.
- 특히 계산량이 많거나 렌더링 과정에서 자주 변경되지 않는 값을 계산할 때 유용하다.

```jsx
const memoizedValue = useMemo(() => {
  // 계산 비용이 높은 연산을 수행하고 결과를 반환
  return computedValue;
}, [dependencies]);
```

<br>

- 첫 번째 매개변수에는 메모이제이션할 값을 계산하는 함수를 전달한다.
- 두 번째 매개변수에는 의존성 배열(dependencies)을 전달한다. 이 배열에 포함된 값들이 변경될 때만 값을 다시 계산한다. 빈 배열([])을 전달하면 함수는 컴포넌트의 렌더링 사이에 변경되지 않는다.

<br>

useCallback과 useMemo는 거의 비슷하지만, useCallback 은 메모이제이션된 함수를 반환하며, useMemo 은 메모이제이션된 값를 반환한다.

## 마무리

리액트에서 자주 사용되지만 헷갈리는 hooks를 모아서 정리해 보았다. 정리하는데 생각보다 시간이 많이 들었지만, 한번 이렇게 쭉 정리를 하고 나니 조금 더 명확해지는 느낌이다. 물론 이 외에도 중요한 hook들은 많이 있겠지만, 이를 바탕으로 하나씩 차근차근 공부해보자!
