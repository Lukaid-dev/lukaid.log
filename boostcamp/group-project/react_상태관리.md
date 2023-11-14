# 리액트 상태관리

네부캠에서 이제 그룹프로젝트를 시작하고 서비스의 기획과 요구사항 분석을 마친 후 본격적으로 작업에 들어가려고 한다. 나는 웹 클라이언트와 크롬 익스텐션 개발을 담당하게 되었는데, 웹 클라이언트의 경우 리액트로 작성하게 되었다. 사실 리액트를 제대로 배운 것도, 제대로 활용하는 것도 모두 처음이라 감이 잘 없었는데, 막상 플젝을 시작하려고 하니, 결정해야하는 것들이 참 많았다.

<br/>

리액트는 프레임워크가 아닌 라이브러리로, 프레임워크가 제공하는 방식대로 개발하는 것이 아니라 개발자가 여러 요구사항에 맞게 모듈들을 가져와서 붙여가며 작업해야한다. 우리는 상태관리, CSS라이브러리, 라우팅, 네트워크 통신 모듈 등에서 수많은 선택지가 있고, 어떤 선택이 우리의 서비스에 가장 잘 맞는지 고민하고 있다.

오늘은 이 중 상태관리에 대해 자세히 알아보고자 한다.

<br/>

<p align="center">
  <img src="https://user-images.githubusercontent.com/97015501/282668089-1fab175c-2a11-4f16-bfaf-b249ac9fd4f6.jpg" width="500" height="300">
</p>

<br/>

## 상태관리 라이브러리는 반드시 써야하는 것인가?

<br/>

> 보통 선택지에 "반드시"가 들어가면 정답이 아닌 경우가 많다...

<br/>

현대의 웹 애플리케이션은 사용자와의 상호작용이 많아지고, 그에 따라 상태관리가 복잡해지고 있다. 리액트는 컴포넌트의 상태를 관리하기 위해 `state`를 사용한다. `state`는 컴포넌트 내부에서 관리되는 상태로, 컴포넌트 내부에서 변경할 수 있고, `props`로 자식 컴포넌트에 전달할 수 있다. 컴포넌트 간에 공유해야하는 상태가 한두개면 뭐.. 그렇다고 칠 수 있는데, 이게 복잡해지고 많아진다면 문제가 시작된다.

<br/>

이를 위해 여러 상태관리 라이브러리들이 제안되는데, `redux`, `recoil`, `zustand`등이 있다. 나는 **반드시 이런 라이브러리들을 도입해야하나?** 라는 의문이 들었다. 일단, 우리가 만들고자하는 서비스의 규모는 그렇게 크지 않고, **무엇보다 라이브러리의 도입에는 명확한 이유**가 있어야 한다고 배웠기에 이러한 의문이 드는 상황에서는 라이브러리를 도입하지 않는 것이 좋을 것 같다는 생각이 들었다.

<br/>

따라서 우선은 상태관리 라이브러리 없이 리액트에서 제공하는 기본적인 기능과 hooks만을 이용하여 서비스를 개발해보기로 했다. 그리고 만약 상태관리가 복잡해지고, 라이브러리를 도입해야할 필요성이 느껴진다면 그때 도입해도 늦지 않을 것이라고 생각했다.

<br/>

## 그럼 라이브러리 없이 어떻게 상태를 관리할거야?

<br/>

일단 컴포넌트 내부에서, 그리고 두세대 정도의 컴포넌트의 상태관리는 고민 할 필요가 없다. 문제는 여러 컴포넌트 간에 공유되는 상태를 어떻게 관리할 것인가이다. 이를 위해 우리는 `Context API`를 사용하기로 했다.

<br/>

### context API를 사용하면 어떤 문제가 해결되는가?

<br/>

리액트에서 props는 반드시 부모에서 자식요소로 전달되어야 하며, 세대를 건너서 전달될 수 없다. 그래서 쓸모도 없는 props를 단지 자식에 건내줄 목적으로만 전달받는 경우가 생기는데 이런 현상을 props drilling이라고 한다. 이 props drilling을 해결하기 위해 context를 사용한다.

<br/>

