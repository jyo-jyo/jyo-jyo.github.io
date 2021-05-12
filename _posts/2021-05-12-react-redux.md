---
category: doc
tags: [redux, react]
---

React의 경우 부모 컴포넌트가 State 값을 관리하고, 하위 컴포넌트로 값을 상속해 주기 때문에, 직관적이고 관리하기 편하다는 장점이 있다.
하지만, 프로젝트의 규모가 커지면 아래와 같은 경우에는 다소 코드가 복잡해져 유지보수에 어려움이 있다.
 - 최상위 컴포넌트에서 최하위 컴포넌트 상속
 - 하나의 값을 여러 컴포넌트에서 업데이트 및 사용

물론, 하위 컴포넌트끼리 직접 소통하는 방법도 있긴 하지만 (Ref 사용)
이렇게 하면 코드가 굉장히 꼬이기 때문에 권장하지 않는다.

따라서, 이러한 경우 해당 상태값을 전역으로 두어 관리하는 방법을 이용하는 것이 좋다.
이를 위해 사용하는 것이 redux이다.
리덕스(redux)는 전역 상태 보관소 Store를 이용하여 모든 컴포넌트로 상태값을 공급한다.
이를 통해, 리액트 구조의 단점을 보완할 수 있다.

- 리덕스 구조
1. 액션(action) : 상태 값에 어떠한 변화가 필요할 때 발생시킨다.

👉 어떤 동작에 대해 선언되어진 객체

👉 반드시 type 필드를 가져야함

// Action types
export const TYPE_NAME = 'SIGN_OUT';
// Action creators
export const actionCreators = () => {
    return{
        type: TYPE_NAME,
    }
};

2. 리듀서(reducer) : 상태 값에 변화를 일으키는 함수이다.

👉 직접 일거리를 수행 = 순수함수

import * as Actions from '../actions/{action_file_name}';
const reducers = (state = initialStates, actions) => {
  const { type } = actions;
  switch (type) {
    case Actions.TYPE_NAME: {
      return {
        ...state,
        value: xxx,
      }
    }
    default: {
      return {
        ...state,
      }
    }
  }
}

3. 스토어(store) : 전역 상태 보관소

👉 모든 컴포넌트로 상태값을 공급

import { createStore } from 'redux';

const create = (reducers) => {
  return createStore(reducers);
}

export default create;

- dispatch : 스토어 값 업데이트 시 사용
- subscribe: 스토어 값을 읽어올 때 사용

이러한 리덕스에는 3가지 원칙이 존재하며, 반드시 지켜야한다.

1️⃣ Store는 유일하여야 한다.
2️⃣ 상태값은 읽기만 가능하다.
3️⃣ 상태 값의 변경은 순수함수로만 가능하다.

