# 크리티컬 랜더링 패스(Critical Rendering Path)

[MDN-Critical Rendering Path](https://developer.mozilla.org/en-US/docs/Web/Performance/Critical_rendering_path)

크리티컬 랜더링 패스(Critical Rendering Path)은 React와 관계없이 웹 클라이언트 개발자라면 반드시 알아야하는 개념이다.

<br>
<br>

## 웹페이지 빌드과정

크리티컬 랜더링 패스(Critical Rendering Path)란 브라우저가 HTML, CSS 및 JavaScript를 화면에 픽셀로 변환하는 일련의 단계를 의미한다. 이 패스는 문서 객체 모델(DOM), CSS 객체 모델(CSSOM), 렌더 트리 및 레이아웃을 포함한다.

브라우저가 서버로부터 HTML 응답을 받아 화면을 그리기 위해 다음의 6단계를 실행한다.

1. DOM 트리 만들기
2. CSSOM 트리 만들기
3. JavaScript 실행
4. Render 트리 만들기
5. 레이아웃 생성하기
6. 페인팅

<br>
<br>

### 1, 2. DOM 트리 및 CSSOM 트리 만들기

DOM과 CSSOM은 다음과 같다.

- 문서 객체 모델(DOM): HTML 파싱 중에 생성되며 HTML은 JavaScript를 요청할 수 있으며 JavaScript가 DOM을 수정할 수도 있다.
- CSS 객체 모델(CSSOM): HTML은 스타일을 요청하거나 포함하며 CSS 객체 모델을 작성한다.

서버에서 응답받은 HTML과 CSS를 각각 읽고 이를 Document Object로 변환하는 과정이다.

### 3. JavaScript 실행

JavaScript는 DOM을 수정할 수 있으므로 DOM 트리와 CSSOM 트리가 완성되기 전에 JavaScript를 실행하면 안된다. 따라서 JavaScript는 DOM 트리와 CSSOM 트리가 완성된 후에 실행된다. JavaScript가 실행되면 DOM 트리와 CSSOM 트리는 변경될 수 있다.

### 4. Render 트리 만들기

가장 중요한 단계인데, 이 단계는 브라우저가 DOM과 CSSOM을 결합하는 곳이며, 이 프로세스는 화면에 보이는 모든 콘텐츠와 스타일 정보를 모두 포함하는 최종 렌더링 트리를 출력한다. 즉 화면에 표시되는 모든 노드의 콘텐츠 및 스타일 정보를 포함한다.

### 5. 레이아웃 생성하기

브라우저가 페이지에 표시되는 각 요소의 크기와 위치를 계산하는 단계.

### 6. 페인팅

레이아웃 단계에서 계산된 정보를 사용하여 화면에 픽셀을 그리는 단계.

<br>
<br>

이러한 과정을 통해 브라우저는 60fps를 유지하면서 화면을 그리도록 하며, 사용자와의 상호작용을 보장한다.
