# execution context

<br>

JavaScript에서 가장 중요한 개념 중 하나인 실행 컨텍스트. 여전히 단어의 조합만으로는 어떤 역할을 하는 녀석인지 감이 전혀오지 않는다. 하지만 이녀석만 이해하면, hoisting, scope, closure, this 등의 그간 이해하기 어려웠 던 개념들을 이해하는데 큰 도움이 될 것이다. (언급되는 녀석들의 체급을 봐도 만만치 않은 놈인건 확실하다.)

<br>

한 문장으로 줄이기는 상당히 어렵겠지만, 많은 시간 구글링을 하고 학습 한 결과 나름대로의 정의를 내려보자면, 실행 컨텍스트는

> **실행할 코드에 제공할 환경 정보들을 모아놓은 객체**

라고 할 수 있겠다.

<br>

실행 컨텍스트는 다음의 네가지 유형의 코드가 실행될 때 생성된다.

- **전역 코드 global code**
  - 전역에 존재하는 소스 코드. 해당 코드가 평가되면 전역 실행 컨텍스트가 생성된다.
- **함수 코드 function code**
  - 함수 내부에 존재하는 소스 코드. 함수 코드가 평가되면 함수 실행 컨텍스트가 생성된다.
- **eval 코드 eval code**
  - eval 함수에 인수로 전달되어 실행되는 소스 코드. strict mode에서 eval 실행 컨텍스트를 생성한다.
  - eval : 문자열로 표현된 자바스크립트 코드를 실행하는 데 사용 되는 자바스크립트 내장 함수. 보안상의 이유로 사용자 입력과 같은 외부에서 제공된 문자열을 eval로 실행하는 것을 주의해야 한다. (그냥 이제는 안쓴다고 생각하면 된다.)
- **모듈 코드 module code**
  - 모듈 내부에 존재하는 소스 코드. 각 모듈은 평가시 모듈별로 독립적인 모듈 실행 컨텍스트를 생성한다.

<br>

실행 컨텍스트가 생성 될 때, scope를 관리하는 역할은 `Lexical Environment`가 담당하고, 코드의 실행 순서를 관리하는 역할은 `Execution Context Stack`(Call Stack)이 담당한다.

<br>

<center>
  <img src="https://file.notion.so/f/f/88d8f7ec-c29e-4286-aad0-9b660662d1a4/7f834bdf-7daa-466d-b12c-c5cfecd620c2/Untitled.png?id=5f56cf76-e36f-4dfb-8b9f-fbbdf0e45f4b&table=block&spaceId=88d8f7ec-c29e-4286-aad0-9b660662d1a4&expirationTimestamp=1705334400000&signature=L-SXXQZTO6TKdLNJaNqztuGaczH9a9CnUcoWMpWMQMY&downloadName=Untitled.png">
</center>

<br>

자바스크립트는 상단의 4가지 소스 코드가 실행될 때 마다 실행 컨텍스트를 생성한다. 실행 컨텍스트들은 스택 자료구조로 관리된다. 이 스택 자료구조를 **실행 컨텍스트 스택**이라고 한다.

4가지 소스 코드가 실행되면 실행 컨텍스트가 생성되어 실행 컨텍스트 스택에 push되며, 코드 실행이 마무리되면 해당 코드의 실행 컨텍스트가 pop된다. 이처럼 실행 컨텍스트 스택에 실행 컨텍스트들이 순서대로 push되고 pop되면서 현재 실행중인 코드가 무엇인지 알 수 있으며, 실행 중인 코드가 끝난 후 어느 코드로 되돌아 갈지도 알 수 있다.

이처럼 실행 컨텍스트 스택은 **코드의 실행 순서를 관리**한다.

<br>

하나의 실행 컨텍스트는 다음과 같은 구조로 이루어져 있다.

<br>

- **VariableEnvironment**
  - 현재 컨텍스트 내의 식별자들에 대한 정보 (`EnvironmentRecord` 이로 인해 hoisting이 발생한다.)
  - 외부 환경 정보 (`OuterEnvironmentReference` 이로 인해 scope와 scope chain이 형성된다.)
  - 선언 시점에서 스냅샷처럼 동작하여 이후의 변경 사항은 반영되지 않음
- **LexicalEnvironment**
  - VariableEnvironment와 같지만, 변경 사항이 실시간으로 반영됨
