# 알게된 단편적 지식들 두번째

<br>

[리액트 공식문서](https://react.dev/)
[비공식 번역문서](https://react-ko.dev/)
[스터디 영상](https://youtube.com/playlist?list=PLjQV3hketAJkh6BEl0n4PDS_2fBd0cS9v&si=EqsHu_o7l1zGcDZd)

<br>

## [Import and Export](https://react-ko.dev/learn/importing-and-exporting-components)

<br>

### .js 확장자가 없는 이유

<br>

가끔 .js와 같은 파일 확장자가 없는 때도 있다.

<br>

```jsx
import Gallery from "./Gallery";
```

<br>

React에서는 './Gallery.js' 또는 './Gallery' 둘 다 사용할 수 있지만 전자의 경우가 ES Modules 사용 방법에 더 가깝다고 한다. 안쓰는건 번들러들이 알아서 넣어주는 거라고 한다. 그냥 확장자 붙여서 사용하자.

<br>

### [Default vs named exports](https://react-ko.dev/learn/importing-and-exporting-components#default-vs-named-exports)

<br>

ESM에서는 default export와 named export라는 두 가지 방법으로 값을 export 한다. 둘은 어떤 차이가 있을까? 우선 **한 파일에서는 하나의 default export만 존재할 수 있고 named export는 여러 개가 존재할 수 있다.** 는 것을 먼저 알아두자. (하나의 default export와 여러 개의 named export를 동시에 사용할 수 있다.)

<br>

<table><thead><tr><th>Syntax</th><th>Export statement</th><th>Import statement</th></tr></thead><tbody><tr><td>Default</td><td><code class="inline text-code text-secondary dark:text-secondary-dark px-1 rounded-md no-underline bg-gray-30 bg-opacity-10 py-px">export default function Button() {}</code></td><td><code class="inline text-code text-secondary dark:text-secondary-dark px-1 rounded-md no-underline bg-gray-30 bg-opacity-10 py-px">import Button from './Button.js';</code></td></tr><tr><td>Named</td><td><code class="inline text-code text-secondary dark:text-secondary-dark px-1 rounded-md no-underline bg-gray-30 bg-opacity-10 py-px">export function Button() {}</code></td><td><code class="inline text-code text-secondary dark:text-secondary-dark px-1 rounded-md no-underline bg-gray-30 bg-opacity-10 py-px">import { Button } from './Button.js';</code></td></tr></tbody></table>

<br>

잘 정리된 위의 표를 보면 알 수 있듯, default import를 사용한다면 import 단어 후에 다른 이름으로 값을 가져올 수 있다. 예를 들어, import Banana from './button.js' 라고 쓰더라도 같은 default export 값을 가져오게된다. 반대로 named import를 사용할 때는 양쪽 파일에서 사용하고자 하는 값의 이름이 같아야 해서 named import라고 불린다.

<br>

보편적으로 한 파일에서 하나의 컴포넌트만 export 할 때 default export 방식을 사용하고 여러 컴포넌트를 export 할 경우엔 named export 방식을 사용한다. (예를들면, `utils.jsx`라는 파일에 여러 유틸함수를 작성했다면 named export를 사용하는 것이 좋다.) 어떤 방식을 사용하든 컴포넌트와 파일의 이름을 의미 있게 명명하는 것은 중요하다. export default () => {} 처럼 이름 없는 컴포넌트는 나중에 디버깅하기 어렵기 때문에 권장하지 않는다.

<br>

## [Writing Markup with JSX](https://react-ko.dev/learn/writing-markup-with-jsx)

<br>

JSX(JavaScript XML)는 JavaScript의 확장 문법으로, React에서 UI를 작성할 때 사용한다. JSX는 HTML과 비슷하지만 JavaScript의 모든 기능을 사용할 수 있다. 컴포넌트를 작성하는 다른 방법(`React.createElement()`)도 있지만, 대부분의 React개발자는 JSX의 간결함을 선호하며 대부분의 코드베이스에서 JSX를 사용한다. (JSX를 사용하지 않고 React를 사용하는 것은 가능하지만, 권장하지 않는다. JSX와 React는 서로 다른 별개의 개념이고, 각각 사용될 수 있지만, 굳이 그러지말자.)

<br>

### [JSX 규칙](https://react-ko.dev/learn/writing-markup-with-jsx#the-rules-of-jsx)

1. 단일 루트 엘리먼트를 반환해야 한다.
2. 모든 태그는 닫혀있어야 한다.
3. ~~거의~~ 대부분이 카멜 케이스로 작성되어야 한다. (예를 들어, onclick은 onClick으로 작성해야 한다.)

<br>

### [왜 여러 JSX태그를 하나로 감싸줘야 할까?](https://react-ko.dev/learn/writing-markup-with-jsx#why-do-multiple-jsx-tags-need-to-be-wrapped)

<br>

jsx는 JavaScript 객체로 변환된다. 하나의 배열로 감싸지 않은 하나의 함수에서는 두 개의 객체를 반환할 수 없다. 따라서 또 다른 태그나 Fragment(`<></>`)로 감싸지 않으면 두 개의 JSX태그를 반환할 수 없다.

<br>

## [Passing Props to a Component](https://react-ko.dev/learn/passing-props-to-a-component)

<br>

React 컴포넌트는 props를 이용해서 정보를 전달한다. HTML의 속성(attribute)이 먼저 생각 날 수 있는데, 객체, 배열, 함수를 포함한 모든 JavaScript 값을 전달할 수 있다.

<br>

## [Passing JSX as children](https://react-ko.dev/learn/passing-props-to-a-component#passing-jsx-as-children)

<br>

다음과 같이 컴포넌트를 중첩해서 쓰고 싶을 때가 있을 수 있다.

<br>

```jsx
<Card>
  <Avatar />
</Card>
```

<br>

JSX 태그 내에 콘텐츠를 중첩하면, 부모 컴포넌트는 해당 컨텐츠를 children이라는 prop으로 받는다.

<br>

```jsx
import Avatar from "./Avatar.js";

function Card({ children }) {
  return <div className="card">{children}</div>;
}

export default function Profile() {
  return (
    <Card>
      // Avatar 컴포넌트가 Card 컴포넌트의 children prop으로 전달된다.
      <Avatar
        size={100}
        person={{
          name: "Katsuko Saruhashi",
          imageId: "YfeOqp2",
        }}
      />
    </Card>
  );
}
```

<br>

children에 반드시 컴포넌트가 들어갈 필요는 없고, 어떤 js 값이든 들어갈 수 있다.

<br>

### [props are immutable](https://react-ko.dev/learn/passing-props-to-a-component#how-props-change-over-time)

<br>

이 장의 원래 재목은 "How props change over time - 시간에 따라 props가 변하는 방식"인데 위의 제목이 더 적절 할 것 같아 바꿨다. 말 그대로 props는 불변하는 값이다. 컴포넌트가 props를 변경해야 하는 경우, 부모 컴포넌트로부터 다른 props, 그러니까 새로운 객체를 전달해달라고 요청해야한다. 이전의 props는 버리고(참조를 끊고), js엔진은 기존 props가 차지했던 메모리를 회수한다.

<br>

**props는 read-only이며, 변경을 시도하지 말자.**

<br>

## [Rendering Lists](https://react-ko.dev/learn/rendering-lists)

<br>

목록을 렌더하고 싶은 경우는 상당히 많다. 리액트를 공부하면서, 결국 리액트보다 js를 잘 하는것이 중요하다고 느끼는 경우를 자주 만나는데 이번도 그런 경우다. React에서도 `filter()`와 `map()`을 사용해 데이터 배열을 필터링하고 컴포넌트 배열로 변환할 수 있다.

<br>

### [Keeping list items in order with key](https://react-ko.dev/learn/rendering-lists#keeping-list-items-in-order-with-key)

<br>

> Warning: Each child in a list should have a unique “key” prop.

<br>

각 배열 항목에는 해당 배열의 항목들 사이에서 고유하게 식별할 수 있는 문자열 또는 숫자인 key를 부여해야한다. 그렇지 않으면 위의 오류를 만나게 될 것이다. key는 React가 컴포넌트를 업데이트할 때 어떤 항목을 변경, 추가 또는 삭제할지 결정하는 데 도움이 된다. 보통 서버에서 내려주는 id를 사용한다고 한다.

<br>

근데 key를 적어주지 않아도 렌더링 자체는 잘 될 것이다. key를 지정하지 않으면, React는 인덱스를 key로 사용한다. 그러나 우리가 렌더링한 항목의 순서는 새 항목이 삽입되거나, 삭제되거나, 배열의 순서가 바뀌는 등에 따라 변경될 수 있다. 인덱스를 key로 사용하면 종종 미묘하고 혼란스러운 버그가 발생한다. 그러니 웬만하면 key를 지정해주자.

<br>

## [Keeping Components Pure](https://react-ko.dev/learn/keeping-components-pure)

<br>

함수형 프로그래밍을 공부하면서 많은 어려움이 있었는데, 순수함수도 그 중 하나였다. 순수함수는 **입력이 같으면 항상 같은 출력을 반환하는 함수**다. 순수함수는 외부 상태를 변경하지 않고, 외부 상태에 의존하지도 않는다. 순수함수는 외부 상태를 변경하지 않기 때문에, 항상 동일한 입력에 대해 동일한 출력을 반환하고, 외부 상태에 의존하지 않기 때문에, 외부 상태가 변경되어도 영향을 받지 않는다.

<br>

컴포넌트를 엄격하게 순수 함수로만 작성하면 코드베이스가 커짐에 따라 당황스러운 버그와 예측할 수 없는 동작을 피할 수 있다. 모든것은 트레이드오프가 있듯, 이러한 이득을 보려면 몇가지 규칙을 지켜야한다.

<br>

React는 여러분이 작성하는 모든 컴포넌트가 순수 함수라고 가정한다. 즉, 여러분이 작성하는 React 컴포넌트는 동일한 입력이 주어졌을 때 항상 동일한 JSX를 반환해야 한다. 컴포넌트는 오직 JSX만을 반환해야 하며, 렌더링 전에 존재했던 객체나 변수를 변경해서는 안 된다. 이는 컴포넌트를 불순하게 만들 수 있다!

<br>

### [Strict Mode](https://react-ko.dev/learn/keeping-components-pure#detecting-impure-calculations-with-strict-mode)

<br>

리액트는 모든것을 두번 계산하도록 하는 Strict Mode를 지원하고, CRA이나 Vite등으로 프로젝트를 시작하면 항상 적용되어 있다. 이렇게 번거로운 Strict Mode를 왜 사용하는 걸까?

<br>

React에서는 렌더링하는 동안 읽을 수 있는 입력이 세 가지 있다: props, state, context. 이러한 입력은 항상 읽기 전용으로 취급해야 한다.

<br>

사용자 입력에 대한 응답으로 무언가를 변경하려면 *변수*에 쓰는 대신 **state** 설정해야 한다. 컴포넌트가 렌더링되는 동안에는 기존 변수나 객체를 절대 변경해서는 안 된다.

<br>

Strict Mode는 사용자의 모든 컴포넌트가 완전히 순수 할 것을 가정하여, 첫번째의 렌더와 두번째의 렌더가 완전히 같은 결과를 반환하는지 확인한다. 만약 두 렌더가 다른 결과를 반환한다면, Strict Mode는 경고를 표시한다. 이는 컴포넌트가 불순하게 되었음을 의미한다.

<br>

## [Where you can cause side effects](https://react-ko.dev/learn/keeping-components-pure#where-you-_can_-cause-side-effects)

<br>

이렇듯 리액트와 사용자는 모든 컴포넌트를 순수하게 유지하기 위해 노력해야 한다. 하지만 모든 코드를 이렇게 순수하게 짤수는 없다. 그러면 아무짝에도 쓸모없는 컴포넌트가 되어버린다. 언젠가는 어딘가에서 무언가는 바뀌어야 한다. 화면 업데이트, 애니메이션 시작, 데이터 변경과 같은 이러한 변경을 **사이드 이펙트**라고 하며, 렌더링 중에 일어나는 것이 아니라 “부수적으로” 일어나는 일이다. (여기서 side effect는 부정적인 개념이 아니라 단순히 렌더링 중에 일어나지 않는, 무언가를 변화시키는 것을 의미한다.)

<br>

React에서 사이드 이펙트는 보통 **이벤트 핸들러**가 담당한다. 이벤트 핸들러는 사용자가 어떤 동작을 수행할 때(예를 들어, 버튼을 클릭할 때) React가 실행하는 함수인데, 이는 컴포넌트 내부에 정의되어 있긴 하지만 렌더링 중에는 실행되지 않는다! 따라서 이벤트 핸들러는 순수할 필요가 없다!!

<br>

다른 모든 옵션을 다 사용했는데도 사이드 이펙트에 적합한 이벤트 핸들러를 찾을 수 없다면, 컴포넌트에서 useEffect 호출을 통해 반환된 JSX에 이벤트 핸들러를 첨부할 수 있다. 이렇게 하면 나중에 렌더링 후 사이드 이펙트가 허용될 때 React가 이를 실행하도록 지시하지만, 이는 최후의 수단으로 여겨야한다.

<br>
<br>
<br>

## 감상

그동안 리액트를 띄엄띄엄 학습했고, 이번처럼 제대로 학습한적이 없었는데, 공식문서 스터디를 하니 확실히 개념이 잡히는 것 같다. 왜 많은 개발자들이 공식문서의 중요성을 역설하는지 다시금 깨달은 것 같다. 상당히 많은 내용이 있지만, 아주 상세히 설명해두었고, 내용이 많은만큼 내가 알아야 할 리액트의 거의 모든 내용을 담고있어서 일단 다음주까지의 목표는 공식문서를 일독하는 것! 힘내보자!!
