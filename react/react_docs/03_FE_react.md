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

리액트에서 가장 유명한 훅 `useState`가 제공하는 다음의 두 가지 기능을 사용하여 이 작업을 수행한다.

1. 렌더링 사이에 데이터를 유지하기 위한 state 변수.
2. 변수를 업데이트하고 React가 컴포넌트를 다시 렌더링하도록 촉발하는 state 설정자 함수.

<br>

```jsx
const [index, setIndex] = useState(0);
```

<br>

### [Meet your first Hook - useState](https://react-ko.dev/learn/state-a-components-memory#meet-your-first-hook)

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

## [State as a Snapshot](https://react-ko.dev/learn/state-as-a-snapshot)

<br>

state는 **스냅샷**처럼 동작한다. 스냅샷은, **어떤 순간의 변경되지 않는 정보**를 의미한다. state 변수를 다시 설정해도 이미 가지고 있는 state 변수는 변경되지 않고, 대신 **리렌더링**이 실행된다. state가 변하면 들고있는 스냅샷을 버리고, 새로운 스냅샷을 만든다.

<br>

우리가 어떤 상황을 사진으로 찍고(스냅샷), 그 상황이 변한다 해서 사진 속의 장면이 변하지는 않는다. 사진 속의 장면에 변화를 주는 유일한 방법은, 상황이 변화했을 때 사진을 다시 찍는 것이다. 이것이 바로 리액트의 state가 동작하는 방식이다.

<br>

### [Setting state triggers renders and Rendering takes a snapshot in time](https://react-ko.dev/learn/state-as-a-snapshot#setting-state-triggers-renders)

<br>

state를 설정하면 (set function을 호출하면) 렌더링이 촉발되고, 렌더링은 그 시점의 스냅샷을 기록한다. **렌더링**이란, React가 컴포넌트, 즉,함수를 호출한다는 뜻이다. 반환되는 JSX는 스냅샷을 참고한다. prop, 이벤트 핸들러, 로컬 변수는 모두 렌더링 당시의 state를 사용해 계산된다.

<br>

React가 컴포넌트를 리렌더링할 때의 과정은 다음과 같다.

1. React가 함수를 다시 호출한다.
2. 함수가 새로운 JSX 스냅샷을 반환한다.
3. React가 반환한 스냅샷과 일치하도록 화면을 업데이트한다.

<br>

우리가 일반적인 함수를 호출하고 함수가 값을 반환하면, 함수 내부에서 사용되었던 지역변수는 사라진다. 하지만 state는 조금 다르게 동작한다. state는 실제로 함수 외부의 메모리에 존재하며, React 자체에 “존재”한다. 하지만 마치 그 컴포넌트에 국한된 지역변수 처럼 보인다. React가 컴포넌트를 호출하면 특정 렌더링에 대한 state의 스냅샷을 제공하고, 컴포넌트는 해당 렌더링의 state 값을 사용해 계산된 새로운 props 세트와 이벤트 핸들러가 포함된 UI의 스냅샷을 JSX에 반환한다!

<br>

state는 함수(컴포넌트)밖의 메모리에 존재하지만, 마치 함수 안의 지역변수 처럼 보이기도 하며, 실제로 해당 함수에만 국한되어서 사용된다!

<br>

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

<br>

위의 컴포넌트는 이벤트 핸들러를 가지고 있는 버튼을 반환하고 버튼을 눌렀을 때 마치 number가 3이 될 것 같지만 실제로는 1이 된다. state는 **스냅샷**처럼 동작한다!! 그러니까 setNumber함수를 불러서 number를 1씩 증가시키는 것 처럼 보여도, 실제로 number의 값은 절대 변하지 않는다. 각 setNumber 함수마다 setNumber에 1이 더해진 새로운 스냅샷(state)를 반환 할 뿐이다. 그래서 number는 1이 된다.

<br>

위의 코드를 알기쉽게 바꿔보면 마치 아래와 같이 동작한다.

<br>

```jsx
<button
  onClick={() => {
    setNumber(0 + 1);
    setNumber(0 + 1);
    setNumber(0 + 1);
  }}
>
  +3
</button>
```

<br>

그럼 아래의 코드는 어떤 alert를 띄울까?

<br>

```jsx
<button
  onClick={() => {
    setNumber(number + 1);
    setTimeout(() => {
      alert(number);
    }, 3000);
  }}
>
  +3
</button>
```

<br>

당연히 0이 뜬다! 동기, 비동기의 문제가 아니라, number는 절대 변하지 않는다! (리렌더가 되지 않는 이상 ㅎㅎ, 하지만 리렌더가 완료되면 number의 값은 바뀌겠지!)

<br>

## [Queueing a Series of State Updates](https://react-ko.dev/learn/queueing-a-series-of-state-updates)

<br>

React에는 rendering queue가 있다고 함... state를 변경하면 다음 렌더링이 이 렌더링 queue에 들어간다고 한다.

