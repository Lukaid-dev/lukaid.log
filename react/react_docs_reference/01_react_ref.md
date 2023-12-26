ref로 DOM을 조작할 때 초기값으로 null을 넣는 이유

- 렌더링 전에는 useRef에 어떤 값을 넣을 수 없기 때문?
- HTML Element를 넣어야 하는데, 렌더링 전에는 아직 HTML Element가 생성되지 않았기 때문!
- 금방 바뀔 값이라는 의미이기도 함

```jsx
import { useRef } from 'react';

function MyComponent() {
  const inputRef = useRef(null);

  // ...

  return <input ref={inputRef} />;
```

그런 다음 ref 객체를 ref 속성으로 조작하려는 DOM 노드의 JSX에 전달하면, ref 객체의 current 속성은 해당 DOM 노드를 가리키게 된다.

그런 다음 React가 DOM 노드를 생성하고 화면에 그린 후, React는 ref 객체의 current프로퍼티를 DOM 노드로 설정합니다. 이제 DOM 노드 `<input>` 접근해 focus()와 같은 메서드를 호출할 수 있습니다.

```jsx
function handleClick() {
  inputRef.current.focus();
}
```
