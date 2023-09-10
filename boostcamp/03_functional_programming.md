# 함수형 프로그래밍이란?

부스트캠프 챌린지 과정을 진행하면서 가장 어려웠던 개념이 두가지가 있었다. 하나는 _"함수형 프로그래밍"_ 이고 다른 하나는, _"비동기 프로그래밍"_ 이다. 나는 데이터 분석을 위해서 python을 다루면서 개발에 처음 입문하였고, django를 사용하며 현업에서 근무했다. 그래서 객체지향 프로그래밍에 대해서는 많이 공부했고, 어느정도 자신있었지만, 함수형과 비동기는 정말 어렵게 느껴졌다. 그래서 이번에 함수형 프로그래밍에 대해서 공부하면서, 함수형 프로그래밍이란 무엇인지, 어떤 특징이 있는지, 어떻게 사용하는지에 대해서 정리해보려고 한다.

<br>

함수형 프로그래밍이란, 함수 중심 프로그래밍이라고도 볼 수 있는데, 자료처리를 수학적 함수의 계산으로 표현하고, 상태와 가변 데이터 대신 **불변 데이터**를 프로그래밍하는 패러다임이다. 그동안은 프로그래밍에서의 함수는 수학에서의 그것과 많이 다르다고 배워왔는데, 함수 중심 프로그래밍 패러다임에서는 드디어 수학적 함수의 정의와 비슷한 함수의 정의가 된다. 그 특징은 다음과 같다.

- 순수함수
- 불변타입
- 일급함수와 고차함수
- 자동 메모리 관리
- 타입시스템 (타입추론)

<br>

## 1. 순수함수

프로그래밍을 맨 처음 배웠을 때, 내가 잘 아는 수학의 개념이 프로그래밍에서의 그것과 많이 달라서 당황했던 기억이 있다. 함수도 그 중 하나였는데, 순수함수의 개념을 보니 수학에서의 개념과 많이 유사하다고 생각이 든다.

- **side effect가 없는 함수**: 함수안에서 처리하는 모든 로직이 함수 밖에 영향을 주지 않는다.
- **변경 불가능한 데이터(immutable)**: 함수의 인자로 전달된 데이터는 변경되지 않는다. 만약 값의 변경이 있다면, 새로운 데이터를 생성해서 반환한다.
- **참조 투명성(referential transparency)**: 부작용이 없다는 의미와 비슷한 말로, 함수의 반환값이 인자에만 의존하고, 함수의 실행이 외부에 영향을 미치지 않는다는 것을 의미한다.
- **느긋한 계산(lazy evaluation)**: 실제 함수 호출시 그 값이 필요할 때까지 계산하지 않음. 이를 통해 불필요한 계산을 줄일 수 있다.
  - <-> **적극적 계산(eager evaluation)**: 함수 호출전에 모든 인자 목록을 계산해 놓음.

<br>

## 2. 익명함수 vs 클로저

- 클로저(closure)는 람다계산식(lambda Calculus) 구현체
- 이름 없는 함수로 작성가능
- 선언된 범위의 변수를 캡쳐해서 저장하고 (선언적으로) 닫힘
- 함수를 선언할 때의 환경을 기억하는 함수 (캡쳐한 변수를 참조(reference))

함수도 클로저의 일종이다. (이것도 할 말이 많다.)

```js
let closure1 = (n) => {
  return n * n;
};

let closure2 = function (n) {
  return n * n;
};
```

<br>

## 3. 일급함수와 고차함수

js에서 함수는 일급객체이다. 함수를 인자로 받거나, 함수를 반환할 수 있다.

```js
function add(a, b) {
  return a + b;
}

function sub(a, b) {
  return a - b;
}

function mul(a, b) {
  return a * b;
}

function div(a, b) {
  return a / b;
}

function calculate(a, b, func) {
  return func(a, b);
}

console.log(calculate(1, 2, add));
console.log(calculate(1, 2, sub));
console.log(calculate(1, 2, mul));
console.log(calculate(1, 2, div));
```

map, filter, reduce 등의 고차함수를 사용하면 코드를 간결하게 작성할 수 있다.

```js
const arr = [1, 2, 3, 4, 5];

const result = arr
  .map((n) => n * n)
  .filter((n) => n % 2 === 0)
  .reduce((a, b) => a + b);

console.log(result); // 20
```

<br>

