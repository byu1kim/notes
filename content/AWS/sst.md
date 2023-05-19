+++
title = "SST"
weight = 11
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

SST(Serverless Stack Toolkit) is an open-source framework for building serverless apps. SST converts your **infrastructure code** into a **CloudFormation template**.

https://sst.dev/

## Initial Configuration

> #### Install

```
npx create-sst@latest [name]
```

Change ts file to js (config, stack, function)

> #### Config File `sst.config.ts`

By default, the app will be deployed to the us-east-1 AWS region.

sst.config.ts > `sst.config.js` and remove all red lines

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

{{< vbox blue >}}
You can change the profile like <br>

- add .env<br>
- .env : AWS_PRPFILE=profilename<br>
- in sst.config.js, add profile:process.env.AWS_PROFILE<br>
  {{< /vbox >}}

> #### Stack folder

App Infrastructure, where you work with APIs, frontend set up file

Rename stacks/MyStack.ts to `stacks/ApiStack.js`

```js
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

> #### Lambda

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

## Frontend

> #### Install React

Do this in the root of your SST app

{{< tabs >}}
{{% tab name="Vite" %}}

```
yarn create vite frontend --template react
cd frontend
yarn add -D sst
```

{{% /tab %}}

{{% tab name="React" %}}

```
npx create-react-app frontend
cd frontend
npm add -D sst
```

{{% /tab %}}
{{< /tabs >}}

Update the dev script

{{< tabs >}}
{{% tab name="Vite" %}}

```
"scripts": {
    "dev": "sst env vite",
```

{{% /tab %}}

{{% tab name="React" %}}

```
"scripts": {
    "dev": "sst env react-scripts start",
```

{{% /tab %}}
{{< /tabs >}}

> #### Add the React app to SST

Create a new file in stacks/FrontendStack.js

```js
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
  });

  stack.addOutputs({
    SiteUrl: site.url || "",
  });
}
```

sst.config.js

```js
import { API } from "./stacks/ApiStack";
import { FrontendStack } from "./stacks/FrontendStack";

export default {
  config(_input) {
    //...
  },
  stacks(app) {
    app.stack(API).stack(FrontendStack);
  },
};
```

> #### Environment Variable from Backend

{{< tabs >}}
{{% tab name="Vite" %}}

```js
const apiUrl = import.meta.env.VITE_API_URL;
```

{{% /tab %}}

{{% tab name="React" %}}

```js
const apiUrl = process.env.REACT_APP_API_URL;
```

{{% /tab %}}
{{< /tabs >}}

## Run the app

> #### Backend

```
npx sst dev
```

The first time you run this command it’ll ask you for the name of a `stage`. A stage or an environment is just a string that SST uses to namespace your deployments.

This can take a few min for the first time.

> #### Frontend

{{< tabs >}}
{{% tab name="Vite" %}}

cd frontend

```
yarn dev
```

{{% /tab %}}

{{% tab name="React" %}}
cd frontend

```
npm run dev
```

{{% /tab %}}
{{< /tabs >}}

---

## PostgreSQL

> #### Connection String

.env file

```
DATABASE_URL=connection_string
```

> #### Database File

Create a new file in `packages/core/src/database.js`

```js
import pg from "pg";
const { Pool } = pg;

let pool;
function getPool() {
  if (!pool) {
    const connectionString = process.env.DATABASE_URL;
    pool = new Pool({
      connectionString,
      application_name: "",
      max: 1,
    });
  }
  return pool;
}

export async function getChats() {
  const res = await getPool().query(`
  SELECT * FROM chats
  ORDER BY timestamp DESC
  `);
  return res.rows;
}

export async function createChat(name) {
  const res = await getPool().query(
    `
  INSERT INTO chats (name)
  VALUES ($1)
  RETURNING *
  `,
    [name]
  );
  return res.rows[0];
}