<br>

### [React batches state updates | state 업데이트 일괄처리](https://react-ko.dev/learn/queueing-a-series-of-state-updates)

<br>

위에서 본 예제를 다시 보고 이해해보자. React는 state 업데이트를 하기 전에 이벤트 핸들러의 모든 코드가 실행될 때까지 기다린다. 따라서, 리렌더링은 모든 setNumber() 호출이 완료된 이후에만 일어난다. 이렇게 하면 너무 많은 리렌더링을 촉발하지 않고도 여러 컴포넌트에서 나온 다수의 state 변수를 업데이트할 수 있다.

<br>

### [Updating the same state multiple times before the next render 다음 렌더링 전에 동일한 state 변수를 여러 번 업데이트하기](https://react-ko.dev/learn/queueing-a-series-of-state-updates#updating-the-same-state-multiple-times-before-the-next-render)

<br>

잘 쓰이지는 않지만, 한번의 렌더링에서 state를 여러번 업데이트하고 싶다면, 함수를 인자로 받는 setNumber를 사용하면 된다. `setNumber(number + 1)` 대신에, `setNumber((number) => number + 1)`를 사용해보자. set function에서 인자에 함수를 전달한다면, 그 함수의 인자는 prevState가 들어오고 반환은 새로운 state를 반환한다. `setFunction(prevState => newState)` 이렇게 사용하면 된다. setFunction 안의 함수를 업데이터 함수(updater function)라고 부른다. 업데이터 함수는 렌더링 중에 실행되므로, 업데이터 함수는 순수해야 하며 결과만 반환해야한다.

<br>

### [Naming conventions](https://react-ko.dev/learn/queueing-a-series-of-state-updates#naming-conventions)

<br>

업데이터 함수 인수의 이름은 해당 state 변수의 첫 글자로 지정하는 것이 일반적이다.

<br>

```jsx
setEnabled((e) => !e);
setLastName((ln) => ln.reverse());
setFriendCount((fc) => fc * 2);
```

<br>

## [Updating Objects in State](https://react-ko.dev/learn/updating-objects-in-state)

<br>

state에는 객체를 포함해서 어쩐 종류의 JavaScript 자료형이든 저장할 수 있다. 하지만 state의 객체를 직접 변경해서는 안된다. 대신, 객체의 복사본을 만들고, 그 복사본을 업데이트한 다음, 그 복사본을 state에 설정해야한다. 슬슬 감이 올텐데 **불변성**에 관한 이야기다.

<br>

### [What’s a mutation?](https://react-ko.dev/learn/updating-objects-in-state#whats-a-mutation)

<br>

mutation이라는 무서운 단어를 쓰면서 강조하고 싶은 내용이 있나보다. mutation은 객체의 속성을 변경하는 것을 의미한다. 원시 타입의 값을 바꾸는 것은 재할당이라고 부른다. React state의 객체는 기술적으로는 변이할 수 있지만, 숫자, 불리언(boolean), 문자열과 같이 불변하는 것처럼 취급해야 한다. **객체를 직접 변이하는 대신, 항상 교체해야 한다**. 간단히 정리하자면 다음과 같다.

<br>

> 원시형(string, number, boolean, null, undefined, Symbol) -> immutable -> mutation 불가능
> 객체형(object, array, function) -> mutable -> mutation 가능

<br>

다시 말해 state에 넣는 모든 JavaScript 객체를 읽기 전용으로 취급해야한다. 뭔가 바꾸고 싶다면, 새로 만들어라! 그래야 React가 렌더링을 촉발할 수 있다.

<br>

### [Copying objects with the spread syntax](https://react-ko.dev/learn/updating-objects-in-state#copying-objects-with-the-spread-syntax)

<br>

객체의 특정 값만 바꾸고 싶을 때 꿀팁 -> spread 연산자

<br>

```jsx
const [user, setUser] = useState({
  name: "Mark",
  age: 33,
  email: ",
});

setUser({ ...user, age: 34 });
```

<br>

<details>
<summary>극한으로 깍기</summary>
<div>

```jsx
export default function Form() {
  const [person, setPerson] = useState({
    firstName: "lukaid",
    lastName: "lee",
    email: "crescent3859@gmail.com",
  });

  // closure
  const handleChange = (target) => (e) =>
    setPerson({ ...person, [target]: e.target.value });

  return (
    <>
      <input
        type="text"
        value={person.firstName}
        onChange={handleChange("firstName")}
      />
      <input
        type="text"
        value={person.lastName}
        onChange={handleChange("lastName")}
      />
      <input
        type="text"
        value={person.email}
        onChange={handleChange("email")}
      />
    </>
  );
}
```

<br>

```jsx
const handleChange = (target) => (e) =>
  setPerson({ ...person, [target]: e.target.value });
```

위의 함수는 아래와 같다.

```jsx
function handleChange(target) {
  return function (e) {
    return setPerson({ ...person, [target]: e.target.value });
  };
}
```

