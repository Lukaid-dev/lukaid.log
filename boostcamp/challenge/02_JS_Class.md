# JavaScript의 클래스

js의 클래스는 참 낯설었다. 일단 **js에는 클래스가 존재하지 않는다. (않았다)** 그래서 클래스를 만들기 위해서는 함수를 사용해야 한다. 그리고 함수를 사용해서 클래스를 만들면 프로토타입이라는 개념이 등장한다. (사실 이것에 대한 정리를 하려고해도 한참이긴 한데, you don't know js를 읽어보면서 정리를 해봐야겠다.)

프로토타입은 클래스의 인스턴스가 공유하는 속성과 메서드를 정의한다. 이렇게 클래스를 만들고 인스턴스를 생성하는 방법은 다음과 같다.

<br>

```js
// prototype pattern

const Health = function (name, healthTime) {
  this.name = name;
  this.healthTime = healthTime;
};

Health.prototype.showHealth = function () {
  console.log(
    this.name + "님, 오늘은 " + this.healthTime + "에 운동을 하셨네요"
  );
};

const lukaid = new Health("lukaid", "12:12");
lukaid.showHealth(); // lukaid님, 오늘은 12:12에 운동을 하셨네요
```

<br>

와 진짜 낯설다. prototype이라는 개념을 들어보기는 했는데, 이렇게 사용하는 방법은 처음이다. 그리고 이렇게 클래스를 만들고 인스턴스를 생성하는 방법은 ES6(2015)에서 도입된 클래스 문법을 사용하면 훨씬 간단하게 구현할 수 있다.

```js
// Classes pattern - ES2015

class Health {
  constructor(name, healthTime) {
    this.name = name;
    this.healthTime = healthTime;
  }

  showHealth() {
    console.log(
      this.name + "님, 오늘은 " + this.healthTime + "에 운동을 하셨네요"
    );
  }
}

const lukaid = new Health("lukaid", "12:12");
lukaid.showHealth(); // lukaid님, 오늘은 12:12에 운동을 하셨네요
```

이렇게 클래스를 만들고 인스턴스를 생성하는 방법은 다른 언어에서 클래스를 만들고 인스턴스를 생성하는 방법과 매우 유사하다. 그래서 js에서도 클래스를 만들고 인스턴스를 생성하는 방법이 이렇게 도입된 것 같다. 그리고 이렇게 클래스를 만들고 인스턴스를 생성하는 방법은 ES6에서 도입된 클래스 문법을 사용하면 훨씬 간단하게 구현할 수 있다. 하지만 위의 방법을 이용하는 것이 Classes pattern보다는 메모리 효율 성에서 매우 유리하다고 한다.

<br>

그 이유는 다음과 같다. 생성자를 통해 생성된 객체(인스턴스)들이 여러개 있어도, prototype에 연결된 객체들(movieObject)는 동일한 메모리 공간에서 효율적으로 재사용 된다. 즉, 두 객체의 prototype은 같다.

prototype pattern을 알고 이를 활용하여 개발을 해보았는데, 생각보다 더 직관적이고, 효율적으로 코딩을 할 수 있는 것 같았다. 아직 어떤 방법이 더 옳은 방법인지 체감하지 못해서 당분간 두 방법을 번갈아 가며 써 볼 계획이다.
