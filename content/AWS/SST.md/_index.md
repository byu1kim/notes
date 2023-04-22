+++
title = "SST"
weight = 5
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## IAM User

## Install AWS CLI

#### Windows

#### Mac

## Backend

- It is a framework that makes it easy to build full-stack applications on AWS
- It will automatically connect to the AWS with your `aws configure` information (created on AWS CLI day)

#### Setup

```
npx create-sst@latest
cd [name]
npm install
```

#### Stack

Rename stacks/MyStack.ts to stacks/ApiStack.js

```
import { Api } from "sst/constructs";

export function API({ stack }) {
  const api = new Api(stack, "api", {
    routes: {
      "GET /": "packages/functions/src/lambda.handler",
    },
  });
  stack.addOutputs({
    ApiEndpoint: api.url,
  });

  return {
    api,
  };
}
```

#### Config File

sst.config.ts > sst.config.js and remove all red lines

```
import { API } from "./stacks/API";

export default {
  config(_input) {
    //...
  },
  stacks(app) {
    app.stack(API);
  }
}
```

or you can change the profile like (actually you don't need it at all)

- add .env
- .env : AWS_PRPFILE=profilename
- in sst.config.js, add profile:process.env.AWS_PROFILE

#### Lambda

Rename lambda.ts to lambda.js in packages/functions/src

```
export async function handler(event, context) {
  return {
    statusCode: 200,
    body: JSON.stringify({
      time: "Hey world"
    }),
  };
}
```

Run the app

```
npx sst dev
```

## Frontend

Do this in the root of your SST app (Yarn)

```
yarn create vite frontend --template react
cd frontend
yarn
yarn add -D sst
```

Create React App version (or Vite)

```
npx create-react-app frontend
cd frontend
npm add -D sst
```

Update the dev script

```
  "scripts": {
    "dev": "sst env vite", (yarn) or  "dev": "sst env react-scripts start" (react)
```

Create a new file in stackes/FrontendStack.js

```
import { StaticSite, use } from "sst/constructs";
import { API } from "./ApiStack";

export function FrontendStack({ stack, app }) {
  const { api } = use(API);

  const site = new StaticSite(stack, "ReactSite", {
    path: "frontend",
    buildOutput: "dist",
    buildCommand: "yarn build",
    environment: {
      VITE_API_URL: api.customDomainUrl || api.url,
    },
  }); // change pink to npm version if you use create-react-app

  stack.addOutputs({
    SiteUrl: site.url || "",
  });
}
```

sst.config.js

```
import { API } from "./stacks/ApiStack";
import { FrontendStack } from "./stacks/FrontendStack";

export default {
  config(_input) {
    //...
  },
  stacks(app) {
    app.stack(API).stack(FrontendStack)
  }
}

```

Use env in Backend

```
Vite : const apiUrl = import.meta.env.VITE_API_URL
React : const apiUrl = process.env.REACT_APP_API_URL
```

Run the app

```
yarn dev / npm run dev
```
