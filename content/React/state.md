+++
title = "Statement Management"
menuTitle = "Statement"
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Passing data from child to parent component

## Context API

#### Create Context

```js
import { createContext } from "react-router-dom";

const CounterContext = createContext(initialState);

function CounterProvider({ children }) {
  return <CounterContext.Provider>{children}</CounterContext.Provider>;
}
```

#### Index.js or App.js

```js
<CounterProvider value={(a, b)}>
  <App />
</CounterProvider>
```

#### Reading

```js
import { useContext } from ‘react’;
import { CounterContext } from ‘..path’;

const { a, b } = useContext(CounterContext);
```
