# 브라우저 렌더링 과정을 설명해주세요. (브라우저는 어떻게 동작 하나요?)

> 사용자는 로드가 빠르고 상호작용이 원활한 컨텐츠로 이루어진 웹 경험을 원한다. 이 두 가지 목표를 달성하기 위해 브라우저가 어떻게 동작하는지 이해하는 것이 중요하다.

> 대부분 브라우저는 싱글 쓰레드이다. 메인 쓰레드가 요청된 모든 작업을 수행하면서도 사용자와의 상호작용에 반응할 수 있도록 보장하기 위해서는 렌더링 시간이 가장 중요하다. 브라우저가 싱글 쓰레드로 동작한다는 점을 이해하고 가능한 메인 쓰레드의 책임을 줄여주는 방식으로 웹 성능 향상을 이룰 수 있다.

브라우저의 렌더링 과정을 이해하기 위해서는 Critical Rendering Path를 알아야 하는데, CRP는 브라우저가 하나의 화면을 그려내는 과정 또는 순서를 말한다.

웹페이지에서 HTML을 요청하면, 서버는 HTML response headers와 data를 반환한다. 그러면 브라우저는 HTML 구문 분석을 시작하여 수신된 바이트를 DOM 트리로 변환한다. 브라우저는 스타일시트, 스크립트 또는 임베디드 이미지 참조 등 외부 리소스에 대한 링크를 찾을 때마다 요청을 시작한다. (requests every time it finds) 이때 일부 요청들은 차단(blocking)될 수 있는데, asset 들기 위해 처리될 때까지 나머지 HTML의 구문 분석이 중단된다. DOM을 구축이 끝나면, CSS Object Model을 빌드한다. DOM과 CSSOM이 완성되면 브라우저는 렌더 트리(builds the render tree)를 빌드하여 표시되는 모든 콘텐츠의 스타일을 계산한다. 렌더 트리가 완성되면, 모든 렌더 트리 요소의 위치와 크기를 정의하는 레이아웃과정이 실행된다(layout occurs). 레이아웃이 완료되면 페이지가 렌더링되거나 혹은 화면에 '페인팅'된다.

이 과정을 간략히 하면 다음과 같다.

**1. HTML parsing** - 웹 브라우저는 HTML 파일을 다운로드하고 이를 파싱하여 페이지의 구조를 파악하여 DOM(Document Object Model)트리를 만든다.

**2. CSS parsing** - 웹 브라우저는 CSS 파일을 다운로드하고 이를 파싱하여 HTML 요소의 스타일을 지정하는 방법을 파악하여 CSSOM(CSS Object Model) 트리를 만든다.

**3. JavaScript parsing** - 웹 브라우저는 자바스크립트 파일을 다운로드하고 이를 파싱하여 페이지에 인터랙션을 추가하는 방법을 파악한다.

**4. Layout** - 웹 브라우저는 CSS 스타일링 및 문서 흐름에 따라 페이지에서 각 HTML 요소의 크기와 위치를 결정한다.

**5. Paint** - 웹 브라우저는 레이아웃 및 스타일링 정보에 따라 화면의 픽셀을 최종적으로 그린다.

!https://media.geeksforgeeks.org/wp-content/uploads/20210728231006/111.png

출처:

https://developer.mozilla.org/en-US/docs/Web/Performance/Critical_rendering_path

https://developer.mozilla.org/ko/docs/Web/Performance/How_browsers_work

https://www.geeksforgeeks.org/critical-rendering-path-flow/

https://wonism.github.io/critical-rendering-path/
