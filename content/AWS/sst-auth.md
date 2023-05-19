+++
title = "SST & Auth"
weight = 12
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Backend

Add Auth to both ApiStack and FrontendStack

> #### ApiStack.js

Add **Cognito**, **auth**, **authorizers**, **default authorizer**

Once it's all set, you are not allowed to change any configs. (need to delete every stacks to re-do it)

```js
import { Api, Cognito } from "sst/constructs";

export function ApiStack({ stack }) {
  // Create auth provider
  const auth = new Cognito(stack, "Auth", {
    login: ["email"],
    // login: ["email", "username"],
  });

  // Adjust the API
  const api = new Api(stack, "Api", {
    authorizers: {
      jwt: {
        type: "user_pool",
        userPool: {
          id: auth.userPoolId,
          clientIds: [auth.userPoolClientId],
        },
      },
    },
    defaults: {
      authorizer: "jwt",
    },
    routes: {
      "POST /private": "packages/functions/src/lambda.handler",
      "GET /public": {
        function: "packages/functions/src/lambda.handler",
        authorizer: "none",
      },
    },
  });

  // Allow authenticated users invoke API
  auth.attachPermissionsForAuthUsers(stack, [api]);

  stack.addOutputs({
    ApiEndpoint: api.url,
    UserPoolId: auth.userPoolId,
    IdentityPoolId: auth.cognitoIdentityPoolId ?? "",
    UserPoolClientId: auth.userPoolClientId,
  });

  return {
    api,
    auth,
  };
}
```

> #### FrontendStack.js

Add **auth**, **Environment variables**

```js
import { StaticSite, Api, Auth, use } from "sst/constructs";
import { API } from "./MyStack";

export function FrontendStack({ stack, app }) {
  const { api, auth } = use(API);

  const site = new StaticSite(stack, "ReactSite", {
    path: "frontend",
    buildOutput: "dist",
    buildCommand: "yarn build",
    // Pass in our environment variables
    environment: {
      VITE_APP_API_URL: api.url,
      VITE_APP_REGION: app.region,
      VITE_APP_USER_POOL_ID: auth.userPoolId,
      VITE_APP_USER_POOL_CLIENT_ID: auth.userPoolClientId,
      VITE_APP_IDENTITY_POOL_ID: auth.cognitoIdentityPoolId ?? "",
    },
  });

  // Show the url in the output
  stack.addOutputs({
    SiteUrl: site.url || "",
  });
}
```

> #### Lambda

You can access the logged in user’s username and sub (id) from the event object.

```js
export async function handler(event: any, context: any) {
  const sub = event.requestContext.authorizer?.jwt.claims.sub;
  const username = event.requestContext.authorizer?.jwt.claims.username;

  return {
    statusCode: 200,
    body: JSON.stringify({
      message: "Hello world!",
      sub,
      username,
    }),
  };
}
```

Use this to store the user’s id in the database when they create a new item.

## Frontend

cd into frontend

```
yarn add aws-amplify
```

> #### Configure Amplify

```js
import { Amplify } from "aws-amplify";

const amplifyConfig = {
  Auth: {
    mandatorySignIn: false,
    region: import.meta.env.VITE_APP_REGION,
    userPoolId: import.meta.env.VITE_APP_USER_POOL_ID,
    userPoolWebClientId: import.meta.env.VITE_APP_USER_POOL_CLIENT_ID,
  },
  API: {
    endpoints: [
      {
        name: "api",
        endpoint: import.meta.env.VITE_APP_API_URL,
        region: import.meta.env.VITE_APP_REGION,
      },
    ],
  },
};
Amplify.configure(amplifyConfig);
```

> #### Register

```js
import { Auth } from "aws-amplify";

Auth.signUp({
  username: email,
  password,
})
  .then(() => {})
  .then((err) => err.message);
```

> #### Verification

```js
Auth.confirmSignUp(email, code)
  .then((res) => {
    console.log(res);
  })
  .catch((err) => setError(err.message));
```

> #### Login

```js
Auth.signIn(email, password)
  .then(() => {})
  .catch((err) => setError(err.message));
```

> #### Header Authorization

```js
headers: {
    Authorization: `Bearer ${(await Auth.currentSession()).getAccessToken().getJwtToken()}`,
  },
```

> #### Auth Class

https://aws-amplify.github.io/amplify-js/api/classes/authclass.html

#### {{% expand "Auth with UI" %}}

```

yarn add aws-amplify @aws-amplify/ui-react

```

- aws-amplify : for aws
- ui-react : auto ui style

> #### vite.config.js

```js
export default defineConfig({
  plugins: [react()],
  define: {
    global: {},
  },
});
```

or `global:"window"`

> #### Configure auth

Ususally configure where the routers are. (`App.jsx`)

```js
import { BrowserRouter, Link, Route, Routes } from "react-router-dom";
import { Amplify } from "aws-amplify";
import { Authenticator } from "@aws-amplify/ui-react";
import RouteGuard from "./RouteGuard";

import Login from "./Login";
import Home from "./Home";

const amplifyConfig = {
  Auth: {
    mandatorySignIn: false,
    region: import.meta.env.VITE_APP_REGION,
    userPoolId: import.meta.env.VITE_APP_USER_POOL_ID,
    userPoolWebClientId: import.meta.env.VITE_APP_USER_POOL_CLIENT_ID,
    // identityPoolId: import.meta.env.VITE_APP_IDENTITY_POOL_ID,
  },
  API: {
    endpoints: [
      {
        name: "api",
        endpoint: import.meta.env.VITE_APP_API_URL,
        region: import.meta.env.VITE_APP_REGION,
      },
    ],
  },
};
Amplify.configure(amplifyConfig);

export default function App() {
  return (
    <Authenticator.Provider>
      <BrowserRouter>
        <main>
          <Routes>
            <Route
              path="/"
              element={
                <RouteGuard>
                  <Home />
                </RouteGuard>
              }
            />
            <Route path="/login" element={<Login />} />
          </Routes>
        </main>
      </BrowserRouter>
    </Authenticator.Provider>
  );
}
```

> #### How to use

```js
import import { Auth, API } from "aws-amplify";
import { useAuthenticator } from "@aws-amplify/ui-react"

export default function Home() {
  const { user, signOut } = useAuthenticator((context) => [context.user]);
```

> #### Route guard

```js
import { Navigate } from "react-router-dom";
import { useAuthenticator } from "@aws-amplify/ui-react";

function RouteGuard({ children }) {
  const { route } = useAuthenticator((context) => [context.route]);

  if (route == "idle") {
    return <></>;
  }

  if (route !== "authenticated") {
    return <Navigate to="/login" />;
  }

  return children;
}

export default RouteGuard;
```

> #### Login with ui

```js
import { useAuthenticator } from "@aws-amplify/ui-react";
import { Navigate } from "react-router-dom";
import { Authenticator } from "@aws-amplify/ui-react";

import "@aws-amplify/ui-react/styles.css";

export default function Login() {
  const { route } = useAuthenticator((context) => [context.route]);

  if (route == "idle") {
    return <></>;
  }

  if (route == "authenticated") {
    return <Navigate to="/" />;
  }

  return <Authenticator signUpAttributes={["email"]} />;
}
```

{{% /expand%}}
