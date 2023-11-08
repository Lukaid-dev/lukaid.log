# 알게된 단편적(이제는 단편적이 아니게 된...) 지식들 다섯번째 - state를 다루는 방법 (1)

<br>

[리액트 공식문서](https://react.dev/)
[비공식 번역문서](https://react-ko.dev/)
[스터디 영상](https://youtube.com/playlist?list=PLjQV3hketAJkh6BEl0n4PDS_2fBd0cS9v&si=EqsHu_o7l1zGcDZd)

<br>

## [Reacting to Input with State | state로 입력에 반응하기](https://react-ko.dev/learn/reacting-to-input-with-state)

<br>

React는 UI를 조작하는 **선언적인 방법**을 제공한다고 한다. 컴포넌트가 다양한 상태를 들고있으면서, 사용자 입력에 반응하여 각 상태들 사이를 전환한다. 이에 대해 자세히 들여다보자.

<br>

### [How declarative UI compares to imperative | 선언형 UI와 명령형 UI의 차이점](https://react-ko.dev/learn/reacting-to-input-with-state#how-declarative-ui-compares-to-imperative)

<br>

우선 선언형의 반대되는 개념인 명령형에 대해서 살짝 알아보자. 명령형 프로그래밍은 프로그램이 어떻게 동작해야 하는지를 명시하는 방식이다. UI를 조작하기 위한 정확한 지침을 작성해야하며, 각 요소에 “명령”을 내려 컴퓨터가 어떻게 UI를 업데이트해야할지 내용을 지시해야한다. 작은 프로그램에서는 문제가 없을 수 있겠지만, 프로그램이 커지면 커질수록 유지보수가 어려워진다. 이는 UI를 업데이트하는 방법을 명시하는 것이 아니라, UI가 어떻게 보여져야 하는지를 명시하는 것이 더 좋다는 것을 의미한다. React는 이런 문제를 해결하기 위해 만들어졌다.

<br>

React에서는 직접 UI를 조작하지 않는다. 즉, 컴포넌트를 직접 활성화하거나 비활성화 하지도, 보여주거나 숨기지도 않는다. 표시할 내용을 **선언**하면 React가 UI를 업데이트할 방법을 알아낸다.

<br>

택시를 타고 내가 가고싶은 곳까지 일일이 좌회전이요, 우회전이요 알려 줄 필요없이, 어디로 가고 싶은지만 말하면 된다! 목적지까지 데려다주는 것은 택시기사의 몫이며, React는 우리의 훌륭한 기사님이 되어 줄 것이다. (물론 진짜 택시 기사님처럼 내가 원하는 것만 말하면 다 만들어주진 않는다...)

<br>

### [Thinking about UI declaratively | UI를 선언적인 방식으로 생각하기](https://react-ko.dev/learn/reacting-to-input-with-state#thinking-about-ui-declaratively)

<br>

그래서 우리는 React로 사고하는 법에 조금 더 익숙해져야 한다. UI를 React로 구현하는 과정을 다시 살펴보면 아래와 같다.

<br>

**1. Identify** your component’s different visual states

- 컴포넌트의 모든 시각적 상태를 식별하자.

**2. Determine** what triggers those state changes

- 사람 및 컴퓨터가 상태 변화를 촉발하는 요소를 파악하자.

**3. Represent** the state in memory using useState

- useState로 상태를 모델링하자.

**4. Remove** any non-essential state variables

- 버그와 모순을 피하기 위해 필수적이지 않은 state를 제거하자.

**5. Connect** the event handlers to set the state

- 이벤트 핸들러를 연결하여 state를 설정하자.

<br>

아주 축약된 내용이지만, 내용이 아주 좋으니 반드시 공식분서를 참고하길 바란다. (미래의 나에게...)

<br>

## [Choosing the State Structure | State 구조 선택](https://react-ko.dev/learn/choosing-the-state-structure)

<br>

React는 상태를 잘 다루고 이를 이용해 UI를 잘 그리는 것이 전부다. 그래서 state가 굉장히 중요한데, 이런 state를 잘 구조화하면 수정과 디버깅이 편한 컴포넌트를 만들 수 있다고 한다. 이에 대해 학습해보자.

<br>

### [Principles for structuring state | state 구조화 원칙](https://react-ko.dev/learn/choosing-the-state-structure#principles-for-structuring-state)

<br>

각 컴포넌트에 작성할 state가 얼마나 있을지, 어떤 데이터를 담을지 잘 생각해봐야한다. 다음의 다섯가지 원칙을 살펴보자.

<br>

**1. 관련된 state들은 그룹화한다.**

- 두 개 이상의 state 변수를 항상 동시에 업데이트하는 경우가 있을 수 있다. 이럴 경우 단일 state 변수로 병합하는 것이 좋다.

**2. state의 모순을 피하라.**

- 여러 state 조각이 서로 모순되거나 ‘불일치’할 수 있는 방식으로 state를 구성하면 실수가 발생할 여지가 생긴다.

**3. 불필요한 state를 피하라.**

- 렌더링 중에 컴포넌트의 props나 기존 state 변수에서 일부 정보를 계산할 수 있다면 해당 정보를 해당 컴포넌트의 state에 넣지 않아야한다.

**4. state 중복을 피하라.**

- 동일한 데이터가 여러 state 변수 간에 또는 중첩된 객체 내에 중복되면 동기화 state를 유지하기가 어렵다. state는 적을수록 좋다.

**5. 깊게 중첩된 state는 피하라.**

- 깊게 계층화된 state는 업데이트하기 쉽지 않다. flat한 state를 유지하자. 중첩이 깊어지면 실수를 유발할 가능성도 높아진다.

<br>

음... 그러니까 최소한의 숫자로 최대한 간단하게 state를 작성하라는 것 같다. 근데 이를 어떻게 적용해야할지는 생각하기 어렵다. 어떻게 적용되는지 알아보자.

<br>

우선 **그룹화**에 대한 내용이다.

<br>

```jsx
const [x, setX] = useState(0);
const [y, setY] = useState(0);
```

or

```jsx
const [position, setPosition] = useState({ x: 0, y: 0 });
```

<br>

둘 중 어떤 코드가 나아보일까? 두개 다 틀린 방식이 아니고, 두 방식 모두 언젠가 반드시 필요한 순간이 있을 수 있다. 그러니까 정답은 없다. 하지만, 두 개의 state 변수가 항상 함께 변경되는 경우에는 하나의 state 변수로 통합하는 것이 좋다.

<br>

다음은 **state간 모순**에 대해 알아보자. 모순이라고 하니까 말이 좀 어려운데, “불가능한” state의 설정을 허용한다면 모순이 발생할 수 있다. 예를 들어, 메세지를 보내는 컴포넌트에서는 **isSending과 isSent는 동시에 true가 되어서는 안되므로**, 세 가지 유효한 state 중 하나를 취할 수 있는 status 라는 state변수 하나로 대체하는 것이 좋다: status는 typing(초기), sending, sent가 될 수 있다.

<br>

다음은 **불필요한 state**에 대한 이야기다.

<br>

```jsx
const [firstName, setFirstName] = useState("");
const [lastName, setLastName] = useState("");
const [fullName, setFullName] = useState("");
```

<br>

fullName은 firstName과 lastName을 합친 값이다. 상태를 바꿔주는 것보다 매번 계산하는 것이 더 경제적일 때도 있다.

<br>

### [Don’t mirror props in state | props를 state에 그대로 미러링하지 마세요](https://react-ko.dev/learn/choosing-the-state-structure#don-t-mirror-props-in-state)

<br>

```jsx
function Message({ messageColor }) {
  const [color, setColor] = useState(messageColor);
```

<br>

불필요한 state를 만들지 않기 위해, props를 state에 그대로 미러링하지 않는 것이 좋다. 위의 코드는 불필요한 state를 만들고 있다. props를 그대로 사용하면 된다. 추가적으로, 부모 컴포넌트가 나중에 다른 messageColor 값(예: blue 대신 red)을 전달하면 setColor를 부르지 않기 때문에 color state 변수가 업데이트되지 않는다.

<br>

**중복된 state**와 **중첩된 state**도 좋지 않은 방식이다. 중복된 state는 동기화하기 어렵고, 중첩된 state는 업데이트하기 어렵다. 이런 문제를 피하기 위해 state를 최소화하고, flat하게 유지하는 것이 좋다.

<br>

요약하자면, state를 구조화하는 방법은 다음과 같다.

<br>

- 두 개의 state 변수가 항상 함께 업데이트되는 경우 두 변수를 하나로 병합하자.
- state 변수를 신중하게 선택하여 ‘불가능한’ state를 만들지말자.
- state를 업데이트할 때 실수할 가능성을 줄이는 방식으로 state를 구성하자.
- 동기화 state를 유지할 필요가 없도록 불필요 및 중복 state를 피하자.
- 특별히 업데이트를 막으려는 경우가 아니라면 props를 state에 그대로 미러링하지 말자.
- 선택과 같은 UI 패턴의 경우 객체 자체 대신 ID 또는 인덱스를 state로 유지하자.
- 깊게 중첩된 state를 업데이트하는 것이 복잡하다면 state를 평평하게 만들자.

<br>

## [Sharing State Between Components | 컴포넌트 간의 state 공유](https://react-ko.dev/learn/sharing-state-between-components)

<br>

보통은 컴포넌트 끌어올리기라고 하며, 상위 컴포넌트에서 하위 컴포넌트로 state를 전달하는 것을 의미한다. 이를 통해 하위 컴포넌트 간에도 state를 공유할 수 있다.

<br>

### [A single source of truth for each state | 각 state의 단일 진실 공급원(SSOT)](https://react-ko.dev/learn/sharing-state-between-components#a-single-source-of-truth-for-each-state)

<br>

React로 작성한 App의 많은 컴포넌트는 각각 고유한 state를 가질 수 있다. root에 가까운 컴포넌트에도 state가 있을 수 있고, leaf(가장 말단)에 가까운 컴포넌트에도 state가 있을 수 있다.

<br>

각 고유한 state들에 대해 해당 state를 “소유”하는 컴포넌트를 선택하게 된다. (For each unique piece of state, you will choose the component that “owns” it.) 이런 원칙을 “single source of truth(단일 진실 공급원)”라고 말하기도 한다.

<br>

무슨말인지 좀 어려운데, 쉽게말해서, 각 state마다 해당 정보를 소유한 특정 컴포넌트가 존재한다는 것을 의미한다. 그래서 컴포넌트 간 정보 공유에는 state를 복제(SSOT 위배)하는 것이 아니라, 공통의 부모로 **state를 끌어올리는 것**이 좋다.

<br>

## 감상

<br>

다음 파트까지 학습 후 한꺼번에 ㄱㄱ
