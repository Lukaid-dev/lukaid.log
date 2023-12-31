# HTML 시멘틱 태그

어떻게든 HTML과 CSS를 피해왔는데, 이제는 어쩔 수 없이 공부해야 할 때가 왔다. HTML에서 가장 모르고 있는것이 각종 태그들의 종류와 기능이다.

웬만한건 다 div에 때려박고 개발했는데, 시멘틱 태그(semantic tag)를 사용하면 더 좋은 웹을 만들 수 있다고 한다.

<br>

## 시멘틱 태그

시멘틱 태그는 뭐 별고 없고, 척 보고 이게 뭔지 알수있는 태그인 것 같다.

div와 같은 태그들은 그냥 레이아웃을 위한 태그라고 한다. 얘는 그냥 봐서는 뭐 하는 애인지 잘 모른다.

근데 header와 같은 시멘틱 태그를 사용하면 단번에 "아 이건 헤더구나. 문서의 맨 위에 있는 내용이구나" 라고 알 수 있다.

장점을 정리해보자.

<br>

### 1. SEO 최적화 강화

시멘틱 태그를 사용하면 검색 엔진 최적화 (SEO)에 유리하다. 로봇이 슥 보는데, 척 보고 알수있는게 있으면 당연히 그럴 것 같다. 검색 엔진이 태그의 목적에 부합하는 구조로 웹사이트가 설계되었을 때, 검색 결과에서 더욱 빠르고 효율적으로 정보를 인식할 수 있다고 한다.

### 2. 웹 접근성 향상

시멘틱 태그는 일반적인 브라우저에서는 차이를 느끼기 어렵지만, 스크린 리더 등의 보조 기기를 사용하는 환경에서 웹 접근성과 사용성을 향상시킨다고 한다. 시각장애인을 위한 웹 서핑 프로그램 같은 환경에서 유용할 것 같다.

### 3. 유지보수의 편의성 제고

사실 개발자 입장에서 가장 중요한 포인트는 이것 같다. 시멘틱 태그를 사용하면 많은 div 요소를 사용하는 것보다 관리가 용이 해진다. 태그의 이름만으로도 어떤 영역을 나타내는지 바로 확인할 수 있고, 각 태그 영역의 특성에 맞는 작업을 구분하여 진행하기 쉬워진다.

<br>

## 시멘틱 태그의 종류

- `<header>`: 머리글, 제목, 헤더를 나타냄.
- `<nav>`: 네비게이션, 목차, 다른 페이지로의 링크를 위한 공간을 나타냄.
- `<aside>`: 좌측과 우측 사이드 위치의 부수적인 내용을 나타냄.
- `<section>`: 주제나 카테고리 별로 섹션을 구분하는 용도로 사용되며, 같은 테마를 가진 여러 콘텐츠를 그룹화한다.
- `<article>`: 기사나 블로그 등 텍스트 위주의 페이지를 구성할 때 사용됨.
- `<footer>`: 바닥글이나 문서 하단의 정보 구분 공간을 나타냄.
- `<address>`: 콘텐츠 작성자나 사이트 소유자의 정보를 부가적으로 가지고 있음.
- `<hgroup>`: 제목과 관련된 부제목을 묶는 역할을 한다.
- `<main>`: 문서 `<body>`의 중심 주제나 주요 내용을 나타내며, 응용 프로그램의 중심 기능과 관련된 콘텐츠를 표현한다.
- `<details>`: 주변 문맥에서 표시된 구절의 관련성이나 중요성을 나타내기 위해 사용되며, 강조나 참조 목적으로 텍스트를 감싸거나 표기한다.
- `<figure>`: 이미지, 다이어그램, 사진 등의 독립적인 콘텐츠를 정의할 때 사용된다.
- `<figcaption>`: `<figure>` 요소의 설명 캡션을 정의한다.
- `<mark>`: 현재 맥락에 중요한 부분을 강조함.
- `<time>`: 특정한 시간 지점이나 구간을 나타내며, datetime과 같은 속성을 이용해 기능을 구현할 수 있다.
- `<summary>`: `<details>` 요소에 대한 요약이나 캡션을 지정하며, 클릭 시 하위 details 요소가 열리거나 닫힌다.

<br>

앞으로 프론트 개발을 할 때, div를 남발하기 보다는, 적절한 시멘틱 태그를 사용하면 좋을 것 같다.
