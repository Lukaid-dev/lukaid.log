# CSS

그동안 JS보다 더 피한게 하나 있다. 그건 바로 CSS! 나는 CSS가 정말 싫었다. 뭘 어쩌라는건지, 내가 원하는대로 안되고... 디버깅도 쉽지않고, 공부도 어려웠다. (사실 핑계지 뭐...) 아무튼 그래서 나는 프론트랑 정말 안맞는다고 생각했고, 프론트 공부를 기피했다.

그러나 전회사에서 플러터를 공부하게 되고, css랑은 다르지만 화면을 그리는 법을 배우고, 이번에 다시 CSS를 접하니 감회가 새롭다. 나는 앞으로 프론트 개발자가 된다는 마인드로 열심히 공부해 보았다. _(아래의 내용은 패스트캠퍼스의 HTML/CSS 강의를 듣고 정리한 내용이다.)_

우선 제일 크게 html은 글자와 상자로 이루어져 있다! 이것만 잘 이해해도... 아니 이것만으론 부족하다.

그리고 화면을 이루는 상자들이나 글자들을 요소라고 부른다. 요소가 화면에 출력되는 특성, 크게 2가지로 구분되는데, 인라인과 블록이 그것이다.

- **인라인(Inline) 요소** : 글자를 만들기 위한 요소들.
- **블록(Block) 요소** : 상자(레이아웃)를 만들기 위한 요소들.

### 인라인 (inline)

- ex) a, br, button, img, input, span 등
- 왼쪽에서 오른쪽으로 (요소가 수평으로 쌓임)
- 가로 세로 모두, 포함한 콘텐츠 크기만큼 자동으로 **줄어듬**!
- 가로 세로 사이즈를 지정할 수 없음
- 마진(외부 여백) 그리고 패딩(내부 여백)의 좌우는 적용되지만 위아래는 적용 안됨
- 자식요소로 블록을 사용 할 수 없다.

### 블록 (block)

- ex) div, h1, header, hr, p, ul 등
- 위에서 아래로 쌓인다. (요소가 수직으로 쌓임)
- 가로 너비는, 부모 요소의 크기만큼 자동으로 **늘어남!**
- 세로 너비는, 포함한 콘텐츠 크기만큼 자동으로 **줄어듬**!
- 가로 세로 사이즈 지정할 수 있음
- 마진 패딩 모두 적용 가능
- 자식요소로 블록, 인라인 모두 가능

### 인라인-블록 (inline-block)

- ex) input
- 인라인 요소와 블록 요소의 중간 형태
- 베이스는 인라인, 근데 가로세로를 지정할 수 있다고 생각하며 됨
- 왼쪽에서 오른쪽으로 (요소가 수평으로 쌓임)
- 가로 세로 모두, 포함한 콘텐츠 크기만큼 자동으로 **줄어듬**!
- 가로 세로 사이즈 지정할 수 있음
- 마진(외부 여백) 그리고 패딩(내부 여백)의 좌우, 위아래 모두 적용 가능
- 자식요소로 블록, 인라인 모두 가능

> 인라인 요소와 블록 요소와의 주요 차이점은 인라인-블록은 요소 뒤에 줄 바꿈을 추가하지 않아서 다른 요소 옆에 위치할 수 있다는 것입니다. 따라서 인라인 요소처럼 인라인-블록은 가로 및 세로 크기를 설정할 수 있습니다. 인라인 요소는 가로 및 세로 크기를 설정할 수 없습니다

<br>
<hr>
<br>

## 선택자 (selector)

CSS의 선택자란, HTML 요소를 선택하는 방법을 말한다. 선택자를 통해 선택된 요소에 CSS 속성을 적용할 수 있다. 종류는 다음과 같이 크게 다섯 종류가 있다.

- 기본
- 복합
- 가상 클래스
- 가상 요소
- 속성

### 기본

