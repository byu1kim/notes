+++
title = "Lambda in .NET"
menuTitle = ".NET"
weight = 3
pre = "- &nbsp"
+++

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
