+++
title = "Use Effect"
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

#### React useEffect Hook

- 컴포넌트가 렌더링 될 때 특정 작업을 실행할 수 있도록 하는 react hook.
- useEffect handles side effect code to keep track of outside react code (Put side-effect code inside useEffect)
- Side-effect : Code outside React such as browser APIs, localStorage, native DOM
- Syntax : useEffect( call back function, dependency array )
- Call back function
- It contains side-effect code (whatever you want to execute)
- Dependency array
- If the array is empty, useEffect will run once after initial render
- If props or state are passed as an array, it will run any time they changes

#### Setting Document Title

```js
useEffect(() => {
  document.title = `About`;
}, []);
```

without going to detail, let user see the graph?

#### useTable

columns

- required
- must be memoized
- the core columns configuration object for the entire table
- nested colmun available

data

- required
- must be memoized

What is Memoization?
In programming, memoization is an optimization technique that makes applications more efficient and hence faster.

It does this by storing computation results in cache, and retrieving that same information from the cache the next time it's needed instead of computing it again.

In simpler words, it consists of storing in cache the output of a function, and making the function check if each required computation is in the cache before computing it.

A cache is simply a temporary data store that holds data so that future requests for that data can be served faster.

###

In React, we can optimize our application by avoiding unnecessary component re-render using memoization.

components re-render because of two things: a change in state or a change in props. This is precisely the information we can "cache" to avoid unnecessary re-renders.

```js
React.memo(Component or array)
```

various application ->

I've built : 디테일하게 적기, 유명한거 적기

이름 발음 보단 프로젝트를 올리기

프로젝트 : 3개
