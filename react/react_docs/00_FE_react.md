# 알게된 단편적 지식들

FE재남님의 리액트 공식문서 번역과 스터디 영상을 공부하면서 알게된 단편적 지식들을 정리한 글이다.

[리액트 공식문서](https://react.dev/)
[비공식 번역문서](https://react-ko.dev/)
[스터디 영상](https://youtube.com/playlist?list=PLjQV3hketAJkh6BEl0n4PDS_2fBd0cS9v&si=EqsHu_o7l1zGcDZd)

<br>
<br>

## [Quick Start](https://react-ko.dev/learn)

<br>

### [js escape](https://react-ko.dev/learn#displaying-data)

<br>

```jsx
return <h1>{user.name}</h1>;
```

<br>

간단한 내용인데, 이렇게 jsx에서 {}를 사용하면 JavaScript를 사용할 수 있다. 여기서 {} 이거를 JavaScript로 “이스케이프” 한다고 표현함

<br>

### [conditional rendering](https://react-ko.dev/learn#conditional-rendering)

<br>

보통 간단한 조건문을 작성할 때는 삼항연산자를 사용한다. else 분기가 필요하지 않은 경우 더 짧은 논리 && 구문을 사용할 수도 있다.

<br>

```jsx
<div>{isLoggedIn && <AdminPanel />}</div>
```

<br>

위 코드는 isLoggedIn이 true일 때만 AdminPanel 컴포넌트를 렌더링한다. 이렇게 하면 조건이 충족되지 않으면 null을 반환하기 때문에 컴포넌트가 렌더링되지 않는다.

> _**주의! &&의 왼쪽에 숫자를 넣지 마세요!**_ : 조건을 테스트하기 위해, JavaScript는 왼쪽을 자동으로 불리언으로 변환한다. 그러나 왼쪽이 0이면 전체 표현식이 해당 값(0)을 가져오고, React는 기꺼이 빈 값 대신 0을 렌더링한다. 그래서 falsy한 값이 나올 수 있는 숫자를 && 왼쪽에 두면 안된다.
> 꼼수로 `!!` 연산자를 이용해서 강제로 불리언으로 만들면 된다.

<br>

### [hook 사용하기](https://react-ko.dev/learn#using-hooks)

<br>

훅은 일반 함수보다 더 제한적으로 사용되어서, 컴포넌트(또는 다른 훅)의 최상위 레벨에서만 훅을 호출할 수 있다. 조건문이나 반복문에서 useState를 사용하고 싶다면, 대신 새로운 컴포넌트를 추출하고 그 컴포넌트에 작성해야 한다. 다시말해 다른 훅에서 훅을 또 불러와서 사용하면 안된다.

<br>

### [component간 정보 전달은 언제나 단방향](https://react-ko.dev/learn#sharing-data-between-components)

<br>

리액트를 처음 공부할 때, 그리고 바닐라JS를 이용해서 리액트처럼 구현하려고 했을 때, 이 정보의 전달에서 애를 많이 먹었다. **component간 정보 전달은 언제나 위에서 아래로 흐른다.**

컴포넌트에서 자식 컴포넌트로 데이터를 전달할 때는 props를 사용한다. props는 부모 컴포넌트가 설정하고, 자식 컴포넌트에서는 오직 **읽기 전용**으로만 사용할 수 있다. 그럼 자식에서 부모의 props를 변경하고 싶을 땐 어떡하나?

당연히 부모에서 해당 **prop을 변경하는 함수**도 같이 넘겨줘야 한다!!

<br>
<br>

## [tic-tac-toe](https://react-ko.dev/learn/tutorial-tic-tac-toe)

<br>

### [state 끌어올리기](https://react-ko.dev/learn/tutorial-tic-tac-toe#lifting-state-up)

<br>

여러 자식 컴포넌트에서 데이터를 수집하거나, 두 자식 컴포넌트가 서로 통신하도록 하려면, 부모 컴포넌트에서 공유 state를 대신 선언하는 것이 좋다. 부모 컴포넌트는 props를 통해 해당 state를 자식 컴포넌트에 다시 전달할 수 있다. 이렇게 해야 자식 컴포넌트가 서로 동기화되고 부모 컴포넌트와도 동기화되도록 유지할 수 있다.

<br>

```jsx
export default function Board() {
  const [squares, setSquares] = useState(Array(9).fill(null));
  return (
    <>
      <div className="board-row">
        <Square value={squares[0]} />
        <Square value={squares[1]} />
        <Square value={squares[2]} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} />
        <Square value={squares[4]} />
        <Square value={squares[5]} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} />
        <Square value={squares[7]} />
        <Square value={squares[8]} />
      </div>
    </>
  );
}
```

<br>

### js slice vs spread

<br>

```jsx
const squares = squares.slice();
```

```jsx
const squares = [...squares];
```

<br>

두 코드 모두 불변성을 유지하면서 배열을 복사한다. 같은 결과를 반환하지만, spread 연산자를 사용하는 것이 더 짧고 간단하다. 더 권장된다고 한다.

<br>
<br>

## [React로 사고하기](https://react-ko.dev/learn/thinking-in-react)

<br>

React에서 UI를 구현하려면 일반적으로 동일한 5단계를 따른다.

Step 1: Break the UI into a component hierarchy | UI를 컴포넌트 계층 구조로 나누기
Step 2: Build a static version in React | React로 정적인 UI 만들기
Step 3: Find the minimal but complete representation of UI state | 최소한의 완전한 UI state 찾기
Step 4: Identify where your state should live | state가 어디에 있어야 할지 파악하기
Step 5: Add inverse data flow | 역방향 데이터 흐름 추가하기

<br>

### [props vs state](https://react-ko.dev/learn/thinking-in-react#props-vs-state)

<br>

리액트에서 정보를 전달하는 유형은 크게 두가지가 있다. props와 state. 이 둘은 유사하지만 매우 다르다.

- Props는 함수가 전달받는 인자와 같다. 부모 컴포넌트가 자식 컴포넌트에 데이터를 넘겨서 자식의 외관을 커스터마이징할 수 있게 해준다. 예를 들어, Form은 color prop을 Button에 전달할 수 있다.
- State는 컴포넌트의 메모리와 같다. state는 컴포넌트가 일부 정보를 계속 추적하고 상호작용하여 변화할 수 있게 해준다. 예를 들어, Button은 isHovered state를 추적할 것이다.

props와 state는 서로 다르지만 함께 사용된다. 부모 컴포넌트는 종종 일부 정보를 (변경할 수 있도록) state에 보관하고, 이를 자식 컴포넌트에 props로 내려보낸다.

이는 이미 [03_props and state](https://velog.io/@lukaid/03props-and-state)에서 한번 자세히 정리한 적이 있다.