|              선택자              |                               설명                               | 예시  |
| :------------------------------: | :--------------------------------------------------------------: | :---: |
| 전체 선택자 (Universal Selector) |                         모든 요소를 선택                         |  \*   |
|   태그 선택자 (Type Selector)    |                   태그 이름이 ABC인 요소 선택                    |  div  |
|  클래스 선택자 (Class Selector)  |              HTML class 속성의 값이 ABC인 요소 선택              | .test |
|   아이디 선택자 (ID Selector)    | HTML id 속성의 값이 ABC인 요소 선택. 기본적으로 하나만 존재 가능 | #test |

### 복합

1. 일치 선택자 (Basic Combinator) : 선택자 ABC와 XYZ를 동시에 만족하는 요소 선택.

   ```html
   <!-- 붙여서 쓰는게 포인트 -->
   <style>
     span.test {
       color: red;
     }
   </style>

   <span class="test">test</span>
   ```

2. 자식 선택자 (Child Combinator) : 선택자 ABC의 자식 요소 XYZ 선택.

   ```html
   <style>
     ul > .test {
       color: red;
     }
   </style>

   <ul>
     <li class="test">test</li>
   </ul>
   ```

3. 하위(후손) 선택자 (Descendant Combinator) : 선택자 ABC의 하위 요소 XYZ 선택. '띄어쓰기'가 선택자의 기호!

   ```html
   <style>
     div .test {
       color: red;
     }
   </style>

   <div>
     <ul>
       <li class="test">test</li>
       얘는 선택 됨
     </ul>
   </div>
   <span class="test">test</span> 얘는 선택 안됨
   ```

### 가상 클래스 (Pseudo-Classes)

해당 요소에 특별한 상태가 적용되었을 때 선택하는 방법

- HOVER : 선택자 ABC 요소에 마우스 커서가 올라가 있는 동안 선택
- ACTIVE : 선택자 ABC 요소에 마우스를 클릭하고 있는 동안 선택
- FOCUS : 선택자 ABC 요소에 포커스가 있는 동안 선택
- first-child: 선택자 ABC 요소의 첫 번째 자식 요소 선택
- last-child: 선택자 ABC 요소의 마지막 자식 요소 선택
- nth-child(n): 선택자 ABC 요소의 n번째 자식 요소 선택

  ```html
  <!-- hover 예시 -->
  <style>
    .test:hover {
      color: red;
    }
  </style>

  <span class="test">test</span>
  ```

근데 hover나 active같은건 js로 처리하는게 좋은거 아닌가 하는 의문이 듬

### 가상 요소 (Pseudo-Elements)

- 요소 선택자 (Pseudo-Elements) BEFORE , AFTER: 선택자 ABC 요소의 내부 앞/뒤에 내용(Content)을 삽입.

  ```html
  <style>
    .test::before {
      content: "before";
    }
    .test::after {
      content: "after";
    }
  </style>

  <span class="test">test</span>
  ```

### 속성

- 속성 선택자 (Attribute): 특정 속성을 포함한 요소 선택
- 이거 자주 씀

  ```html
  <style>
    [disable] {
      color: red;
    }
  </style>

  <input type="text" value="test" disabled />

  <style>
    [type="text"] {
      color: red;
    }
  </style>

  <input type="text" value="test" />
  ```

<br>
<hr>
<br>

### 상속되는 CSS 속성들

모두 글자/문자 관련 속성들! (모든 글자/문자 속성은 아님 주의!)

|     속성명      |               설명               |
| :-------------: | :------------------------------: |
|      font       | 글꼴 관련 속성들을 한꺼번에 지정 |
|   font-family   |         글꼴(서체) 지정          |
|    font-size    |          글자 크기 지정          |
|   font-style    |         글자 기울기 지정         |
|   font-weight   |          글자 두께 지정          |
|   line-height   |           줄 높이 지정           |
|      color      |          글자 색상 지정          |
|   text-align    |          정렬 방법 지정          |
|   text-indent   |          들여쓰기 지정           |
| text-decoration |        문자 장식(선) 지정        |

