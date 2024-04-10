+++
title = "React"
weight = 1
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

# Redux

React state management library

## Version

- React 17
- Redux 4
- React Router 5

# Environment Build

### Dev environment

---

- Compile JSX
- Transpile JS
- Linting
- Generate index.html
- Reload on save
  => one command (npm)
  with
  Node, Webpack, Babel, ESLint, npm Scripts

### Webpack

- webpack.config.dev.js file

### Babel

- Transpile modern JS : Tell Babel what browsers you need to support
- Compile JSX to JS : So the browser understand the code

#### Configure Babel

- Go to `package.json` and add below

```
  "babel": {
    "presets": [
      "babel-preset-react-app"
    ]
  },
```

### NPM

- `package.json`

```
  "scripts": {
    "start": "webpack serve --config webpack.config.dev.js --port 3000"
  },
```

npm start to run

### Debugging

put debugger; inside the app

### ESLint

ESLint will alert us when we make mistakes

## Summary

Webpack

- Babel
- ESLint

# React Component Approaches

## Create React Components

There are 4 Ways to Create React Components

#### 1. createClass Component

The original way to create a React component

```js
var HellowWorld = React.createClass({
  render: function () {
    return <h1>Hello World!</h1>;
  },
});
```

#### 2. JS Class Component

```js
class HelloWorld extends React.Component {
  constructor(props) {
    super(props);
  }

  render() {
    return <h1>Hello World</h1>;
  }
}
```

#### 3. Function Component

```js
function HelloWorld(props) {
  return <h1>Hello World</h1>;
}
```

#### 4. Arrow Function Component

const HelloWorld = (props) => <h1>Hello World</h1>

####

- Class Component vs Function Component
- React Life Cycle
- React Hool (useEffect, useRef,...)

## Container vs Presentation

#### Container (Stateful, Controller View)

- Have little or no markup
- ex) Backend for the frontend
- Mainly passing data and actions down to child components.
- Knows about Redux
- Often statueful

#### Presentation (Stateless, View)

- Nearly all markup
- Receipt data and actions via props
- Doesn't know about Redux
- Often no state

---

# Initial App Structure

---

# Intro to Redux

## State

The state is a built-in React object that is used to contain data or information about the component.
A component's state can change over time. Whenever it changes, the component re-renders.

> ### State Management

1. Lift State (Parent to Child Component)

- Lifted to common ancestor of two components
- Pass down to children : Prop drilling
- Good for small app

2. React Context

- You can use global data
- Import the context and consume the context in your component
- Provider : In your top level component
- Consumer : Use Consumer where you use global data.

3. Redux

- Has one separte Store to store global data
- Any components can `dispatch an action` (ex. createUser) -> Redux Store update to the new data
- Any connected components receieved this new data from the Redux store => It rerender!

> ### Redux vs. Context API

Redux는 Flux 아키텍처 패턴을 구현한 라이브러리로, 전역 상태 관리를 위해 사용된다. Redux는 애플리케이션의 상태를 중앙 집중적으로 관리하고, 상태 변경을 예측 가능하도록 만든다. 이를 위해 Redux는 불변성을 유지하고, 상태 변경을 위한 액션(Action)을 사용하며, 이 액션들을 통해 상태를 변경하는 리듀서(Reducer)를 작성한다.

반면, Context API는 React v16.3에서 소개된 기능으로, 컴포넌트 트리에서 전역 데이터를 공유하기 위한 방법이다. Context API는 Provider와 Consumer를 사용하여 상위 컴포넌트에서 하위 컴포넌트로 데이터를 전달한다. 이를 통해 Redux와 같은 전역 상태 관리가 가능하지만, Redux보다는 간단하고 쉽게 구현할 수 있다.

## Redux

Redux is a predictable state container designed to help you write JavaScript apps that behave consistently across client, server, and native environments

> ### Why Redux

- One Store : Redux centralized all of your application state in a single store, avoid storing the same data more than once
- Recuced Bolilerplate (less boilerplate than Flux)
- Isomorphic/Universal Friendly : Architecture is friendly to server rendering your React components
- Immutable Store
- Hot Reloading
- Time-travel debugging : step foward and backward through state
- Small (2k)

> ### 3 Core Redux Principles

#### 1. One immutable store

Your application state is placed in a single `immutable store`

#### 2. Actions trigger changes

The only way to change state in Redux is to emit an action

#### 3. Reducers update state (Reducers return updated state)

State changes are handled by pure functions which are called `reducers`

> ### Redux Flow

Store, Action, Reducer

#### Store

Store is where storing global state.

```js
// Create Store
let store = createStore(reducer);
```

`store.dispatch(action)` The store can dispatch an action

`store.subscribe(listener)` can subscribe to a listener

`store.getState()` return its current state

`replaceReducer(nextReducer)` replace a reducer, for hot reloading

{{< vbox blue >}}
v changing state is only by dispatching the action. <br>
v actions are handled by reducers
{{< /vbox >}}

#### Action

```js
{type: RATE_COURSE, rating: 5}
```

The events happening in the application are called actions. Actions are plain objects containing a description of an event. An action **must have a type property**. It describes a user's intent.

It can be simple number, boolean, any value that is serializable. No functions or promises.

