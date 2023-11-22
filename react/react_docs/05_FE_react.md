# 알게된 단편적(이제는 단편적이 아니게 된...) 지식들 여섯번째 - state를 다루는 방법 (2)

<br>

[리액트 공식문서](https://react.dev/)
[비공식 번역문서](https://react-ko.dev/)
[스터디 영상](https://youtube.com/playlist?list=PLjQV3hketAJkh6BEl0n4PDS_2fBd0cS9v&si=EqsHu_o7l1zGcDZd)

<br>

## [Preserving and Resetting State | state 보존 및 재설정](https://react-ko.dev/learn/preserving-and-resetting-state)

<br>

state는 컴포넌트 간에 격리되어 있으며, React는 UI 트리에서 어떤 컴포넌트가 어떤 state에 속하는지를 추적한다. ...? 잠깐 컴포넌트가 state에 속한다고..? React가 UI트리를 그려서 DOM을 관리하는 것은 알고있었다. 그런데 state가 컴포넌트에 속한다는 것은 처음 알았다...

<br>

### [State is tied to a position in the tree | state는 트리의 한 위치에 묶입니다](https://react-ko.dev/learn/preserving-and-resetting-state#state-is-tied-to-a-position-in-the-tree)

<br>

컴포넌트에 state를 부여할 때, state가 컴포넌트 내부에 “존재”한다고 생각할 수 있지만, 실제로는 React 내부의 어떤 공간에 모든 컴포넌트의 state가 모여있다. React는 UI 트리에서 해당 컴포넌트가 어디에 위치하는지에 따라 보유하고 있는 각 state를 올바른 컴포넌트와 연결해준다. 그러니까 state만을 보아주는 별도의 저장공간이 있다고 생각하자.

<br>

그래서, **동일한 위치의 동일한 컴포넌트는 state를 유지한다.** 중요한 것은 JSX 마크업이 아니라 UI 트리에서의 위치이다. UI트리상 같은 위치에 존재한다면, 어떤 다른 state가 update되어 리렌더링되더라도, 해당 컴포넌트는 그 위치에 그대로 있기 때문에 state가 유지된다.

<br>

같은 논리로 **동일한 위치의 다른 컴포넌트는 state를 초기화한다.** 구조가 다르면 React는 트리에서 컴포넌트를 제거할 때 state를 파괴하기 때문에, 리렌더링 사이에 state를 유지하려면 트리의 구조가 “일치”해야한다.

<br>

이에 대해 더 자세한 설명은 그냥 공식문서를 옮겨적는 것에 지나지 않을 것 같아 생략한다. 공식문서의 해당 부분을 반드시 다시 읽어보자.

<br>

## [Extracting State Logic into a Reducer | State로직을 Reducer로 추출하기](https://react-ko.dev/learn/extracting-state-logic-into-a-reducer)

<br>

하나의 컴포넌트에 여러 state 업데이트가 여러 이벤트핸들러에 분산되어 있다면, 이는 과부하를 일으킬 수 있다. 이때, reducer라는 단일 함수를 통해 컴포넌트 외부의 모든 state 업데이트 로직을 통합할 수 있다.

<br>

보통 `useState`로 개발을 하다가 로직이 복잡해지면 `useReducer`로 마이그레이션을 할텐데, 이때 아래의 세 단계를 따르면 된다.

<br>

**1. Move** from setting state to dispatching actions.

- state를 설정하는 것에서 action들을 전달하는 것으로 변경하기 (여기서 action은 사용자가 설정하는 js 객체)

**2. Write** a reducer function.

- reducer 함수 작성하기

**3. Use** the reducer from your component.

- 컴포넌트에서 reducer 사용하기

<br>

```jsx
// taskReducer.jsx
export default function tasksReducer(tasks, action) {
  switch (action.type) {
    case "added": {
      return [
        ...tasks,
        {
          id: action.id,
          text: action.text,
          done: false,
        },
      ];
    }
    case "changed": {
      return tasks.map((t) => {
        if (t.id === action.task.id) {
          return action.task;
        } else {
          return t;
        }
      });
    }
    case "deleted": {
      return tasks.filter((t) => t.id !== action.id);
    }
    default: {
      throw Error("Unknown action: " + action.type);
    }
  }
}
```

<br>

```jsx
// App.jsx
import { useReducer } from "react";
import AddTask from "./AddTask.js";
import TaskList from "./TaskList.js";
import tasksReducer from "./tasksReducer.js";

export default function TaskApp() {
  const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);

  function handleAddTask(text) {
    dispatch({
      type: "added",
      id: nextId++,
      text: text,
    });
  }

  function handleChangeTask(task) {
    dispatch({
      type: "changed",
      task: task,
    });
  }

  function handleDeleteTask(taskId) {
    dispatch({
      type: "deleted",
      id: taskId,
    });
  }

  return (
    <>
      <h1>Prague itinerary</h1>
      <AddTask onAddTask={handleAddTask} />
      <TaskList
        tasks={tasks}
        onChangeTask={handleChangeTask}
        onDeleteTask={handleDeleteTask}
      />
    </>
  );
}

let nextId = 3;
const initialTasks = [
  { id: 0, text: "Visit Kafka Museum", done: true },
  { id: 1, text: "Watch a puppet show", done: false },
  { id: 2, text: "Lennon Wall pic", done: false },
];
```

<br>

주의해야 할 점은 **reducer는 반드시 순수해야 한다는 것이다.** setState와 마찬가지로 reducer는 렌더링 중에 실행되며 side effect를 일으키면 안된다. 요청을 보내거나 timeout을 스케쥴링하거나 사이드 이펙트(컴포넌트 외부에 영향을 미치는 작업)을 수행해서는 안 된다.

<br>

## [Passing Data Deeply with Context | context로 데이터 깊숙이 전달하기](https://react-ko.dev/learn/passing-data-deeply-with-context)

<br>

상태관리는 리액트에서 가장 중요한 부분이자 어려운 부분이라고 할 수 있다. 나는 리액트 플젝을 진행하면서도 다른 상태관리 라이브러리보다 context api를 사용하였는데, 이를 사용하면 부모 컴포넌트가 props를 통해 명시적으로 전달하지 않고도 깊이 여부와 무관하게 그 아래 트리의 모든 컴포넌트에서 일부 정보를 사용할 수 있다.

<br>

### [Context: an alternative to passing props | Context: props 전달의 대안](https://react-ko.dev/learn/passing-data-deeply-with-context#context-an-alternative-to-passing-props)

<br>

context는 다음의 세 단계로 진행한다.

<br>

**1. Create** a context.

- context를 생성합니다.

**2. Use** that context from the component that needs the data.

- 데이터가 필요한 컴포넌트에서 해당 context를 사용합니다.

**3. Provide** that context from the component that specifies the data.

- 데이터를 지정하는 컴포넌트에서 해당 context를 제공합니다.

<br>

이와 관련해서 예제가 있는데 내가 느끼기에는 살짝 복잡한 느낌이 있었다. 요약하자면 내가 원하는 상태를 담는 context를 `createContext`로 생성하고, `<anyContext.Provider></anyContext.Provider>`를 통해서 해당 상태를 전달하고 싶은 컴포넌트들을 감싼 후, `useContext`를 통해 해당 컴포넌트에서 사용하면 된다.

<br>

Context는 중간 컴포넌트들을 통과한다. context를 제공하는 컴포넌트와 context를 사용하는 컴포넌트 사이에 원하는 만큼의 컴포넌트를 삽입할 수 있다. Context를 사용하면 “주변 환경에 적응”하고 렌더링되는 위치 (context)에 따라 다르게 표시되는 컴포넌트를 작성할 수 있다. 또한 서로 다른 React context도 서로 재정의하지 않는다.(서로 완전히 독립이다.) `createContext()`로 만드는 각 context는 다른 context와 완전히 분리되어 있으며, 특정 context를 사용하거나 제공하는 컴포넌트를 함께 묶으며, 하나의 컴포넌트가 문제없이 다양한 context를 사용하거나 제공할 수 있다.

<br>

놀라운 사실은(사실 별로 놀랍진 않다.) 잘 알려진 대부분의 상태관리 라이브러리(redux, recoil, jotai, zustand 등)는 context를 기반으로 구현되어 있다는 것이다. 또한 react에서 가장 유명한 라우터 라이브러리인 react-router도 context를 기반으로 구현되어 있다. 그러니 context를 잘 이해하고 사용하는 것이 중요하겠지..?

<br>

## 정리

<br>

리액트를 처음 학습할 때 정말 고생을 많이 했던 (사실 지금도 어렵다) 상태관리에 대한 정리를 어느정도는 끝낸 것 같다. 이제 프로젝트를 본격적으로 진행하고 있으니, 플젝을 통해서 직접 코드를 작성하고 그간 학습한 개념들을 재조합하고 내 개념으로 만들어야겠다.