export async function deleteChat(id) {
  const res = await getPool().query(
    `
  DELETE FROM chats
  WHERE id = $1
  RETURNING *
  `,
    [id]
  );
  return res.rows[0];
}
```

> #### Lambda

Create a new Lambda function in `packages/functions/src/name.js`

Get

```js
import { getChats } from "@your-app-name/core/database";

export async function main(event) {
  const chats = await getChats();
  return {
    statusCode: 200,
    body: JSON.stringify({ chats: chats }),
  };
}
```

Post

```js
import { createChat } from "@my-sst-app/core/database";

export async function main(event) {
  const { name } = JSON.parse(event.body); // Get the chat name from the POST body
  const chat = await createChat(name);
  return {
    statusCode: 200,
    body: JSON.stringify({ chat: chat }),
  };
}
```

Delete

```js
import { deleteChat } from "@my-sst-app/core/database";

export async function main(event) {
  const { chatId } = event.pathParameters; // get the chatId from the path parameters
  await deleteChat(chatId);
  return {
    statusCode: 200,
    body: JSON.stringify({}),
  };
}
```

You can access the path parameter with `event.pathParameters.name`

> #### Routes

Set your routes in ApiStack

```js
import { Api, use } from "sst/constructs";

export function ApiStack({ stack, app }) {
  const api = new Api(stack, "Api", {
    defaults: {
      function: {
        environment: {
          DATABASE_URL: process.env.DATABASE_URL,
        },
      },
    },
    routes: {
      "POST /chats": "packages/functions/src/chats/post.main",
      "GET /chats": "packages/functions/src/chats/get.main",
      "DELETE /chats/{chatId}": "packages/functions/src/chats/delete.main",
    },
  });

  stack.addOutputs({
    ApiEndpointNotes: api.url,
  });

  return {
    api,
  };
}
```

## MongoDB

Mongoose is not mandatory to use it.

> #### Addding the API

```js
const api = new Api(stack, "Api", {
  defaults: {
    function: {
      environment: {
        MONGODB_URI: process.env.MONGODB_URI,
      },
    },
  },
  routes: {
    "GET /": "packages/functions/src/lambda.handler",
  },
});
```

> #### Add MongoDB URI

**MongoDB Website > Database > Connect > Connect your application**

Copy and paste the connetion string into `.env` file

```
MONGODB_URI=mongodb+srv://
```

> #### Query Database

```
npm install mongodb mongoose
```

Configure MongoDB Connection (Right above the handler function)

```js
import { MongoClient } from "mongodb";

let cachedDb = null;

async function connectToDatabase() {
  if (cachedDb) {
    return cachedDb;
  }

  const client = await MongoClient.connect(process.env.MONGODB_URI);

  cachedDb = await client.db("db-name");

  return cachedDb;
}
```

Query

```js
export const handler = async (event, context) => {
  context.callbackWaitsForEmptyEventLoop = false;

  const db = await connectToDatabase();

  const result = await db.collection("collection-name").insertOne({ data });

  return result;
};
```

## Mongoose

Configure Mongoose. In case of using mongoose, mongoDB is not necessary.

```js
import mongoose from "mongoose";
import Word from "@wordbook-m/core/models/Word"; // Schema

let cachedDb = null;

async function connectToDatabase() {
  if (cachedDb) {
    return cachedDb;
  }
  cachedDb = await mongoose.connect(process.env.MONGODB_URI);
  return cachedDb;
}
```

Lambda

```js
await connectToDatabase();
const doc = await Word.create({ eng, kor });
```

Schema `core/src/model/Name.js`

```js
import mongoose from "mongoose";

const nameSchema = new mongoose.Schema({
  name: { type: String, required: true },
  created: { type: Date, required: true, default: Date.now },
  obj: { type: mongoose.Schema.Types.ObjectId, ref: "ModelName" },
});

export default mongoose.model("ModelName", nameSchema);
```

## Deploy

Stop local development environment and run the following.

```
npx sst deploy --stage prod
```

Separate environments : Development and Production.

While developing, it doesn't break the production.