Actions typically created by 'Action creators'. Typically the action creator has the same name as the action's type.

```js
// Action Creator
rateCourse(rating) {
  return { type: RATE_COURSE, rating: rating } // returing action
}
```

#### Reducer

Reducer is a function that takes state in and action and returns new state to change(update) the store. Usually contains a switch statement

When the new state is returned from the Reducer, the store is updated.

React re-renders any components that are utilizing the data using React-Redux.

Reducer

```js
function myReducer(state, action) {
  switch (action.type) {
    case "INCREMENT_COUNTER":
      state.counter++; // state is immutable, can't change like this.
      return state;
    default:
      return state;
  }
}
```

Reducer example

```js
function myReducer(state, action) {
  switch (action.type) {
    case "INCREMENT_COUNTER":
      return { ...state, counter: state.counter + 1 }; // right way to update the state.
    default:
      return state;
  }
}
```

1. Copy the object : ...state
2. Update the counter property from copied obejct : counter: state.counter + 1

`Reducers must be pure`.

They should produce no side effects

#### Forbidden in Reducers

- Mutate arguments
- Perform side effects : like API calls or routing transitions
- Call non-pure functions

A reducer's return value should depend solely on the values of its parameters

It should call no other non-pure funtions like date.now or math.random

Instead of mutating state,
`Reducers should return an updated copy of state. Redux will use that copy to update the store`

All Reducers are called on each dispatch : That is why all reducers should return the untoched state as the default

Each reducer only handles its slice of state.

#### Subscription

Store에 보관된 State를 가져옴

---

## Immutability

Redux store is immutable.

### Immutability?

To change state, return a new object.
Immutable state : instead of changing your state object, you must return a new object that represents your application's new state.

Mutating is updating the state

Immutable already

- Number
- String
- Boolean
- Undefined
- Null

=> every time you change the value of these, a new copy is created.

Mutable

- Objects
- Arrays
- Functions

### Handlilng Immutable State

#### Native JS

- Object.assign
- Spread operator
- Map, filter, reduce

#### Libraries

- Immer
- seamless-immutable
- react-addons-update
- Immutable.js

### Why Immutability?

Each time you need to change your store's state, you must return an updated copy.

Why? 3 reasons.

1. Clarity

- It is clear to know what file you have to open to see the state changing.

2. Performance

- Imporve performance
- It doesn't have to check if the property in the state has changed or not (because it is immutable). Instead, it checks the reference in the memory to see if the state has changed or not.

3. Awesome Sauce

- Time-travel debugging
- Undo/Redo
- Turn off individual actions
- Play interations back

### Handling Immutablility

How Do I Enforce Immutability?

- use library like immer, immutable.js, seamless-immutable,,,,

---

# Connecting React to Redux

## Container vs Presentational Components

| Container                | Presentational            |
| ------------------------ | ------------------------- |
| Focus on how things work | Focus on how things look  |
| Aware of Redux           | Unaware of Redux          |
| Subscribe to Redux State | Read data from props      |
| Dispatch Redux actions   | Invoke callbacks on props |

## React-Redux Introduction

> ### React-Redux

React-redux ties your React Components together to Redux. It consists with Provider component and the Connect function.

> ### Provider
>
> It is utilized at your app's route. It wraps your entire application.

```js
<Provider store={this.props.store}>
  <App />
<Provider>
```

Connect function
A function provided by React Redux. It connects your React components to the Redux store.

```js
export default connect(mapStateToProps, mapDispatchToProps)(AuthorPage);
```

mamState : what state you want to pass ?
mapDispatchToProps : what action you want to pass ?
both parameters are optional.

## mapStateToProps

What state should I expose as props?
When you define connect function,
The component will subscribe to the Redux store updates,
and anytime it updates, mapStateToProps will be called.
This function returns an object.

## mapDispatchToProps

## A Chat with Redux

---

# Redux Flow

---

# Async in Redux

---

# Async Writes in Redux

---

# Async Status and Error Handling

...

# 리덕스 쉽게

## 리덕스 쓰는 이유

Props 귀찮을 때
State(변수)
State보관함.js = store.js여기에 state를 전부다 보관
세팅방법은

```js
// index.js
import { Provider } from 'react-redux';
import { createStore } from 'redux';

const weight = 100;


function reducer(state = weight, action){
  // 여기다가 스테이트 수정하는 방법을 전부다 여기에 씀. 이걸 해주는걸 리듀서라고 부름.
  if (action.type === 'add){
    state++;
    return state
  }
  return state;


let store = createStore(reducer)

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
)}
```

```js
// App.js
import { useSelector } from "react-redux";

function App() {
  const we = useSelector((state) => state);

  // 수정요청을 하는걸 여기서 부름. 그걸 해주는게 디스패치
  const dispatch = useDispatch();

  return (
    <div>
      {we}
      <button
        onClick={() => {
          dispatch({ type: "add" });
        }}
      >
        더하기
      </button>
    </div>
  );
}
```

쓰는 이유 2 : State 관리 용이

컴포넌트들은 store.js에다가 상태를 변경해달라고 요청만 한다
그럼 에러 났을때 컴퍼넌트 하나하나 안봐도 되고 store.js에서만 보면 되니까 추적이 쉽당