> 자식요소에 강제 상속 하려면 inherit 값을 주면 됨

## 선택자 우선순위

### 우선순위란, 같은 요소가 여러 선언의 대상이 된 경우, 어떤 선언의 CSS 속성을 우선 적용할지 결정하는 방법

1. 점수가 높은 선언이 우선함!

2. 점수가 같으면, 가장 마지막에 해석된 선언이 우선함!

| 선택자 종류 | 점수 |
| :---------: | :--: |
| !important  |  ∞   |
|   inline    | 1000 |
|     id      | 100  |
|    class    |  10  |
|     tag     |  1   |
|     \*      |  0   |

<br>
<br>
<hr>
<br>
<br>

# CSS의 속성(Properties)

HTML과 비슷하게, CSS도 특정한 기능을 하는 속성들이 미리 정의되어 있으며, 이를 활용하여 태그의 스타일을 정의 할 수 있다.

속성의 종류는 매우 많지만 중요한 속성들은 다음과 같다.

- 박스 모델
- 글꼴, 문자
- 배경
- 배치
- 플렉스(정렬)
- 전환
- 변환
- 띄움
- 애니메이션
- 그리드
- 다단
- 필터

먼저 HTML과 CSS에서 사용하는 단위는 다음과 같다.

| 단위 |            설명             |
| :--: | :-------------------------: |
|  px  |            픽셀             |
|  %   |        상대적 백분율        |
|  em  |      요소의 글꼴 크기       |
| rem  | 루트 요소(html)의 글꼴 크기 |
|  vw  |  뷰포트 가로 너비의 백분율  |
|  vh  |  뷰포트 세로 너비의 백분율  |

<br>
<br>

## 박스 모델

CSS의 박스 모델은 HTML의 요소를 상자(박스)로 생각하고, 이 박스의 크기와 여백을 지정하는 속성들이다.

**크기와 레이아웃**

- **width, height**
  - 요소의 가로/세로 너비
  - 기본값: 브라우저 자동 계산
- **max-width, max-height**
  - 요소의 최대 가로/세로 너비
  - 기본값: 제한 없음
- **min-width, min-height**

  - 요소의 최소 가로/세로 너비
  - 기본값: 0

**여백과 내부 여백**

- **margin**
  - 요소의 외부 여백(공간) 지정
  - 기본값: 0
- **padding**
  - 요소의 내부 여백(공간) 지정
  - 기본값: 0
  - **요소의 크기가 증가한다!!!**

**테두리와 모양**

- **border**
  - 요소의 테두리 선 지정
  - border: 선-두께 선-종류 선-색상 (border-width, border-style, border-color)
  - **요소의 크기가 증가한다!!!**
- **border-style**
  - 테두리 선 종류
  - 기본값: none
  - solid: 실선
  - dashed: 파선(점선)
- **border-color**
  - 요소 테두리 선 색상 지정
  - 기본값: black
- **border-radius**
  - 요소 모서리 둥글게 깎음
  - 기본값: 0

**크기 계산과 내용 넘침**

- **box-sizing**
  - 요소 크기 계산 기준 지정
  - 기본값: content-box
  - content-box: 내용으로 크기 계산
  - border-box: 요소의 내용(content) + 여백(padding) + 테두리(border)까지 포함하여 크기 계산
  - border-box로 지정하면, 내가 원하는 크기의 박스를 얻고 싶을 때 유용
- **overflow**
  - 내용이 요소 크기를 넘어설 때 보여짐 제어
  - 기본값: visible
  - visible: 넘친 내용을 그대로 보여줌
  - hidden: 내용 잘라냄
  - scroll: 잘라내고 스크롤바
  - auto: 내용 있을 때 잘라내고 스크롤바

**화면 출력 특성**

