+++
title = "SST"
weight = 1
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

# IAC Tools

- Terraform : for ec2, not serverless
- Cloud Formation

# Serverless Application Model

# AWS CDK

SST
SST
It is a framework that makes it easy to build full-stack applications on AWS
It will automatically connect to the AWS with your $ aws configure information (created on AWS CLI day)

Setup

```
$ npx create-sst@latest
```

```
cd into the app & npm install
```

Stack
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

### Config File

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

Lambda : packages/functions
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
