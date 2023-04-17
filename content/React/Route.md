+++
title = "Route"
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Tab

```
npm install --save react-tabs
```

—save: development purpose and deployed

https://reactcommunity.org/react-tabs/

https://github.com/reactjs/react-tabs

```js
import { Tab, Tabs, TabList, TabPanel } from "react-tabs";
import "react-tabs/style/react-tabs.css;

<Tabs>
  <TabList>
    <Tab>Title 1</Tab>
    <Tab>Title 2</Tab>
  </TabList>

  <TabPanel>
    <h2>Any content 1</h2>
  </TabPanel>
  <TabPanel>
    <h2>Any content 2</h2>
  </TabPanel>
</Tabs>
```

## Router

Routing is what component to render when a user enters a new URL

https://reactrouter.com

```
npm install react-router-dom
```

### Browser Router

```js
import { BrowserRouter, Routes, Route } from “react-router-com”;

function AppRouter () {
	return (
		<BrowserRouter>
		    <Nav />
			<Routes>
				<Route path=“/“ exact element={App />} />
				<Route path=“/*” element={<PageNotFound />
			</Routes>
		</BrowserRouter>} />); }

export default AppRouter;
```

### Create Browser Router

##### Entry

```js
import { createBrowserRouter, RouterProvider } from "react-router-dom";

function App() {
  const router = createBrowserRouter([
    {
      path: "/",
      element: <Nav />,
      children: [
        { path: "/", element: <App /> },
        { path: "/profile", element: <Profile /> },
      ],
    },
  ]);
  return (
    <AuthProvider>
      <RouterProvider router={router} />{" "}
    </AuthProvider>
  );
}
```

##### Nav.js

```
import { Outlet } from 'react-router-dom';
....
return ( <><nav></nav><Outlet /></> )
```

### Authenticated Routes

##### AuthRoute.js

```js
import { Navigate, useLocation } from "react-router-dom";

export default function AuthRoute({ children }) {
  const { user } = useContext(AuthContext); // Global Variable
  const location = useLocation();
  if (!user) {
    return <Navigate to={{ pathname: "/login", state: { from: location } }} />;
  }
  return children;
}
```

##### Entry

```js
element: <AuthRoute>
  <Profile />
</AuthRoute>;
```

### NavLink

It enables to style :active for navigation bar menu

```
import { NavLink } from “react-router-dom”;

<NavLink to=“PATH”> HOME </NavLink>
```

### Link

Internal link in the app, use Link

```
import { Link } from “react-router-dom”;

<Link to=“PATH”> HOME </Link>
```

{{% notice info %}}
External Link : `<a href=""></a>`
{{%/notice%}}

### Document Title

```js
useEffect(() => {
  document.title = `About`;
}, []);
```

## Responsive Nav

```js
// Track Nav Open or Close (useState)
const [ navOpen, setNavOpen ] = usetState(false);

// handle navOpen by clicking
const showHideNav = () => {
    setNavOpen(!navOpen);
    }

// 데스크탑이면 메뉴 닫기
const isDesktop = (e) => {
    if (e.matches) {
        setNavOpen(false);
        }
    }

const closeNav = (e) => {
    if (window.innerWidth < 600 ){
        showHideNav();
        } else {
            e.target.blur(); } }

// navOpen controll by window size changes
useEffect( () => {
	let mediaQuery = window.matchMedia(‘(min-width: 600px)’);
	mediaQuery.addEventListener(‘change’, isDesktop);
	return () => mediaQuery.removeEventListener(‘change’, isDesktop); }, [] )

return (
	// 햄버거 버튼
<div onClick={showHideNav}>

// 메뉴
<nav onClick={closeNav} />
	<ul><li>Menu 1</li> <li>Menu 2</li></ul></nav> </div> );
```
