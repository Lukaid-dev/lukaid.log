## 1.

안녕하세요. 15조 백준룸즈입니다. 그리고 저는 발표자 이성우입니다. 반갑습니다!

저희는 실시간 코딩 경쟁 플랫폼인 백준 룸즈를 기획하고 열심히 개발하고 있습니다.

## 2.

이번 발표에서는 한번도 리액트를 사용해보지 못한 세명의 개발자가 모여서, 리액트를 학습하고, 적용하면서 어떤 어려움이 있었는지, 어떤 방식으로 해결했는지를 공유하고자 합니다.

당연히 저희보다 훨씬 잘 하시는 분들이 많은 것으로 아는데, 아 쟤네는 저런데서 어려워했구나 하면서 한번 들어보시면 좋을 것 같습니다.

## 3.

여러가지 어려움이 있었는데, 대표적으로 이 세가지에서 많이 힘들었습니다. 첫번째 상태관리에 대한 고민, 두번째 라우팅에 관한 고민, 세번째 리액트 쿼리와 서스펜스에 대한 고민

## 4.

오늘은 여기서 두번째 고민인 라우팅에 대해서 그중에서도 특별히 버전 6에 대해서 얘기해 볼게요. 사실 첫번째 고민인 상태관리도 준비했었는데, 이게 내용이 너무 길어지고 깊어져서 이번에는 제외를 했습니다.

간단하게만 말씀드리면 저희는 전역 상태관리 라이브러리를 도입하지는 않고, context api로 모든것을 해결하고 있는 중입니다.

## 5.

저는 사실 리액트에는 url을 사용하는 라우팅이라는 개념이 있는지도 몰랐습니다. 그래서 예전에 리액트 라우터를 사용한 경험이 있는 팀원분의 도움을 받아 다음과 같이 코드를 작성해 봤습니다. 아주 잘 작동했지만, 더 공부를 하기 위해 공식문서에 방문했을 때 이것이 구버전이라는 것을 알게 되었습니다.

## 6.

신버전에서 가장 큰 특징은 createBrowserRouter를 사용하는 것이더라구요. 공식문서에서 대놓고 모든 리액트 라우터 플젝에 권장하는 라우터라고 써있더라구요. 그 뒤에 문장은 잘 이해를 못했는데, 아무튼 권장한다고 하니 이를 학습하고 적용하기로 했습니다.

## 7.

createBrowserRouter를 사용하기로 하고, 가장 큰 목표는 다음과 같이 두가지였습니다. 첫번째는 라우터를 독립시키는 것, 두번째는 data APIs를 활용하는 것입니다.

## 8.

저는 flutter로 개발한 경험이 있는데, 이 예제를 보니 flutter의 goRouter나 autoRoute와 비슷한 느낌이었습니다. createBrowserRouter를 이용해서 라우터 객체를 반환해주고, 라우터 객체에 우리 앱에서 사용되는 모든 페이지를 등록해주면 되더라구요. 개인적으로는 이게 구버전보다 훨씬 직관적이고 좋은 것 같습니다. 여기서 path가 domain뒤에 붙는 path , http://localhost:3000/home -> /home 이고, children이 선언되는 곳은 App컴포넌트에 Outlet으로 지정해주면 됩니다. 만약에 Children안에 또 다른 Children이 있어서 이를 쭉 타고 들어가고 싶다면, 원하는 컴포넌트에 Outlet을 지정해주면 됩니다.

그리고 보통 이렇게 App 컴포넌트의 Outlet을 감싸도록 각종 Provider를 선언해주더라구요 저희도 context나 tanstackquery를 이렇게 선언해주었습니다.

## 9.

그래서 결국 라우터에 담겨있는 이 컴포넌트들을 렌더링 해줘야 할텐데 그 역할을 하는게 RouterProvider라는 컴포넌트 입니다. 얘도 마찬가디로 react-router에서 가져와 쓰는건데 저렇게 router property에 우리가 선언한 라우터를 넣어주면 됩니다. 그리고 라우터 위에서 이동은 Link컴포넌트를 이용하면 됩니다.

## 10.

이제 첫번째 목표인 라우터 독립은 됐고, data apis는 뭘까요? 공식문서에서는 다음과 같이 설명하는데, 데이터를 로드, 변경 및 재검증할 시점에 관한 것... 라우팅을 위한 데이터 로딩 및 처리를 보다 효율적으로 관리할 수 있도록 하는 기능....

그러니까 data를 router단에서 처리하는 것에 대한 내용인 것 같습니다. 오늘은 loader에 대해서만 얘기해 볼게요.

## 11.

이런 상황을 가정해봅시다. 충분히 일어날만한 상황이죠. ~~~~~

react router는 이럴때 data apis가 유용할 것이라고 말하고 있습니다. 진짜 그런지 함 보죠

## 12.

모든 Route는 `"loader"`함수를 정의할 수 있고, 각 라우트가 렌더링되기 전에 호출된다고 합니다. 오른쪽의 코드를 보면, 원하는 컴포넌트를 렌더하는 라우트에 loader함수를 정의해주고, 그 안에서 data를 fetch해주고 있습니다.

이때 이 함수가 완료되기 전 까지 라우팅은 일어나지 않습니다. 그리고 해당 data는 `useLoaderData`를 통해 접근할 수 있습니다.

## 13.

프로덕트 라우트만 떼어와서 보면 다음과 같습니다. 여기서는 목데이터를 가져오는데 실제면 여러분 서버의 리소스에 접근하게 되겠죠. 그리고 오른쪽에 보시면 프로덕트 컴포넌트가 있는데 여기서 useLoaderData를 통해 data에 접근하고 있습니다. 이렇게 하면 data가 로드되기 전까지는 프로덕트 컴포넌트가 렌더링되지 않습니다.

## 14.

근데 데이터를 가져오는데 많은 시간이 걸린다면 어떨까요? 오른쪽 코드는 제가 인위적으로 3초정도 딜레이를 줬습니다.

## 15.

그런 경우에는 프로덕트 페이지로 라우트하기 전 페이지에서 navigate의 state를 파악해서 로딩중임을 알려주는 것이 좋을 것 같습니다. 저는 그냥 Loading...이라는 텍스트를 띄웠습니다.

## 16.

데모를 같이 보시죠. 프로덕트 페이지에 필요한 정보를 얻어오기 전까지 라우트가 일어나지 않습니다. 그리고 로딩중이라는 것도 알려줄 수 있죠. 위에 url도 바뀌지 않는 것을 알 수 있습니다.

## 17.

네 여기까지 6버전의 리액트 라우터에 대해서 간단하게만 알아봤습니다. 이외에도 정말 많은 기능이 있는데, 여러분이 이런 기능들을 학습하실때 제 발표가 조금이나마 도움이 되었으면 좋겠습니다. 감사합니다.
