+++
title = "Intro"
weight = 1
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## React

Javascript library for web and native user interfaces

## Environment Setup

### 1. Use Boilerplate

#### Create React App

```
npx create-react-app [name]
cd [name]
npm start
```

#### Vite

{{< tabs >}}
{{% tab name="NPM" %}}

```
npm create vite@latest
```

{{% /tab %}}

{{% tab name="Yarn" %}}

```
yarn create vite
```

{{% /tab %}}
{{< /tabs >}}

### 2. Manual Setup

- Compile JSX, Transpile JS : Babel
- Linting : ESLing
- Generate index.html : Webpack
- Reload on save : NPM

## Initial File Structure

- src
  - components
    - page folders
    - App.js
    - 404.js
  - index.js

##### Index.js

```js
import React from "react";
import { render } from "react-dom";
import { BrowserRouter as Router } from "react-router-dom";
import "bootstrap/dist/css/bootstrap.min.css";
import App from "./components/App";
import "./index.css";

render(
  <Router>
    <App />
  </Router>,
  document.getElementById("app")
);
```

#### App.js

```js
import React from "react";
import { Route, Switch } from "react-router-dom";
import HomePage from "./home/HomePage";
import AboutPage from "./about/AboutPage";
import Nav from "./common/Nav";
import PageNotFound from "./PageNotFound";

function App() {
  return (
    <div className="container-fluid">
      <Nav />
      <Switch>
        <Route exact path="/" component={HomePage} />
        <Route path="/about" component={AboutPage} />
        <Route component={PageNotFound} />
      </Switch>
    </div>
  );
}

export default App;
```

#### Nav.js

```js
import React from "react";
import { NavLink } from "react-router-dom";

const Nav = () => {
  const activeStyle = { color: "#F15B2A" };
  return (
    <nav>
      <NavLink to="/" activeStyle={activeStyle} exact>
        Home
      </NavLink>
      <NavLink to="/about" activeStyle={activeStyle}>
        About
      </NavLink>
    </nav>
  );
};

export default Nav;
```

## Key terms

#### JSX

Javascript syntax for writing HTML in Javascript