- **display**
  - 요소의 화면 출력(보여짐) 특성 (각 요소에 이미 지정되어 있음)
  - 각 요소들의 기본값을 벗어나서, 내가 원하는 화면 출력 특성을 부여할 수 있다.
  - 기본값: block, inline, inline-block
  - block: 상자(레이아웃) 요소
  - inline: 글자 요소
  - inline-block: 글자 + 상자 요소
  - flex: 플렉스 박스 (1차원 레이아웃, 수평정렬에 용이함)
  - grid: 그리드 (2차원 레아이웃)
  - none: 보여짐 특성 없음, 화면에서 사라짐
  - 기타: table, table-row, table-cell 등

**투명도**

- **opacity**
  - 요소의 투명도
  - 기본값: 1 (불투명)
  - 0~1 사이 값으로 지정

<br>
<br>

## 글꼴, 문자

**font-style**

- 글자의 기울기
- 기본값: normal

**font-weight**

- 글자의 두께(가중치)
- 기본값: normal, 400
- bold, 700
- 100 ~ 900

**font-size**

- 글자의 크기
- 기본값: 16px

**font-family**

- 글꼴 지정

**line-height**

- 한 줄의 높이, 행간과 유사
- 기본값: normal
- 요소의 글꼴 크기의 배수로 지정

**color**

- 글자의 색상
- 기본값: rgb(0, 0, 0) , 검정색

**text-align**

- 문자의 정렬방식
- 기본값: left

**text-decoration**

- 문자의 장식(선)
- 기본값: none
- underline, line-through

**text-indent**

- 문자 첫 줄의 줄여쓰기
- 기본값: 0

<br>
<br>

## 배경

**background-color**

- 요소의 배경 색상
- 기본값: transparent (투명함)

**background-image**

- 요소의 배경 이미지 삽입
- 기본값: 이미지 없음
- url(”경로”)

**background-repeat**

- 요소의 배경 이미지 반복
- 기본값: repeat

**background-position**

- 요소의 배경 이미지 위치

**background-size**

- 요소의 배경 이미지 크기

**background-attachment**

- 요소의 배경 이미지 스크롤 특성
- 기본값: scroll
- scroll: 이미지가 요소를 따라서 같이 스크롤
- fixed: 이미지가 뷰포트에 고정, 스트롤 X

<br>
<br>

## 배치

**position**

- 요소의 위치 지정 기준
- top, bottom, left, right, z-index (모두 음수 가능)
- 기본값: static
- static: 기준 없음 (그냥 원래 있는 자리에 있음)
- relative: 요소 자신을 기준
  - 본인이 원래 있었던 그 자리 기준!
  - 배치 전 자리는 비어 있다 (그 자리에 다른 요소가 채워지지 않는다)
  - 그래서 자기 자신을 기준으로 배치하지는 않는다.
  - 보통은 position absolute의 구조상의 부모요소가 위치상의 부모요소가 될 수 있도록 포지션을 부여하는 용도로 쓰임
- absolute: 위치 상 부모 요소를 기준 (위치 상 부모 요소를 반드시 확인!)
  - 배치에서 가장 많이 사용됨
  - 부모 기준으로 배치가 되니, 원래자리는 없어짐. 즉, 원래 있던 자리에 다른 요소가 채워짐
  - 위치상의 부모요소의 position을 relative를 줘야 함
- fixed: 뷰포트(브라우저)를 기준

**top, bottom, left, right**

- 요소의 각 방향별 거리 지정
- 기본값: 브라우저가 계산

<br>
<br>

### 요소 쌓임 순서(Stack order)

어떤 요소가 사용자와 더 가깝게 있는지(위에 쌓이는지) 결정

1. 요소에 position 속성의 값이 있는 경우 위에 쌓임.(기본값 static 제외)
2. 1번 조건이 같은 경우, z-index 속성의 숫자 값이 높을 수록 위에 쌓임.
3. 1번과 2번 조건까지 같은 경우, HTML의 다음 구조일 수록 위에 쌓임. (더 늦게 선언되었을 수록)

**z-index**

- 요소의 쌓임 정도를 지정
- 기본값: 0
- 포지션 속성이 없을 경우, z-index가 아무리 높아도 밑에 있음

