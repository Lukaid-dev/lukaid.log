# React Router Dom

주중에 4일이나 예비군을 가서 일정을 소화하지 못하는건 진짜 아닌거 같아서 2일은 주말 예비군을 다녀왔다... 진짜 너무 가기싫지만 어쩌겠는가... 장소도 너무 멀어서 체력소모가 상당했다.. 아직 이틀 남았는데 월요일 화요일 다녀오고 치워버려야지.. 아무튼 지난주에 공부한 React Router Dom에 대해서 정리해보자

## 1. React Router Dom?

react는 spa이기 때문에 하나의 index.html을 기반으로 동작한다. 그렇기 때문에 다른 페이지로 이동할 때마다 새로운 html을 받아오는 것이 아니라, js를 통해 페이지를 업데이트한다. 이때, react-router-dom을 사용하면 쉽게 페이지를 이동할 수 있다. react-router-dom을 사용하면 동적 라우팅을 쉽게 구현할 수 있는데, 동적 라우팅이란 사용자의 요청에 따라 다른 결과를 보여주는 것을 말한다. 예를 들어, 사용자가 /home에 접속하면 home 컴포넌트를 보여주고, /about에 접속하면 about 컴포넌트를 보여주는 것이다.

## 2. React Router Dom 사용법

```bash
npm install react-router-dom --save
```

설치 후 먼저 우리의 리액트 앱에 어디서나 react router를 사용할 수 있게 해주자.

```jsx
// main.jsx
import App from "./App";
import { BrowserRouter } from "react-router-dom";

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById("root")
);
```

```jsx
// App.jsx
import { Route, Routes } from "react-router-dom";

function App() {
  return (
    <div className="App">
      <Routes>
        <Route path="/" component={<Home />} />
        <Route path="/about" component={<About />} />
      </Routes>
    </div>
  );
}
```

````jsx
// About.jsx
import { Link } from "react-router-dom";

function About() {
  return (
    <div>
      <h1>About</h1>
      <Link to="/">HomePage로 가기</Link>
      <Link to="detail">DetailPage로 가기</Link>
    </div>
  );
}
```jsx

````

### BrowserRouter

HTML5 History API(pushState, replaceState 및 popState 이벤트)를 사용하여 UI를 URL과 동기화된 상태로 만들어준다. 다시말해, HTML5의 History API를 사용하여 페이지를 새로고침하지 않고도 주소를 변경할 수 있도록 해준다.

### Routes

Routes는 앱에서 생성될 모든 개별 경로(`Route`)에 대한 컨테이너 역할을 함. Route로 생성된 자식 컴포넌트 중에서 매칭되는 첫번째 Route를 렌더링 한다.

### Route

Route는 path와 component를 props로 받아서 단일 경로를 만든다.

- path는 원하는 컴포넌트의 URL 경로를 지정한다.
- component는 해당 경로로 이동했을 때 렌더링할 컴포넌트를 지정한다.

### Link

Link는 클릭하면 다른 경로로 이동시켜주는 컴포넌트이다. a 태그와 비슷하지만, a 태그는 페이지를 새로고침하면서 이동하지만 Link는 페이지를 새로고침하지 않고 이동한다.

## 3. Nested Routing

Nested Routing 또한 이미 익숙한 개념이다. 라우팅 구조에서 부모 라우트가 하나 이상의 자식 라우트를 가질때 이를 여러개의 라우트로 하나하나 선언하는 것이 아니라 부모 라우트에 자식 라우트를 선언하는 방식이다.

```jsx
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/about" element={<About />}>
    <Route path="detail" element={<Detail />}>
      <Route path=":id" element={<DetailId />} />
      <Route path="history" element={<DetailHistory>} />
    </Route>
  </Route>
</Routes>
```

이렇게 선언하면 복잡한 레이아웃 코드를 작성 할 필요가 없다. 대부분의 레이아웃은 URL의 세그먼트에 연결된다.
