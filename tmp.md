이미 선언된 JavaScript의 객체는 프로토타입을 활용하여 그 메서드와 속성을 정의하거나 수정할 수 없다.

프로토타입 객체는 JavaScript에서 객체 간 상속을 구현하는 메커니즘으로, 모든 JavaScript 객체는 프로토타입 객체를 가지며, 이를 통해 다른 객체의 속성과 메소드를 상속받을 수 있다.

객체를 사용할 때, 해당 객체에 정의되지 않은 속성이나 메소드를 호출하면 JavaScript는 프로토타입 체인을 따라 상위 프로토타입을 검색하며, 이를 통해 상속된 속성과 메소드를 공유하고 재사용할 수 있다.

JavaScript에서는 프로토타입 체이닝을 활용하여 객체 간의 계층 구조를 형성하고, 코드의 재사용성과 유지보수성을 높일 수 있다.

정답 1번

```js
function Test(name) {
  this.name = name;
}

Test.prototype.sayHello = function () {
  console.log("Hello, " + this.name + "!");
};

const test = new Test("성우");
test.sayHello(); // "Hello, 성우!" 출력

Test.prototype.sayHello = function () {
  console.log("안녕, " + this.name + "!");
};

test.sayHello(); // "안녕, Alice!" 출력
```
