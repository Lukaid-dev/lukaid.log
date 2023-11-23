---
marp: true
theme: gaia
class: lead
paginate: true
footer: "BaekjoonRooms_boostcamp_2023"
---

# Web 15. BaekjoonRooms

Real-time Online Coding Competition Platform

---

![bg right:50% 90%](chobo.jpeg)

한번도 (리액트를) 경험하지 못한
세명의 프론트개발자(진)이 모였다!

이것은 props인가 state인가!

**극한개발!**

---

고민 1. 상태관리가 그렇게 중요하다던데, 라이브러리는 뭘 쓰지?

고민 2. 라우팅? SPA에 라우팅이 있어?

고민 3. react-query와 suspense는 필수라고 하시던데 이건 또 뭐지?

---

오늘은 고민1과 고민2에 대한 경험을 공유해보겠습니다. (고민3은 다음에...)

**고민 1. 상태관리**가 그렇게 중요하다던데, 라이브러리는 뭘 쓰지?

**고민 2. 라우팅**? SPA에 라우팅이 있어?

~~고민 3. react-query와 suspense는 필수라고 하시던데 이건 또 뭐지?~~

---

<!-- header: 고민 1. 상태관리!! -->

우리는 어떤 경험도 없으니 후보들을 나열해보자.

후보 1. redux

후보 2. recoil

후보 3. jotai

후보 4. zustand

---

<!-- header: 고민 1. 상태관리!! -->

우리는 어떤 경험도 없으니 후보들을 나열해보자.

후보 1. redux -> 전 회사 프론트 개발자 야근의 주범. 그리고 사장되는 분위기래

후보 2. recoil

후보 3. jotai

후보 4. zustand

---

<!-- header: 고민 1. 상태관리!! -->

우리는 어떤 경험도 없으니 후보들을 나열해보자.

~~후보 1. redux -> 전 회사 프론트 개발자 야근의 주범. 그리고 사장되는 분위기래~~

후보 2. recoil -> redux 보다는 낫다! 근데 meta에서 손 땠다는데?

후보 3. jotai

후보 4. zustand

---

<!-- header: 고민 1. 상태관리!! -->

우리는 어떤 경험도 없으니 후보들을 나열해보자.

~~후보 1. redux -> 전 회사 프론트 개발자 야근의 주범. 그리고 사장되는 분위기래~~

~~후보 2. recoil -> redux 보다는 낫다! 근데 meta에서 손 땠다는데?~~

후보 3. jotai -> 자료가 거의 없다..

후보 4. zustand

---

<!-- header: 고민 1. 상태관리!! -->

우리는 어떤 경험도 없으니 후보들을 나열해보자.

~~후보 1. redux -> 전 회사 프론트 개발자 야근의 주범. 그리고 사장되는 분위기래~~

~~후보 2. recoil -> redux 보다는 낫다! 근데 meta에서 손 땠다는데?~~

~~후보 3. jotai -> 자료가 거의 없다..~~

후보 4. zustand -> 아주 핫한 신생 라이브러리! 그래서 근본없어보임..

---

<!-- header: 고민 1. 상태관리!! -->

우리는 어떤 경험도 없으니 후보들을 나열해보자.

~~후보 1. redux -> 전 회사 프론트 개발자 야근의 주범. 그리고 사장되는 분위기래~~

~~후보 2. recoil -> redux 보다는 낫다! 근데 meta에서 손 땠다는데?~~

~~후보 3. jotai -> 자료가 거의 없다..~~

~~후보 4. zustand -> 아주 핫한 신생 라이브러리! 그래서 근본없어보임..~~

---

<!-- header: 고민 1. 상태관리!! -->

그래 결론은 **근본**이다! 근본이 중요하다!

---

<!-- header: 고민 1. 상태관리!! -->

그래서 저희는 상태관리의 근본. **Context API**를 사용하기로 했습니다.

<p style="font-size: 16px"> 물론 다른 상태관리 라이브러리들이 근본 없다는 말은 아닙니다...(눈치) </p>

---

<!-- header: 고민 1. 상태관리!! -->

공식문서

> useContext is a React Hook that lets you read and subscribe to context from your component.

> useContext는 컴포넌트에서 context를 읽고 구독할 수 있게 해주는 React Hook입니다.

context는 다음의 세 단계로 진행합니다.

---

<!-- header: 고민 1. 상태관리!! -->

**1. Create** a context. | context를 생성합니다.

**2. Use** that context from the component that needs the data. |
데이터가 필요한 컴포넌트에서 해당 context를 사용합니다.

**3. Provide** that context from the component that specifies the data. | 데이터를 지정하는 컴포넌트에서 해당 context를 제공합니다.

---

<!-- header: 고민 1. 상태관리!! -->

![bg 90%](passing_data_context_close.webp)

![bg 90%](passing_data_context_far.webp)

---
