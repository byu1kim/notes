+++
title = "SST"
weight = 5
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## IAM User

## Install AWS CLI

#### Windows

#### Mac

## SST

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
