+++
title = "React"
weight = 1
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

redux : react state management library

# Version

- React 17
- Redux 4
- React Router 5

# Environment Build

### Why Redux

- One Store : Redux centralized all of your application state in a single store, avoid storing the same data more than once
- Recuced Bolilerplate (less goiler plate than Flux)
- Isomorphic/Universal Friendly : Architecture is friendly to server rendering your React components
- Immutable Store
- Hot Reloading
- Time-travel debugging : step foward and backward through state
- Small (2k)

### Dev environment

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

---

# Actions, Stores, and Reducers

---

# Connecting React to Redux

---

# Redux Flow

---

...