```js
let nilNumbers = [1, 2, undefined, 4, 5];
let mapNumbers = nilNumbers.map((value) => [value]);
console.log(mapNumbers); // [ [ 1 ], [ 2 ], [ undefined ], [ 4 ], [ 5 ] ]

// let mapNumberOnly = nilNumbers.map((value) => [value]).flat();
let mapNumberOnly = nilNumbers.flatMap((value) => [value]);
console.log(mapNumberOnly); // [ 1, 2, undefined, 4, 5 ]
```

<br>

## 4. 커링과 함수합성

```js
const plus = (a, b) => a + b;
const multi = (a, b) => a * b;

function calculate(method) {
  return function (a) {
    return function (b) {
      return method(a, b);
    };
  };
}

calculate(plus)(10)(3); // 13

let adder = calculate(plus);
adder(10)(3); // 13

let adder10 = adder(10);
adder10(3); // 13
adder10(5); // 15

let multiply = calculate(multi);
multiply(10)(3); // 30

let doubled = multiply(2);
doubled(10); // 20
doubled(22); // 44
```

<br>

## 함수형 프로그래밍의 4가지 특징 (드림코딩 엘리님 영상 요약)

[영상 출처](https://youtu.be/4ezXhCuT2mw)

<br>

### 1. Pure Functions

함수에서 외부의 상태값을 참조하거나 변경하지 않는다.
동일한 인자를 주면 항상 동일한 결과를 반환한다. 외부의 영향을 받으면 안된다.

❌

```js
let num = 1;

function add(a) {
  return a + num;
}
```

✅

```js
function add(a, b) {
  return a + b;
}
```

<br>

### 2. Stateless, Immutability

함수에 전달된 인자를 변경하지 않는다. 함수 내부에서 인자를 변경하면 안된다.
전달된 인자를 변경하지 않고, 새로운 오브젝트를 만들어서 반환한다.

❌

```js
let person = { name: "John", age: 20 };

function increaseAge(person) {
  person.age++;
  return person;
}
```

✅

```js
let person = { name: "John", age: 20 };

function increaseAge(person) {
  return { ...person, age: person.age + 1 };
}
```

✅

```js
let person = Object.freeze({ name: "John", age: 20 });

function increaseAge(person) {
  return Object.freeze({ ...person, age: person.age + 1 });
}
```

위 두가지 원칙을 지키면서 함수를 구현하면, 함수는 참조 투명성을 지키게 된다. 즉, 부작용(side effect)가 없는 함수가 된다.
멀티쓰레딩 환경에서도 안정적으로 동작할 수 있다.

<br>

### 3. Expression Only

if나 switch, for과 같은 여러 문장을 사용하는 것을 지양한다. 함수는 단일 표현식으로 구현한다.

❌

```js
let numbers = [1, 2, 3];

function multiply(numbers, multiplier) {
  let result = [];
  for (let i = 0; i < numbers.length; i++) {
    result.push(numbers[i] * multiplier);
  }
  return result;
}
```

✅

```js
let numbers = [1, 2, 3];

function multiply(numbers, multiplier) {
  return numbers.map((n) => n * multiplier);
}
```

<br>

### 4. First Class and Higher Order Functions

마지막으로 함수는

1. 다른 데이터와 마찬가지로 변수에 할당하거나, 인자로 전달하거나, 반환값으로 사용할 수 있다. (first class)
2. 함수 자체를 인자로 전달하거나, 함수를 반환하는 함수를 만들 수 있다. (higher order)

**first class**

```js
const addTwo = (a) => a + 2;
const multiplyTwo = (a) => a * 2;
const transform = (numbers) => numbers.map(addTwo).map(multiplyTwo);
console.log(transform([1, 2, 3, 4])); // [6, 8, 10, 12]
```

**higher order**

```js
const addToppings = (topping) => (food) => food + topping;
const egg = addToppings("🍳");
const bacon = addToppings("🥓");

console.log(egg("🥞")); // 🥞🍳
console.log(bacon("🥞")); // 🥞🥓
```

## 정리

함수형 프로그래밍에 대해 정리해보았다. 쉽지 않은 내용이다. 사실 우리는 함수형 프로그래밍을 생각보다 많이 접하고 사용해왔을지 모른다. 위에서 언급한 `map`, `filter`, `reduce`와 같은 고차함수들도 함수형 프로그래밍 패러다임을 지키면서 설계된 함수이다. (일급 함수 (first-class functions)와 고차 함수 (higher-order functions)의 개념을 기반으로 만들어 졌다.)

당연히 함수형에 대한 내용이 오늘 정리한 것보다 더 많이 있고, 그 활용방안 또한 내가 상상할 수 없을 정도로 넓다는 것을 알고있다. 개발의 세계란 너무나 방대하다... ㅎㅎ
