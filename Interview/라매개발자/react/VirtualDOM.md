### React Fiber

**등장 배경**

React에서 Virtual DOM의 핵심은 `현재` 그리고 `새롭게 생성된` 상태를 담은 Virtual DOM을 비교하는 “재조정자(reconciler)”입니다. 결국 변경되는 부분만 실제 DOM에 반영하기 위해서는 어디가 바뀌었는지를 확인하는 것이 가장 중요하기 때문입니다.

- **before : Stack Reconciler 스택 재조정자**
  - V16 이전의 React는 Reconciler의 동작이 `스택 알고리즘`(깊이 우선 탐색)으로 이루어져 있었습니다.
  - 단순한 React Element로 구성된 Tree가 Virtual DOM의 역할을 했고, Current Tree와 WIP(Working in Progress) Tree를 비교하고 화면에 변경사항을 반영하는 모든 작업을 “Stack”에 담아 동기적으로 수행했습니다.
  - 동기적으로 수행되는 작업은 `일시정지 되거나`, `취소될 수 없으며` 또한 `작업이 끝날 때까지 새로운 작업을 수행하지 못해` 버벅이는 화면을 보여주는 등 사용자 경험을 저하시켰습니다.

[stack reconciler 가 재귀적으로 virtual DOM을 탐색하는 순서를 시각화한 애니메이션](https://codepen.io/ejilee/pen/ZExPQRE)

stack reconciler 가 재귀적으로 virtual DOM을 탐색하는 순서를 시각화한 애니메이션

- **after : Fiber Reconciler 파이버 재조정자**
  - 앞 선 Stack Reconciler의 약점을 보완하고자 v16 부터 등장한 재조정자 엔진입니다.
  - Fiber Reconciler는 렌더링 작업을 잘게 쪼개어 여러 프레임에 걸쳐 실행할 수 있습니다.
  - 특정 작업에 “우선 순위”를 매겨두어 작업의 작은 조각들을 “일시 정지”, “재가동” 할 수 있게 되었습니다.
  - 중요한 점은 Fiber Tree는 실제로 Tree의 구조가 아닌, `return`, `sibling`, `child`를 통해 다음으로 수행할 Fiber Node를 가리켜 연결하는 Linked List 구조를 가지고 있습니다

[fiber reconciler 가 각 fiber의 return child sibling 을 참조하여 트리를 탐색하는 순서를 시각화한 애니메이션](https://codepen.io/ejilee/pen/eYMXJPN)

fiber reconciler 가 각 fiber의 return child sibling 을 참조하여 트리를 탐색하는 순서를 시각화한 애니메이션

- Fiber Tree는 깊이 우선 탐색이 아닌 Fiber Node가 가지고 있는 `return`, `sibling`, `child`를 통해 다음 탐색할 위치를 찾습니다.
  - 우선적으로, child가 있다면 child가 가리키는 fiber node를 다음으로 탐색
  - child가 없다면, sibling가 가리키는 fiber node를 다음으로 탐색
  - sibling이 없다면, return을 통해 본인 이전에 탐색한 fiber node로 복귀
- 이러한 Linked List 형태의 탐색 덕분에, 우선 순위가 높은 작업으로 인해 현재 reconciliation 작업이 멈추더라도 다시 돌아올 fiber node를 `return`을 통해 이어서 작업이 가능케 되었습니다.
- 화면에 반영이 필요한 요소들은 마지막 commit 단계에서 한번에 화면에 반영되기 때문에, reconciliation 작업이 중단되더라도 렌더링에 아무런 영향을 미치지 않습니다.

### React Fiber 작업 순서

**Phase1 : 이펙트 정보를 포함한 새로운 Fiber 트리를 만들어내는 것**

concurrent 하게 일시 정지되고 재가동될 수 있습니다. user input 혹은 animation 같은 급한 작업이 있는지 수시로 확인하여, 만약 앞서 수행해야 될 부분이 있다면, 해당 요소를 먼저 처리하거나 중단할 수 있다.

<aside>
🎯 **참고) 이펙트란?**

이펙트는 두 트리를 비교한 후 감지된 ‘바뀐 점, 바뀌어야 할 작업’을 의미

이펙트의 속성은 다음과 같습니다.

- `flags` : 이펙트, 변화의 유형, 정수. 바이너리로 숫자로 표기되어있고, bitwise OR assignment (`|=`)를 사용해서 여러 변화의 조합을 `workInProgress.flags |= Placement` 이런 식으로 축적합니다
- `nextEffect` : reconciliation 과정에서 새로운 변경 사항 혹은 이펙트가 생겼을 때, 이전 이펙트가 발생했던 fiber의 nextEffect 가, 이 새로운 이펙트를 가진 fiber를 가리키도록 합니다. 덕분에, commit 단계에서 변화가 생긴 Fiber node만을 빠르게 탐색할 수 있습니다.
- `firstEffect`, `lastEffect` : fiber 하위에 있는 서브 트리에서의 첫 이펙트를 가진 fiber 와 마지막 이펙트를 가진 fiber 를 가리킵니다.
- `alternate` - 해당 fiber의 교대 fiber 입니다.
</aside>

1. **최초 렌더링 시, 모든 Fiber node의 관계를 가진 Fiber node tree를 생성하고, 화면에 반영**
   - 해당 Fiber node tree를 current tree로 지정
2. **어플리케이션에 업데이트가 발생할 경우, react는 current tree를 복제하여 새로운 tree를 생성**
   - 새롭게 복제한 tree를 WIP tree로 지정
3. **React는 beginWork() 함수를 실행해 파이버 작업을 수행**
   - “Fiber node의 input으로 들어온 값”과 “current tree의 Fiber node의 Prop값”을 비교하여, 변경 요소가 있을 경우 effect 유형을 표시한다.
   - beginWork()는 다음으로 수행할 자식 Fiber node를 반환합니다.
4. **beginWork()가 반환한 Fiber node를 따라 계속해서 작업을 진행**
   - 자식이 없는 Fiber node로 인해 null이 반환될 때까지 진행
5. **다음 Fiber node가 `null`일 경우 completeUnitofWork() 함수를 실행**
   - 현재 작업을 수행하고 있는 Fiber node를 완료 표시
   - completeWork() 함수를 호출
     - 만약, completeWork() 호출 후 child node가 생겼다면 다시 탐색을 시작
   - 현재 작업을 수행하고 있는 Fiber node에 sibling이 있다면 sibling Fiber node로 이동하여 탐색을 시작
   - child와 sibling이 둘 다 없다면, return으로 이전에 작업한 Fiber node로 돌아온다.
6. **root node까지 마지막으로 완료 표시를 한 경우, 최종적으로 내부에 effect를 포함한 Fiber 트리를 만들어 내고 다음 단계로 이동**

**Phase2 : 모든 작업을 real DOM에 반영**

- WIP Tree에 담긴 effect에 따라 화면에 반영시켜주는 작업을 수행해줍니다
  - 이 과정을 `flush` 라고 합니다.
  - 이 과정은 동기적으로 수행되며, 작업을 취소하거나 멈출 수 없습니다.
- 화면에 변경 사항을 반영한 후, current Tree가 가리키는 포인터를 최종적으로 생성한 WIP Tree로 바꾸어줍니다.

## 관련 자료 정리

> 답변과 관련된 자료 및 북마크 등을 작성해주세요

[DOM의 새로운 발견! Virtual DOM 동작 원리](https://velog.io/@sunhwa508/Virtual-DOM)

[재조정 (Reconciliation) – React](https://ko.legacy.reactjs.org/docs/reconciliation.html)

[React 파이버 아키텍처 분석](https://d2.naver.com/helloworld/2690975)

[React Fiber 얕게 알아보기](https://velog.io/@jangws/React-Fiber)

[React Deep Dive — Fiber](https://blog.mathpresso.com/react-deep-dive-fiber-88860f6edbd0)

[React 톺아보기 - 05. Reconciler_1 | Deep Dive Magic Code](https://goidle.github.io/react/in-depth-react-reconciler_1/)