</div>
</details>

<br>

### [Updating a nested object | 중첩된 객체 업데이트하기](https://react-ko.dev/learn/updating-objects-in-state#updating-a-nested-object)

<br>

냅다 펼치기!! (이게 불편하면 [Immer.js](https://contents.kyobobook.co.kr/sih/fit-in/280x0/pdt/9791158394646.jpg)를 사용하자)

<br>

```jsx
const [user, setUser] = useState({
  name: "Mark",
  age: 33,
  email: "test@test.com",
  address: {
    city: "Seoul",
    country: "Korea",
  },
});

setUser({
  ...user,
  address: {
    ...user.address,
    city: "Busan",
  },
});
```

<br>

## [Updating Arrays in State](https://react-ko.dev/learn/updating-arrays-in-state)

<br>

배열도 마찬가지다! state에 저장된 배열을 업데이트하려면, 새로운 배열을 만들고(또는 기존 배열을 복사본을 만듦) 새 배열을 사용하도록 state를 설정해야한다.

<br>

<table><thead><tr><th align="left"></th><th align="left">avoid (mutates the array)<br><span class="translate">비추천 (배열 직접 변이)</span></th><th align="left">prefer (returns a new array)<br><span class="translate">추천 (새 배열 반환)</span></th></tr></thead><tbody><tr><td align="left">adding <br><span class="translate">추가</span></td><td align="left"><code class="inline text-code text-secondary dark:text-secondary-dark px-1 rounded-md no-underline bg-gray-30 bg-opacity-10 py-px">push</code>, <code class="inline text-code text-secondary dark:text-secondary-dark px-1 rounded-md no-underline bg-gray-30 bg-opacity-10 py-px">unshift</code></td><td align="left"><code class="inline text-code text-secondary dark:text-secondary-dark px-1 rounded-md no-underline bg-gray-30 bg-opacity-10 py-px">concat</code>, <code class="inline text-code text-secondary dark:text-secondary-dark px-1 rounded-md no-underline bg-gray-30 bg-opacity-10 py-px">[...arr]</code> spread syntax </td></tr><tr><td align="left">removing <br><span class="translate">삭제</span></td><td align="left"><code class="inline text-code text-secondary dark:text-secondary-dark px-1 rounded-md no-underline bg-gray-30 bg-opacity-10 py-px">pop</code>, <code class="inline text-code text-secondary dark:text-secondary-dark px-1 rounded-md no-underline bg-gray-30 bg-opacity-10 py-px">shift</code>, <code class="inline text-code text-secondary dark:text-secondary-dark px-1 rounded-md no-underline bg-gray-30 bg-opacity-10 py-px">splice</code></td><td align="left"><code class="inline text-code text-secondary dark:text-secondary-dark px-1 rounded-md no-underline bg-gray-30 bg-opacity-10 py-px">filter</code>, <code class="inline text-code text-secondary dark:text-secondary-dark px-1 rounded-md no-underline bg-gray-30 bg-opacity-10 py-px">slice</code></td></tr><tr><td align="left">replacing <br><span class="translate">교체</span></td><td align="left"><code class="inline text-code text-secondary dark:text-secondary-dark px-1 rounded-md no-underline bg-gray-30 bg-opacity-10 py-px">splice</code>, <code class="inline text-code text-secondary dark:text-secondary-dark px-1 rounded-md no-underline bg-gray-30 bg-opacity-10 py-px">arr[i] = ...</code> assignment</td><td align="left"><code class="inline text-code text-secondary dark:text-secondary-dark px-1 rounded-md no-underline bg-gray-30 bg-opacity-10 py-px">map</code></td></tr><tr><td align="left">sorting <br><span class="translate">정렬</span></td><td align="left"><code class="inline text-code text-secondary dark:text-secondary-dark px-1 rounded-md no-underline bg-gray-30 bg-opacity-10 py-px">reverse</code>, <code class="inline text-code text-secondary dark:text-secondary-dark px-1 rounded-md no-underline bg-gray-30 bg-opacity-10 py-px">sort</code></td><td align="left">copy the array first<br><span class="translate">배열을 복사한 다음 처리</span></td></tr></tbody></table>

<br>

## 감상

<br>

이번 장은 state에 대해 더 깊게 이해 할 수 있었다.

1. state는 컴포넌트의 메모리라는 것을 알게 되었고
2. state는 스냅샷처럼 동작한다는 것을 알게 되었다.
3. state를 업데이트 할 때는 불변성을 지켜야한다는 것을 알게 되었고, 불변성을 지키기 위해서는 객체나 배열을 업데이트 할 때는 새로운 객체나 배열을 만들어서 업데이트 해야한다는 것을 알게 되었다.

새롭게 리뉴얼 된 공식문서에서 이렇게 자세한 내용까지 다뤄주고 있어서 아주 좋다. 다음장으로 넘어가서 state에 대해 더 자세히 학습해보자.