- **ThisBinding**
  - this 식별자가 바라봐야 할 대상 객체
  - this 바인딩은 함수 호출 패턴에 의해 동적으로 결정된다.

<br>

`LexicalEnvironment`의 내부에는 `environmentRecord`와 `outerEnvironmentReference`로 구성돼 있다.

<br>

- **EnvironmentRecord**
  - 실행 컨텍스트 내부에서 선언된 변수를 관리하는 저장소
  - 호이스팅이 발생하는 원인
- **OuterEnvironmentReference**
  - 실행 컨텍스트가 생성된 시점에 실행 중인 코드의 LexicalEnvironment를 참조하여 외부 환경에 대한 참조를 기록
  - 스코프 체인을 구현하는 매커니즘
  - Javascript는 오직 함수에 의해서만 스코프가 생성된다.

<br>

여기까지 정리하는데 생각보다 많은 시간을 쓰고있다... 이제 호이스팅과 스코프, 스코프 체인에 대해 간단하게만 정리하고 넘어가자.

<br>

### 호이스팅

<br>

JavaScript에서 호이스팅은 함수와 변수의 선언문을 **해당 스코프의 선두로 옮긴 것처럼 동작하는 특성**을 말한다. 호이스팅은 오직 선언문만 해당되며, 모든 선언문(var, let, const, function, function\*, class)에 적용된다. 이들을 식별자라고도 부르며, 실행 컨텍스트가 생성 될 때 `EnvironmentRecord`에 저장된다. 이때, 변수의 경우는 선언 당시의 값이 아닌 `undefined`로 초기화 된다. 함수의 경우는 함수 객체가 저장된다.

<br>

es6부터는 let과 const 타입의 변수를 선언 할 경우 선언과 초기화가 분리되어 진행된다. 간단히 말하면 식별자만을 수집하고 초기화는 하지 않는다. 이때, 선언은 `EnvironmentRecord`에, 초기화는 `LexicalEnvironment`에 진행된다. 이렇게 분리되어 진행되는 이유는 `Temporal Dead Zone` 때문이다. `Temporal Dead Zone`은 변수가 선언되기 전까지는 변수를 참조할 수 없다는 것을 말한다. 이러한 `Temporal Dead Zone`을 통해 변수의 선언이 변수의 스코프에 속하는지 여부를 확인할 수 있다.

<br>

JavaScript에서 함수를 선언하는 방법은 크게 세가지가 있다. 함수 선언문, 함수 표현식, 화살표 함수 표현식이다. 이 세가지 방법은 호이스팅에 따라 다르게 동작한다.

먼저 함수 선언문의 경우 호이스팅이 동작한다. 선언, 초기화 부분이 함께 호이스팅된다. 그래서 코드 어디서든 함수 호출이 가능하다.

<br>

```jsx
foo(); // foo
function foo() {
  console.log("foo");
}
foo(); // foo
```

<br>

다음으로 함수 표현식의 경우 호이스팅이 동작하지 않는다. 선언 부분만 호이스팅되고, 초기화 부분은 호이스팅 되지 않는다. 그래서 함수를 선언한 다음에 호출해야 한다.

<br>

```jsx
foo(); // Uncaught TypeError: foo is not a function
var foo = function () {
  console.log("foo");
};
foo(); // foo
```

<br>

마지막으로 화살표 함수 표현식의 경우 함수 표현식과 동일하게 동작한다.

<br>

```jsx
foo(); // Uncaught TypeError: foo is not a function
var foo = () => {
  console.log("foo");
};
foo(); // foo
```

<br>

### this 바인딩

<br>

그럼 함수 표현식과 화살표 함수는 차이가 없는 것일까? 당연히 그렇지 않다.

함수 표현식과 함수 선언문 내부의 this는 호출한 컨텍스트에 따라 동적으로 결정되지만, 화살표 함수 내부의 this는 상위 스코프의 this를 그대로 활용한다. 그래서 화살표 함수는 상위 스코프의 this를 그대로 활용한다. (따라서 화살표 함수는 생성자 함수로 사용할 수 없다.)

this가 정적으로 바인딩 된다는 것은, 코드가 실행되기 전에 선언되는 시점에서 this가 결정되는 것을 의미한다. 다시말해, 가장 근접한 상위 스코프에 바인딩 된다는 것이다.

반대로 동적 바인딩은 함수가 실행되는 시점에 this가 결정되는 것을 의미한다. 주로 화살표 함수나 함수를 콜백으로 전달할 때, 그리고 명시적으로 this를 지정하는 경우에 동적 바인딩이 일어난다.

