+++
title = "Lambda Function"
menuTitle = "Lambda"
weight = 1
 
pre = "- "
+++

### Lambda

Serverless, event-driven compute service

### Serverless

- Cloud-native development model.
- Applications without having to manage severs. (Pay what you use)

### Lambda Function Setup

- AWS > Lambda > Function > Create function
- Function name : get/post/put etc
- Runtime : Lambda function language (Node.js 18.x)
- Architecture : x86_64 (arm64 is for mobile)

### Lambda Function

- mjs : Moduled Javascript (ES6, same as javascript file with type:module in package.json)
- Hit Deploy > Test
- Every lambda is triggered by something ... here, test btn is the trigger
- API Gateway is one of trigger (it is kind of nginx , accept http request)
- Check console.log : Monitor > CloudWatch

### Lambda in VSC

Configure

```
npm i mysql2
npm init -y : to right our type is module
```

Module [package.json]

```js
"type" : "module"
```

everything (node module, json files) has to be one file, same path as index.js

#### [ index.js ]

[ GET ]

```
import mysql from 'mysql2'

const pool = mysql
.createPool({
host: process.env.MYSQLHOST,
user: process.env.MYSQLUSER,
password: process.env.MYSQLPASSWORD,
database: process.env.MYSQLDATABASE,
port: process.env.MYSQLPORT || 3306,
})
.promise()

export const handler = async(event) => {
const [books] = await pool.query("select \* from books");
const response = {
statusCode: 200,
body: JSON.stringify('Hello from Lambda!'),
};
return response;
};

[ POST, PUT ]
const body = JSON.parse(event.body);
... sql query ...
const book = rows[0]; // TODO implement
const response = {
statusCode: 201,
body: JSON.stringify(book),
};

[ DELETE ]
let id = event?.pathParameters?.id
let intId = parseInt(id)
... sql query ...
const response = {
statusCode: 200,
body: JSON.stringify(books),
};
```

Upload to Lambda
Compress files (not folder)
Back to Lambda > Upload from > .zip file

### Test API

Lambda Test or Postman or
Curl in Terminal :

```
curl --request POST \ -- header "content-type:"application/json"  \ --data '{"name" : "White Chicks}' \ https://url for the post
```

### Lambda in ASP.NET

Install the lambda template for .net

```
dotnet new -i Amazon.Lambda.Templates
```

Create a function

```
dotnet new lambda.EmptyFunction --name MyFunction

Update the profile and region in aws-lambda-tools-defaults.json
  ...
  "profile": "default",
  "region": "us-east-1",
  ...

using Amazon.Lambda.RuntimeSupport;

var handler = (string input, ILambdaContext context) =>
{
  context.Logger.LogInformation("Get Request");
  context.Logger.LogInformation(input);
        var response = new APIGatewayHttpApiV2ProxyResponse
        {
            StatusCode = (int)HttpStatusCode.OK,
            Body = "Hello AWS Serverless",
            Headers = new Dictionary<string, string> { { "Content-Type",
"text/plain" } }
            };

            return response;
};

await LambdaBootstrapBuilder.Create(handler).Build().RunAsync();
```

From inside the function directory, run the following command to install the runtime support package

```
dotnet add package Amazon.Lambda.RuntimeSupport
dotnet add package Amazon.Lambda.APIGatewayEvents
```

Test the function locally

```
dotnet test MyFunction.Tests/MyFunction.Tests.csproj
dotnet tool install -g Amazon.Lambda.Tools
```