`App Component`에서 user정보를 가져오고, 이를 `Profile Component`와 `Profile Component`의 자식 Component인 `Edit Profile Component`에 전달하고 싶다고 가정해보자. 그리고 `Profile Component`에서는 user의 정보만을 필요로 하고, 이를 수정하는 메서드는 `Edit Profile Component`에서 필요하다면, `Profile Component`는 수정 로직이 필요 없지만 단순히 자식 Component에게 전달하기 위해 user정보를 받아야한다. 이런 경우에는 Context API를 사용하면 된다.

<br/>

```
디렉토리 구조

src
├── App.jsx
├── EditProfile.jsx
├── Profile.jsx
└── UserContext.jsx
```

<br/>

우선 상태의 저장소 개념의 context를 만들어야 한다. 이는 컴포넌트 외부에 선언되며 전역에서 임포트만 한다면 언제든 접근 할 수 있다.

<br/>

```jsx
// UserContext.jsx
import { createContext } from "react";

export const UserContext = createContext();
```

<br/>

다음으로 가장 상단에 위치한 Component인 `App Component`이다. 여기서 상단에 위치한다는 말은, 서비스 전체의 상단을 의미 하는 것이 아니고, 우리가 전달할 state를 기준으로 봤을 때 최상단을 의미한다.

<br/>

```jsx
// App.jsx
import { useState } from "react";
// UserContext를 가져온다.
import { UserContext } from "./UserContext";
import "./App.css";
import Profile from "./Profile";

function App() {
  const [user, setUser] = useState({
    id: 1,
    name: "김철수",
    nickname: "reactMaster",
    age: 20,
  });

  // user의 나이를 변경하는 메서드
  const onChangeNickname = (nickname) => {
    setUser({
      ...user,
      nickname,
    });
  };

  return (
    // 내가 context를 전달하고 싶은 하위 컴포넌트들을 UserContext.Provider로 감싸준다.
    // 그리고 전달하고 싶은 정보들을 value에 넣어준다.
    <UserContext.Provider value={{ user, onChangeNickname }}>
      <Profile />
    </UserContext.Provider>
  );
}

export default App;
```

<br/>

다음은 `App Component`의 자식 Component인 `Profile Component`이다. `Profile Component`에서는 user의 정보만을 필요로 하고, 이를 수정하는 메서드는 `Edit Profile Component`에서 필요하다. context를 사용하지 않는다면, `Profile Component`도 꼼짝없이 필요도 없는 props들을 전달받아야 하겠지만, 이제는 그렇지 않다.
<br/>

```jsx
// Profile.jsx
import { useContext } from "react";
import { UserContext } from "./UserContext";
import EditProfile from "./EditProfile";

function Profile() {
  // UserContext에서 내가 필요한 정보만을 가져와도 된다.
  const { user } = useContext(UserContext);

  return (
    <div>
      <h1>프로필</h1>
      <p>ID: {user.id}</p>
      <p>이름: {user.name}</p>
      <p>닉네임: {user.nickname}</p>
      <p>나이: {user.age}</p>
      <EditProfile />
    </div>
  );
}

export default Profile;
```

<br/>

마지막으로 `Profile Component`의 자식 Component인 `Edit Profile Component`이다. 여기서는 state의 수정 메서드인 `onChangeNickname`가 필요하다. 이는 부모에서 전달 받는 것이 아니고, context에서 꺼내온다.

<br/>

```jsx
// EditProfile.jsx
import { useState, useContext } from "react";
import { UserContext } from "./UserContext";

function EditProfile() {
  // onChangeNickname를 부모가 아닌 context에서 꺼내온다.
  const { user, onChangeNickname } = useContext(UserContext);
  const [content, setContent] = useState("");

  const onChangeContent = (e) => {
    setContent(e.target.value);
  };

  const onClick = () => {
    onChangeNickname(content);
  };

  return (
    <div>
      <h1>프로필 수정</h1>
      <p>이름: {user.name}</p>
      <p>나이: {user.age}</p>
      <div>
        <input type="text" value={content} onChange={onChangeContent} />
        <button onClick={onClick}>수정</button>
      </div>
    </div>
  );
}

export default EditProfile;
```

<br/>

## 정리

<br/>

이렇게 간단하게 context API에 대해서 공부해봤다. 더 자세하고 깊은 내용은 나보다 훨씬 잘 정리해두신 분들이 많겠지만, 내가 이해한 내용을 바탕으로 한번 정리해보았다. 이 글을 읽는 누구라도 context API에 대해 이해하는데 도움이 되었으면 좋겠다.
