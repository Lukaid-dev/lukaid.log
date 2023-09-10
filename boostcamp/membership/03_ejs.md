# ejs (embedded javascript)

## ejs??

이번 과제에서 express를 사용하고, ejs라는 템플릿 엔진을 사용할 것이 요구사항으로 나왔다. 사실 처음에는 당황했다. 템플릿 엔진이라면 django의 그것이나 python진영의 jinja를 잠깐 써본것이 전부였는데, 사실 그렇게 좋은 경험은 아니었다. 나의 지금가지의 웹/앱 개발 경험은 전부 CSR이었고(react, flutter용 api server 제작), 거의 전부 json형태의 데이터를 주고받는 api 서버 였다.

ejs는 javascript 진영의 템플릿 엔진이라고 한다. 이걸 왜 사용하라고 했을까 고민해봤는데, 두가지 생각이 들었는다.

첫째로, html과 css에 집중하되 서버가 있어야 한다. 첫번째주의 주요 학습 포인트는 html과 css라고 생각하다. 나는 그동안 html과 css를 제대로 써본 경험이 거의 없다. 근데 이런 상황에서 프론트의 html, css, js다 신경쓰고, express도 따로 띄워서 api서버를 만들기에는 조금 무리가 있을수도 있겠다 싶은 생각이 들었다.

두번째는, 레거시에 대한 경험이다. 요즘은 뭐 next다 뭐다 해서 다시 SSR이 대세가 되는 듯 하지만, 내가 경험했던 것 처럼 CSR이 대세다. 그래서 이번 기회에 SSR도 한번 경험해보고 (물론 ejs가 제대로 된 SSR인지는 모르겠다) 예전의 웹 개발은 어떻게 되는지 그 역사를 경험해 볼 수 있는 기회가 될 것 같다고 생각도 했다.

과제의 초반에는 사실 ejs를 제대로 학습하지 않았다. 이번주만 잠깐 쓸거라고 생각하기도 했고, 솔직히 오래된 기술에 시간을 쓰고 싶지 않았다. 하지만 위의 생각들이 들고나니, 조금은 제대로 공부해야겠다는 생각이 들어서 공부하며 정리해보려 한다.

<br>
<hr>
<br>

## ejs!

그래서 ejs는 무엇일까. [공식문서](https://ejs.co/)에는 아래와 같이 쓰여있다.

> What is the "E" for? "Embedded?" Could be. How about "Effective," "Elegant," or just "Easy"? EJS is a simple templating language that lets you generate HTML markup with plain JavaScript.

plain js로 html을 만들 수 있는 간단한 템플릿 언어라고 한다. ejs확장자 파일에서 html과 유사한 문법을 사용하여 html을 만들 수 있다. 이렇게 하면서 얻을 수 있는 공식문서 상 장점은 다음과 같다.

- plain js를 사용
- 빠른 개발시간
- 간단한 문법
- 빠른 실행
- 쉬운 디버깅
- 활발한 커뮤니티 (진짜?)

뭐... 보통 어느 공식문서에나 써있을 법한 말들이 써있긴하다. 그래도 plain js를 사용한다는 것은 꽤 큰 장점이라고 생각한다. 다른 템플릿 엔진들은 템플릿 언어를 따로 배워야 하지만, ejs는 그럴 필요가 없다. 어차피 써야하는 html과 js만 쓰면 되니까.

다음은 가장 중요한 ejs에서 사용되는 태그들에 대해 공부해보자.

<br>
<hr>
<br>

## ejs 태그

ejs에서 사용되는 태그는 다음과 같다. 공식문서를 참조하였고, 간단한 예시를 추가했다. 참고로 태그 이름은 내가 임의로 지은거다. 공식문서에는 없다.

### 1. 스크립트 태그(`<% %>`)

> 'Scriptlet' tag, for control-flow, no output

가장 기본적인 태그이며 JavaScript 코드 블록을 실행하는 데 사용된다. 템플릿 내에서 조건문, 반복문, 함수 호출 등의 JavaScript 코드를 실행할 수 있다!

```ejs
<% if (isAdmin) { %>
  <p>Welcome, administrator!</p>
<% } %>

<ul>
  <% for (let item of items) { %>
    <li><%= item %></li>
  <% } %>
</ul>

<% if (loggedIn) { %>
  <p>Welcome, <%= username %>!</p>
<% } else { %>
  <p>Please log in to continue.</p>
<% } %>
```

`<% var x = 10; %>`와 같이 템플릿 내에서 JavaScript 변수를 선언할 수도 있다.

### 2. 출력 태그(`<%= %>`)

> Outputs the value into the template (HTML escaped)

변수나 JavaScript 표현식을 템플릿에서 출력하는 데 사용된다. 예를 들어, `<%= name %>`는 name 변수의 값을 템플릿에 출력한다. 이때, HTML이 이스케이프되어 출력된다.

html이 이스케이프 된다는 건, html의 특수문자들이 html entity로 변환되어 출력된다는 것이다. 예를 들어, `<`는 `&lt;`로, `>`는 `&gt;`로 변환된다. 이를 통해, 사용자가 입력한 값을 그대로 출력하더라도, html이 실행되지 않고, 그대로 출력되는 것을 방지할 수 있다. (보안 취약점인 크로스 사이트 스크립팅(XSS) 공격을 방지할 수 있다.)

```ejs
<p>Welcome, <%= name %>!</p>
```

### 3. 인클루드 태그(`<%-  %>`)

> Outputs the unescaped value into the template

프로젝트 내에 존재하는 다른 ejs파일을 불러올 수 있다. 이때, html이 이스케이프되지 않고 그대로 출력된다.

```ejs
<%- include('header') -%>
```

### 4. HTML escape and unescape 태그

앞서 말했듯, `<%=`는 html을 escape하고 `<%-`는 html을 unescape한다(escape하지 않는다). 다음 예시의 출력은 어떻게 될까?

```ejs
<p>Escaped HTML: <%= "<h3>Escaped HTML</h3>" %></p>
<p>Unescaped HTML: <%- "<h3>Unescaped HTML</h3>" %></p>
```

<img width="319" alt="image" src="https://user-images.githubusercontent.com/97015501/265066196-e0648d4d-2b59-48ac-8ea2-b738b24425a9.png">

`<%=`는 html이 이스케이프되어서 `<h3>`태그를 단순한 문자열로 출력하였고,

`<%-`는 html이 이스케이프되지 않아서 `<h3>`태그가 그대로 출력되었다.

이러한 특성을 이용하여, 다른 ejs파일을 불러올 때, `<%- include('header') -%>`와 같이 사용하면, header.ejs파일의 내용이 그대로 출력된다.

<br>
<br>

이정도만 알면 프로젝트를 할때 EJS를 사용할 수 있을 것 같다. 그동안 좀 막무가내로 사용했는데, 주말간 리팩토링 하면서 EJS를 제대로 사용해보고 싶다.
