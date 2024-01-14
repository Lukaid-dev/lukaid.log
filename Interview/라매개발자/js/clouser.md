# 클로저(Closure)에 대해 설명해주세요.

> 클로저는 주변 상태(어휘적 환경, Lexical scoping)에 대한 참조와 함께 묶인(포함된) 함수의 조합이다. 그러니까, 클로저는 내부 함수에서 외부 함수의 범위에 대한 접근을 제공한다. JavaScript에서 클로저는 함수 생성 시 함수가 생성될 때마다 생성된다.

무슨 말인지 진짜 어렵다... 처음 이 클로저 개념을 공부할 때 생소한 단어와 이해가 안 되는 문장들로 상당히 곤란했던 기억이 있다. 우선 어휘적 환경(Lexical scoping)은 뭘까? 어휘적 환경은 쉽게 말해서, 변수가 코드 내부 어디에 선언되었는지를 말하는 것이다. 호출되는 방식에 따라 동적으로 결정되는 this와는 다르게, 코드가 작성된 순간 정적으로 정해져 버린다. 클로저는 이러한 "선언된 어휘적 환경"을 조합해서 코딩하는 기법이라고 할 수 있다.

```jsx
function init() {
  var name = "Mozilla"; // name은 init에 의해 생성된 지역 변수이다.
  function displayName() {
    // displayName() 은 내부 함수이며, 클로저다.
    console.log(name); // 부모 함수에서 선언된 변수를 사용한다.
  }
  displayName();
}
init(); // Mozilla
```

위 코드에서 `init()`은 지역 변수 `name`과 함수 `displayName()`을 생성한다. 그렇다고 이들을 반환하는 것은 아니고, 변수 `name`과 함수 `displayName()`은 `init()`함수 본문(어휘적 환경)에서만 사용할 수 있다. `displayName()`은 함수 레벨의 스코프에서 변수를 가지고 있지 않지만, 클로저는 내부 함수에서 외부 함수의 변수에 접근할 수 있기 때문에 선언된 어휘적 환경에서 변수 `name`에 접근할 수 있게 된다.

당신이 react를 사용한다면, 아마도 거의 매일 이 클로저 개념을 사용할 가능성이 높은데, react의 다양한 hook들이 클로저로 선언되어 있고 가장 대표적으로 `useState`가 그렇다. `useState`를 아주 단순화해 보면 다음과 같다. (실제로 이렇게 구현되어 있지는 않았을 것이다.)

```jsx
function useState(initialValue) {
  let value = initialValue;
  const state = () => value;
  const setState = (newValue) => {
    value = newValue;
  };

  return [state, setState];
}
```

외부 함수 `useState()`가 반환한 내부 함수 `setState()`는 외부 함수 `useState()`의 호출이 끝났음에도 자신이 선언된 외부 함수가 선언된 어휘적 환경을 기억하기 때문에 계속해서 state 값을 사용할 수 있다.

출처:

https://developer.mozilla.org/ko/docs/Web/JavaScript/Closures#%EC%96%B4%ED%9C%98%EC%A0%81_%EB%B2%94%EC%9C%84_%EC%A7%80%EC%A0%95lexical_scoping

[모던 리액트 딥다이브](https://wikibook.co.kr/react-deep-dive/)
