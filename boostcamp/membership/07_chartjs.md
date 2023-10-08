# Chart.js

달력은 라이브러리를 안쓰고 어떻게든 구현했는데, 차트는 여러 시도를 해보다가 도저히 못할 것 같아서 `chart.js`를 사용하기로 했다.

<br>

[chart.js 공식문서](https://www.chartjs.org/docs/latest/getting-started/)

<br>
<br>

## 브라우저에서 데이터 시각화하기

브라우저에서 데이터를 시각화하는 방법에는 크게 두가지가 있다. **SVG**와 **Canvas**가 그것이다.

<br>

### SVG (Scalable Vector Graphics)

**1. 벡터 그래픽**: SVG는 벡터 기반 그래픽 형식으로, 그래픽 요소를 수학적인 방정식으로 표현한다. 이미 많이 활용해 보았는데, `.svg` 확장자를 가진 파일들이 바로 SVG 파일이다. jpg나 png와 같은 래스터 이미지와는 달리 SVG는 확대하거나 축소해도 그때 그때 고품질 렌더링을 제공해서 이미지가 깨지지 않고, 해상도와 상관없이 부드러운 그래픽을 제공한다.

**2. XML 기반**: SVG 파일은 XML 형식으로 작성되며, 텍스트 에디터로 편집할 수 있다. 이를 통해 검색엔진 최적화에도 활용할 수 있다고 한다.

<br>

**3. DOM(Document Object Model) 요소**: SVG 그래픽은 웹 페이지의 DOM 요소로 삽입되며, JavaScript를 사용하여 동적으로 조작할 수 있다.

<br>

예시:

```xml
<svg width="14" height="14" viewBox="0 0 14 14" fill="none" xmlns="http://www.w3.org/2000/svg">
<rect x="0.333313" y="0.333344" width="13.3333" height="13.3333" rx="6.66667" fill="black"/>
<path d="M10.3333 5L5.74996 9.44444L3.66663 7.42424" stroke="white" stroke-linecap="round" stroke-linejoin="round"/>
</svg>
```

<br>

### Canvas

**1. 비트맵 그래픽**: Canvas는 SVG와 달리, 비트맵 그래픽을 생성하며, 픽셀 단위로 그림을 그린다. 그래서 그래픽의 크기를 조절하면 이미지 품질이 손실될 수 있다.

**2. JavaScript를 이용한 그리기**: Canvas는 JavaScript로 그래픽을 그리는 API를 제공하며, 픽셀 단위로 직접 조작할 수 있다. 그림을 수정하기 위해서는 전체 그림을 다시 그려야한다.

**3. 애니메이션 및 상호 작용**: Canvas를 사용하면 JavaScript를 통해 애니메이션 및 상호 작용을 구현할 수 있다. 그래프 위에 마우스를 올려놓으면 툴팁이 나오는 것도 이런 방식으로 구현할 수 있다.

<br>

### 정리

- `SVG`는 `HTML`의 일부이기 때문에 `DOM`을 사용할 수 있고, `CSS`를 사용할 수 있다.
- `Canvas`는 `HTML`의 일부가 아니기 때문에 `DOM`을 사용할 수 없고, `CSS`를 사용할 수 없다.
- `Canvas`는 `JavaScript`를 사용해서 그림을 그리고, `SVG`는 `XML`을 사용해서 그림을 그린다.
- `SVG`는 `HTML`과 같은 `DOM`을 사용하기 때문에 `JavaScript`로 `DOM`을 조작하는 것처럼 `SVG`를 조작할 수 있다.
- `Canvas`는 `SVG`보다 더 빠르고, 복잡하며, 많은 기능을 제공하고, 많은 코드가 필요하다.

<br>
<br>

## Chart.js

Chart.js는 Canvas를 사용해서 데이터를 시각화하는 라이브러리이다. Chart.js는 Chart라는 클래스를 제공하며, Chart 클래스의 인스턴스를 생성하면 그래프를 그릴 수 있다. Canvas 요소를 활용하여 비트맵 그래픽으로 렌더링되며, 셀 단위로 그려지며, 동적인 상호 작용 및 애니메이션을 위한 JavaScript 코드를 사용하여 조작될 수 있다.

<br>

```html
<div>
  <canvas id="myChart"></canvas>
</div>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<script>
  const ctx = document.getElementById("myChart");

  new Chart(ctx, {
    type: "bar",
    data: {
      labels: ["Red", "Blue", "Yellow", "Green", "Purple", "Orange"],
      datasets: [
        {
          label: "# of Votes",
          data: [12, 19, 3, 5, 2, 3],
          borderWidth: 1,
        },
      ],
    },
    options: {
      scales: {
        y: {
          beginAtZero: true,
        },
      },
    },
  });
</script>
```

<br>

공식문서에서 제공하는 위의 예시 코드를 보면, 매우 간단하게 차트를 생성 할 수 있는 것을 알 수 있다.

우선 html에는 canvas tag만을 작성하고, 해당 태그를 Chart 클래스의 인스턴스를 생성할 때, 첫번째 인자로는 canvas 요소를 전달한다.

두번째 인자로는 차트를 그리기 위한 정보를 전달한다.

type은 차트의 타입을 지정하며, `bar`, `line`, `pie`, `doughnut` 등이 있다.

data는 `labels`와 `datasets`로 구성되며, 한다. 데이터는 `labels`와 `datasets`로 구성된다. `labels`는 차트의 x축에 표시될 라벨들을 전달하고, `datasets`는 차트의 데이터를 전달한다. 배열의 각 요소는 차트의 데이터를 전달한다. `datasets`의 각 요소는 `label`, `data`, `borderWidth` 등의 속성을 가진다. `label`은 차트의 범례에 표시될 라벨을 전달하고, `data`는 차트의 데이터를 전달한다. `borderWidth`는 차트의 테두리 두께를 전달한다.

마지막으로 options은 `scales`와 `y`로 구성된다. `scales`는 차트의 축에 대한 옵션을 전달하고, `y`는 y축에 대한 옵션을 전달한다. `beginAtZero`는 y축의 최소값을 0으로 설정한다.

<br>

도넛 차트의 예시는 다음과 같다.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Doughnut 차트 예제</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  </head>
  <body>
    <canvas id="myDoughnutChart" width="400" height="400"></canvas>

    <script>
      var data = {
        labels: ["Red", "Blue", "Yellow", "Green"],
        datasets: [
          {
            data: [30, 20, 15, 35],
            backgroundColor: ["red", "blue", "yellow", "green"],
          },
        ],
      };

      var ctx = document.getElementById("myDoughnutChart").getContext("2d");
      var myDoughnutChart = new Chart(ctx, {
        type: "doughnut",
        data: data,
        options: {
          // 추가 옵션 설정 가능
        },
      });
    </script>
  </body>
</html>
```