<br>
<br>

### 요소의 display가 변경됨!

position 속성의 값으로 absolute, fixed가 지정된 요소는, display 속성이 block으로 변경됨.

<br>
<br>

## 플렉스(정렬, 1차원 레이아웃)

block 속성의 박스에 display값을 flex를 주면,

1. 이때부터 해당 박스는 **flex container**가 되며, (자식 요소들은 **flex items** 라고 부름)
2. 자식 요소들이 수평 정렬된다.

이때, flex container와 flex items에 부여할 수 있는 속성들이 나뉘는데, 다음과 같다.

- flex container 속성들 : display, flex-flow, flex-direction, flex-wrap, justify-content, align-content, align-items
- flex items 속성들: order, flex, flex-grow, flex-shrink, flex-basis, align-self

<br>
<br>

### flex container 속성들

**display**

- flex container의 화면 출력(보여짐) 특성
- flex: 블록 요소와 같이 flex container 정의
- inline-flex: 인라인 요소와 같이 flex container 정의

**flex-direction**

- 주 축을 설정
- 기본값: row
- row: 좌 → 우
- row-reverse: 우 → 좌
- column: 위 → 아래
- column-reverse: 아래 → 위

**flex-wrap**

- flex items 묶음(줄 바꿈) 여부
- 기본값: nowrap
- nowrap: 묶음(줄 바꿈) 없음
- wrap: 여러 줄로 묶음 (줄 바꿈 있음)

**justify-content**

- 주 축의 정렬 방법
- 기본값: flex-start
- flex-start: flex items를 시작점으로 정렬
- flex-end: flex items를 끝점으로 정렬
- center: flex items를 가운데 정렬
- space-between: 각 flex item 사이를 균등하게 정렬
- space-around: 각 flex item의 외부 여백을 균등하게 정렬

**align-content**

- 교차 축의 여러 줄 정렬 방법 (두 줄 이상이라 별로 안씀)
- 기본값: stretch
- stretch: flex items를 시작점으로 정렬
- flex-start: flex items를 시작점으로 정렬
- flex-end: flex items를 끝점으로 정렬
- center: flex items를 가운데 정렬
- space-between: 각 flex item 사이를 균등하게 정렬
- space-around: 각 flex item의 외부 여백을 균등하게 정렬

**align-items**

- 교차 축의 한 줄 정렬 방법 (이걸 더 많이 씀)
- 기본값: stretch
- stretch: flex items를 시작점으로 정렬
- flex-start: flex items를 시작점으로 정렬
- flex-end: flex items를 끝점으로 정렬
- center: flex items를 가운데 정렬

<br>
<br>

### flex items 속성들

**order**

- flex item의 순서
- 기본값: 0
- 숫자가 작을수록 먼저 배치

**flex-grow**

- flex item의 증가 너비 비율
- 기본값: 0
- 숫자로 증가비율을 부여

**flex-shrink**

- flex item의 감소 너비 비율
- 기본값: 1
- 0을 입력하면 감소가 없는 것

**flex-basis**

- flex item의 공간 배분 전 기본 너비
- 기본값: 요소의 Content 너비
- 0을 입력하면 기본 컨텐츠 너비를 0으로 설정함
- flex-grow와 함께 사용하여, 원래 컨텐츠 너비는 무시하고, 비율대로 배분하고 싶을 때 사용함

<br>
<br>

## 정리

역시 진리의 _"알고 보니 쉽다"_ (그렇다고 css가 쉽다는건 아님!) 그동안 css를 너무 피했었는데, 이번에 다시 공부하면서, 그동안 내가 css를 너무 어렵게 생각했던 것 같다. 사실 그냥 이해를 하려는 노력을 안한게 맞는 것 같다. 앞으로 프론트를 만질일이 더 많아질 것 같은데, 모르는 내용이 나올때마다 주먹구구로 피하지 말고 공부해서 내것으로 만들자.