<br>

### 스코프

<br>

스코프는 식별자가 유효한 범위를 말한다. 스코프는 함수의 중첩 관계로 생성되며, 함수 내부에서 외부 함수의 식별자에 접근할 수 있지만, 외부 함수는 내부 함수의 식별자에 접근할 수 없다. 이러한 중첩 관계를 스코프 체인이라고 한다. 대부분의 프로그래밍 언어는 블록 레벨 스코프를 따르지만, JavaScript는 함수 레벨 스코프를 따른다.

<br>

### 스코프 체인

<br>

식별자의 유효범위를 가장 가까운 쪽에서부터 먼쪽으로 검색해 나가는 것을 말한다. 보통 `Local -> Script -> Global`순으로 진행된다. 실행컨텍스트에서 `outerEnvironmentReference`는 이러한 스코프 체인을 구현하는 매커니즘이다.

<br>

이제 실제 예제를 통해서 개념을 정리해보자.
<br>

## 생활코딩님 예제

<br>

아래의 예제를 보면서 실행 컨텍스트가 어떻게 생성되고, 스코프 체인이 형성되는지 살펴보자.

<br>

```html
<script>
  n0 = "n0"; // global scope
  var v0 = "v0"; // global scope
  let l0 = "l0"; // script scope
  const c0 = "c0"; // script scope
  console.log(v0, n0, l0, c0); // v0, n0, l0, c0
  console.log(window.v0, window.n0, window.l0, window.c0); // v0, n0, undefined, undefined
  function fn2() {
    n2 = "n2"; // global scope
    console.log(n0, n1, n2);
    var v2 = "v2"; // local scope
    console.log(v0, v2);
    // console.log(v1) // v1 is not defined
    let l2 = "l2"; // local scope
    console.log(l0, l2); // l0, l2
    // console.log(l1);
    const c2 = "c2;"; // local scope
    console.log(c0, c2); // c0, c2
    // console.log(c1);
  }
  function fn1() {
    n1 = "n1"; // global scope
    var v1 = "v1"; // local scope
    let l1 = "l1"; // local scope
    const c1 = "c1"; // local scope
    fn2(); // fn2가 선언 될 때의 scope를 기억하고 있음
  }
  fn1();
  console.log(n2);
  // console.log(v2, l2, c2); // v2, l2, c2 is not defined
</script>
```

<br>

위의 예제를 보면, 어떤 레벨에서 변수와 함수가 선언 될 때, 해당 변수와 함수들이 어느 스코프에 들어가는지 대략적으로 이해할 수 있다. 정리를 하면 다음과 같다.

<br>

### global execution context에서 선언된 변수와 함수

<br>

| 변수      | 스코프 |
| --------- | ------ |
| a=1       | global |
| var a=1   | global |
| let a=1   | script |
| const a=1 | script |

<br>

### local execution context에서 선언된 변수와 함수

<br>

| 변수      | 스코프 |
| --------- | ------ |
| a=1       | global |
| var a=1   | local  |
| let a=1   | local  |
| const a=1 | local  |

<br>

## 정리

<br>

면접 스터디를 하면서도 상당히 어려웠던 개념이라 꼭 한번 정리해야지 생각만 하고 있었는데, 이번 기회에 한번 싹 정리하니 마음이 좋다. 사실 부족한 내용이 많긴 하지만, 스스로 이해한 바를 정리하려고 노력했고, 아직 이해되지 않는 부분이나 어려운 부분들은 직접 경험을 쌓아가면 이해도를 올린 후 다시 정리 해보려 한다.

<br>

### 출처

[생활코딩님 유튜브](https://www.youtube.com/watch?v=QtOF0uMBy7k&ab_channel=%EC%83%9D%ED%99%9C%EC%BD%94%EB%94%A9)
[준일님 블로그](https://junilhwang.github.io/TIL/Javascript/Domain/Execution-Context/#_2-%E1%84%89%E1%85%B5%E1%86%AF%E1%84%92%E1%85%A2%E1%86%BC-%E1%84%8F%E1%85%A5%E1%86%AB%E1%84%90%E1%85%A6%E1%86%A8%E1%84%89%E1%85%B3%E1%84%90%E1%85%B3-%E1%84%80%E1%85%AE%E1%84%89%E1%85%A5%E1%86%BC)
